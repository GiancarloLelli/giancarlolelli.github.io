---
layout: post
title:  "Usare ASP.NET Core e Azure per creare un Flash Briefing JSON feed per Alexa"
date:   2018-11-19 9:00:00 +0200
categories: howto
---
{: style="text-align: justify"}
Per partecipare alla promozione dedicata agli sviluppatori *Amazon* - ovvero coloro che inviavano una skill per Alexa entro Novembre - ho creato una semplice flah briefing skill per il feed della facoltà di informatica dell'Uuniversità di Cagliari. Questa skill è al momento in certificazione, ma il codice è disponibile su GitHub a [questo](https://github.com/GiancarloLelli/alexaunicafeedwrapper) indirizzo. 

{: style="text-align: justify"}
Data la semplicità del codice, e per comodità di lettura, vi incollo lo snippet che ho usato per wrappare il feed XML del sito in un formato JSON *Alexa compliant*. Di seguito il codice:
```csharp
using Microsoft.AspNetCore.Mvc;
using System;
using System.Collections.Generic;
using System.Linq;
using System.ServiceModel.Syndication;
using System.Xml;

namespace Alexa.Web.Controllers
{
    [Route("api/feed/[controller]")]
    public class AlexaController : Controller
    {
        [HttpGet]
        public IActionResult Get()
        {
            var feed = new AlexaFeed();

            using (var xmlReader = XmlReader.Create("http://corsi.unica.it/informatica/feed/"))
            {
                var reader = SyndicationFeed.Load(xmlReader);

                foreach (var originalItem in reader.Items)
                {
                    var copy = new AlexaFeedItem
                    {
                        redirectionUrl = originalItem.Links[0].Uri.ToString(),
                        titleText = originalItem.Title.Text,
                        mainText = originalItem.Title.Text,
                        updateDate = originalItem.PublishDate.DateTime.ToString("yyyy-MM-ddTHH:mm:ss.0Z"),
                        uid = Guid.NewGuid().ToString()
                    };

                    feed.Items.Add(copy);
                }
            }

            return Json(feed.Items.OrderByDescending(y => y.updateDate));
        }
    }

    public class AlexaFeed
    {
        public AlexaFeed()
        {
            Items = new List<AlexaFeedItem>();
        }

        public IList<AlexaFeedItem> Items { get; set; }
    }

    public class AlexaFeedItem
    {
        public string uid { get; set; }
        public string updateDate { get; set; }
        public string titleText { get; set; }
        public string mainText { get; set; }
        public string redirectionUrl { get; set; }
    }
}
```
Tralasciando quanto grezzo può essere il codice - il concetto è che mettere su un feed per Alexa è molto facile. L'importanto è rispettare lo schema JSON definito dalla documentazione o riportato nella nostra classe **AlexaFeedItem** e il date format che Alexa usa per individuare gli item più recenti e raccontarceli a voce. 