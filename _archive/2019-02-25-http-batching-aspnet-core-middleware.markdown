---
layout: post
title:  "HTTP Request Batching in ASP.NET Core"
date:   2019-02-25 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
L'altro giorno mi sono imbattuto in un post che mi ha portato ad approfondire il concetto di batching delle request HTTP - ovvero la possibilità che ha un cliet di confezionare un particolare tipo di richiesta, con un content type particolare che non fa altro che raggruppare altre *n* richieste HTTP che vengono poi inviate in massa ad un endpoint dedicato di un servizio *X*. Il concetto mi è piaciuto molto e ho provato a capire come questo concetto si poteva declinare in ASP.NET Core. A detta di [questa](https://github.com/aspnet/AspNetCore/issues/6000) issue purtroppo non sembrava possibile.

{: style="text-align: justify"}
Tuttaia, dopo qualche oretta passata a esplorare del codice su GitHub e su internet sono riuscito a mettere su una implementazione di questa feature su ASp.NET Core utilizzando degli ingredienti molto semplici:
* Un middleware
* Un Action Executor custom

{: style="text-align: justify"}
Sto ancora cercando di capire se le performance di questo codice siano accettabili, ma come al solito ho messo tutto su GitHub a questo indirizzo: https://github.com/GiancarloLelli/aspnetbatchingmiddleware. Per i più impazienti in basso riporto uno snippet contente il codice del Middleware:
```csharp
using GL.Sdk.Http.Batching.Configuration;
using GL.Sdk.Http.Batching.Extensions;
using GL.Sdk.Http.Batching.Mvc;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.WebUtilities;
using Microsoft.Extensions.Options;
using Microsoft.Net.Http.Headers;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace GL.Sdk.Http.Batching
{
    public class HttpBatchingMiddleware
    {
        private const int BATCH_SIZE = 100;
        private const int BOUNDARY_LIMIT = 70;
        private const string SUBTYPE = "batch";
        private const string CONTENT_TYPE = "multipart/batch";

        private readonly RequestDelegate _next;
        private readonly HttpBatchingMiddlewareOptions _options;

        public HttpBatchingMiddleware(RequestDelegate next, IOptions<HttpBatchingMiddlewareOptions> options)
        {
            _next = next;
            _options = options.Value;
        }

        public async Task InvokeAsync(HttpContext context)
        {
            var contentType = context.Request.ContentType ?? string.Empty;
            var isBatchedContentType = contentType.Contains(CONTENT_TYPE);

            if (!isBatchedContentType)
            {
                await _next.Invoke(context);
            }
            else
            {
                // Start a multipart container 
                var outerContent = new MultipartContent(SUBTYPE);

                // Read the multipart request
                var mediaTypeHeaderValue = MediaTypeHeaderValue.Parse(contentType);
                var boundary = HeaderUtilities.RemoveQuotes(mediaTypeHeaderValue.Boundary);

                if (boundary.Length <= BOUNDARY_LIMIT)
                {
                    var reader = new MultipartReader(boundary.Value, context.Request.Body);
                    var section = await reader.ReadNextSectionAsync();
                    var requestMessageCollection = new List<HttpRequestMessage>();

                    // Section to HTTP Request conversion
                    while (section != null)
                    {
                        using (var bodyreader = new StreamReader(section.Body))
                        {
                            var rawRequest = await bodyreader.ReadToEndAsync();
                            var request = rawRequest.ReadAsHttpRequestMessage();
                            requestMessageCollection.Add(request);
                        }

                        section = await reader.ReadNextSectionAsync();
                    }

                    // Parallel execution
                    var tasks = new List<Task<HttpResponseMessage>>();
                    requestMessageCollection.ForEach(x => tasks.Add(_options.Client.SendAsync(x, CancellationToken.None)));
                    (await Task.WhenAll(tasks)).ToList().ForEach(y => outerContent.Add(new HttpMessageContent(y)));

                    // Return the multipart response to caller
                    await context.ServeMultipartContent(new MultipartContentResult(outerContent));

                    return;
                }

                await _next.Invoke(context);
            }
        }
    }
}
```
{: style="text-align: justify"}
Ho chiesto un feedback al team di ASP.NET su questa mia implementazione, sono curioso di sapere *se* e *cosa* mi risponderanno.