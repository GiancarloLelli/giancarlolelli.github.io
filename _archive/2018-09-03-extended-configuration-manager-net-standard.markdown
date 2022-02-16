---
layout: post
title:  "Nuova release: Extended Confiuration Manager 2.0"
date:   2018-09-03 9:00:00 +0200
categories: project
---
{: style="text-align: justify"}
Recentemente ho avuto il tempo di rimettere mano ad una piccola libreria che ho sviluppato quando utilizzando le funzionalità dei *secrets* di ASP.NET Core mi sono appassionato al concetto di poter caricare dei setting da una sorgente che non fosse il file appsettings.json / Web.config dell'applicazione ma invece fosse un qualche data source o JSON remoto. Questo approccio credo sia molto utile anche in applicazioni di tipo console - o comunque non solo web app.

{: style="text-align: justify"}
Per questo motivo, ho esplorato il concetto di estensione della famosa classe **ConfigurationManager** - il primo blocker è stato il fatto che questa classe come tutti sappiamo è statica quindi non è possibile creare degli extension methods. Per ovviare a questo problema ho creato una sorta di wrapper di questa classe, non statico ma che fa da proxy a tutti i metodi del ConfigurationManager classico ma che adesso può appunto aggiungere di altri.

{: style="text-align: justify"}
Gli extension method che ho aggiunto permettono di:
* Aggiungere dei settings da un file JSON statico
* Aggiungere dei settings da un endpoint REST pubblico
* Aggiungere dei settings da un endpoint REST protetto (autenticazione OAuth con Token)

{: style="text-align: justify"}
La libreria è molto semplice, un esempio del suo utilizzo può essere visto nello snippet qua sotto.
```csharp
using GL.Console.Secrets;
using GL.Console.Secrets.Extensions;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;

namespace GL.App
{
    class Program
    {
        static void Main(string[] args)
        {
            var ext = new ExtendedConfigurationManager();
            ext.AddSecretsFromEndpoint("https://jsonplaceholder.typicode.com/posts/1");
            ext.AddSecretsFromJson("secretsTest.json");
            ext.AddSecretFromProtectedEndpointAsync("https://jsonplaceholder.typicode.com/posts/1", GetToken, ParseResponseMessage);

            // All the settings are now in the Secret collection
            System.Console.WriteLine($"Settings: {ext.Secrets["title"]}");
        }

        static async Task<string> GetToken()
            => await Task.FromResult("Bearer: OAuth20_Token");

        static async Task<Dictionary<string, string>> ParseResponseMessage(HttpResponseMessage response)
            => await Task.FromResult(new Dictionary<string, string> { { "SecretKey", "SecretValue" } });
    }
}
```
{: style="text-align: justify"}
La libreria è basata su **.NET Standard 2.0** e purtroppo, a causa di una serie di problemi con i binsing redirects noti, ha come dipendenza il pacchetto NuGet **System.Configuration.ConfigurationManager** - questa libreria è stata testata su una console application che aveva come target il **.NET Framework 4.7.2**

{: style="text-align: justify"}
Ho ovviamente pubblicato il tutto su NuGet a [questo](https://www.nuget.org/packages/GL.Console.Secrets) indirizzo mentre invece il repository ufficiale, fornito in licenza MIT, è disponibile su GitHub [qua](https://github.com/GiancarloLelli/secrets).