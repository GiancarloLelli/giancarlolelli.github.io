---
layout: post
title:  "Abilitare un endpoint Open API all'interno delle Azure Functions"
date:   2020-10-05 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Da poco mi sono trovato di fronte alla necessità di avere una Azure Function in grado di rispondere al classico path **/swagger** per permettere la sua esposizione dietro Azure API Management. A quanto pare non esiste una maniera molto semplice per farlo, è infatti necessario creare altre **2** function separate, che si occupano rispettivamente di servire la solita Swagger UI e di fornire il JSON contenente la specifica delle API. Fortunatamento non sono il primo ad affrontare questo problema e su GitHub ho trovato un comodo progetto in grado di supportarci per questo task. L'URL del repository è questo: https://github.com/yuka1984/azure-functions-extensions-swashbuckle e come potete vedere tramite la definizione di un file *SwashBuckleStartup.cs*
```csharp
using System.Reflection;
using AzureFunctions.Extensions.Swashbuckle;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Hosting;
using SampleFunction;

[assembly: WebJobsStartup(typeof(SwashBuckleStartup))]
namespace SampleFunction
{
    internal class SwashBuckleStartup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            //Register the extension
            builder.AddSwashBuckle(Assembly.GetExecutingAssembly());
        }
    }
}
```
e la creazione di 2 function, abbiamo la possibilità di esporre le nostre API Serverless dietro Azure APIM. **Una caratteristica che non ho menzionato prima** è che questo è stato necessario poichè l'istanza di APIM era su una subscription diversa rispetto alla Azure Function, altrimenti grazie all'integrazione nativa di Azure sarebbe stato tutto più semplice.
```csharp
using System.Net.Http;
using System.Threading.Tasks;
using AzureFunctions.Extensions.Swashbuckle;
using AzureFunctions.Extensions.Swashbuckle.Attribute;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;

namespace SampleFunction
{
    public static class SwaggerFunctions
    {
        [SwaggerIgnore]
        [FunctionName("Swagger")]
        public static Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "swagger/json")]
            HttpRequestMessage req,
            [SwashBuckleClient] ISwashBuckleClient swashBuckleClient)
        {
            return Task.FromResult(swashBuckleClient.CreateSwaggerDocumentResponse(req));
        }

        [SwaggerIgnore]
        [FunctionName("SwaggerUi")]
        public static Task<HttpResponseMessage> Run2(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "swagger/ui")]
            HttpRequestMessage req,
            [SwashBuckleClient] ISwashBuckleClient swashBuckleClient)
        {
            return Task.FromResult(swashBuckleClient.CreateSwaggerUIResponse(req, "swagger/json"));
        }
    }
}
```
