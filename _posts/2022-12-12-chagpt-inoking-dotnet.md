---
layout: post
title:  "Invocare API di ChatGPT da .NET"
date:   2022-12-12 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
In basso un esempio su come invocare le API di OpenAI per richiedere la classification di un fine-tuning. 
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
