---
layout: post
title:  "CLI Reference per ChatGPT / OpenAI"
date:   2022-12-05 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Con l'uscita di ChatGPT e il periodo pre-Natalizio ho esplorato la suite di OpenAI cercando di capire come era possibile personalizzare uno dei modelli multi milionari su un training set generato da me e inerente a delle richieste di supporto di utenti verso il reparto IT. L'idea era quella di provare a replicare un caso "banale" di Intelligent Operation & Automation. Mi sento di dire che sono soddisfatto del risultato. Riporto di seguito il codice utilizzato per la generazione del training set e i comandi della CLI usati per attivare il fine-tuning del modello OpenAI

> Fine-tuning lets you get more out of the models available through the API by providing. GPT-3 has been pre-trained on a vast amount of text from the open internet. When given a prompt with just a few examples, it can often intuit what task you are trying to perform and generate a plausible completion. This is often called "few-shot learning." Fine-tuning improves on few-shot learning by training on many more examples than can fit in the prompt, letting you achieve better results on a wide number of tasks. Once a model has been fine-tuned, you won't need to provide examples in the prompt anymore. This saves costs and enables lower-latency requests.

```csharp
using System.Text;

using ChatGPT.Dataset.Permutation.Wordlist;

namespace ChatGPT.Dataset.Permutation
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var access_problems = new AccessProblemsWordlists();
            var password_reset = new PasswordResetWordlists();

            var list_container_access = new List<List<string>> 
            { 
                access_problems.Subjects, 
                access_problems.Situation, 
                access_problems.Verb, 
                access_problems.What, 
                access_problems.CallToAction 
            };

            var list_container_password = new List<List<string>> 
            { 
                password_reset.Subjects, 
                password_reset.Situation, 
                password_reset.Verb, 
                password_reset.What, 
                password_reset.CallToAction 
            };

            WriteAllCombinations(list_container_access, string.Empty, 0);
            WriteAllCombinations(list_container_password, string.Empty, 0);

            string[] lines = File.ReadAllLines("training_data.csv");
            File.WriteAllLines("training_data_unique.csv", lines.Distinct().ToArray());
        }

        static void WriteAllCombinations(List<List<string>> word_lists, string line, int n) 
        {
            if(word_lists.Count == 0)
            {
                return;
            }

            if(n == word_lists.Count)
            {
                Console.WriteLine(line);
                File.AppendAllText("training_data.csv", string.Concat(line, Environment.NewLine));
                return;
            }

            int m = word_lists[n].Count;

            for (int i = 0; i < m; i++)
            {
                var document_line = string.Concat(line, " ", word_lists.ElementAt(n).ElementAt(i).ToString()).Trim();
                WriteAllCombinations(word_lists, document_line, n + 1);
            }
        }
    }
}
```
Questi invece i comandi della CLI di OpenAI per la creazione del fine tuning
```powershell
> openai tools fine_tunes.prepare_data -f training_data_unique.csv
> openai api fine_tunes.create -t "training_data_unique_prepared.jsonl" -m ada
> openai api fine_tunes.follow -i ft-5xZ97TCXyACvLdza8Xn9cRJv
```

Questo il codice per la chiamata al servizio OpenAI sfruttando il nostro modello Fine Tuned
```csharp
using System.Text;
using System.Text.Json;
using System.Text.Json.Nodes;

namespace ChatGPT.Completion
{
    internal class Program
    {
        static async Task Main(string[] args)
        {
            var url = "https://api.openai.com";
            var api_key = Environment.GetEnvironmentVariable("OPENAI_API_KEY");

            var client = new HttpClient();
            client.BaseAddress = new Uri(url, UriKind.Absolute);
            client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", $"Bearer {api_key}");

            var ms = new MemoryStream();
            await JsonSerializer.SerializeAsync<OpenAPICompletionModel>(ms, new OpenAPICompletionModel());
            ms.Seek(0, SeekOrigin.Begin);

            var reader = new StreamReader(ms);
            var payload = await reader.ReadToEndAsync();

            var response = await client.PostAsync("/v1/completions", new StringContent(payload, Encoding.UTF8, "application/json"));
            var content = await response.Content.ReadAsStringAsync();

            Console.WriteLine($"Prompt Sent: {new OpenAPICompletionModel().prompt}\n");
            Console.WriteLine($"Response JSON: {content}");

            Console.ReadLine();
        }
    }

    internal class OpenAPICompletionModel
    {
        public string model { get { return "ada:ft-personal-2023-01-05-14-36-08"; } }
        public string prompt { get { return "Non riesco ad accedere all'applicazione perfavore controllate###"; } }
        public int max_tokens { get { return 1; } }
        public int logprobs { get { return 2; } }
        public int temperature { get { return 0; } }
    }
}
```
