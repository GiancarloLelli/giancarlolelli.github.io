---
layout: post
title:  "Instagram Image Scraping con .NET Core"
date:   2019-01-21 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
L'altro giorno mi stavo annoiando e per combattere la sensazione di noia, ho provato a vedere se con una semplice console app in .NET Core sarei riuscito a sviluppari una utility che mi permettesse di scaicare le immagini da Instagram. Con un pò di sorpresa, è stata una impresa abbastanza semplice. Ho condiviso il codice su GitHub [qui](https://github.com/GiancarloLelli/igdownloader) ma vediamo di commentarlo brevemente in questo post.
```csharp
using (var client = new HttpClient())
{
    var page = await client.GetAsync(igUrl);
    var html = await page.Content.ReadAsStringAsync();

    // HTML Query
    var pageDocument = new HtmlDocument();
    pageDocument.LoadHtml(html);
    var element = pageDocument.DocumentNode.SelectSingleNode("(//meta[contains(@property,'og:image')])[1]");

    if (element != null)
    {
        var imageUrl = element.Attributes["content"].Value;
        var uri = new Uri(imageUrl);

        var imageStream = await client.GetStreamAsync(uri);
        using (var fileStream = new FileStream(uri.Segments.Last(), FileMode.Create, FileAccess.Write))
        {
            imageStream.CopyTo(fileStream);
        }
    }
}
```
{: style="text-align: justify"}
Il succo di tutto è la libreria **HtmlAgilityPack** - famosissima libreria (disponibile su NuGet) che permette di scorrere gli elementi del DOm di una pagina web con delle query XPath. Come potete vedere, una volta scaricata la pagina, andiamo a leggere un meta tag denominato *og:image* e sfruttiamo il contenuto della sua attributo **Content** per ottenere l'URL dell'immagine che vogliamo salvare sul nostro PC. Easy no? :)

### Disclaimer
{: style="text-align: justify"}
**Le immagini su Instagram devono sempre e comunque ritenersi di proprietà dell'account che le posta, o per lo meno di chi su Instagram decide di postare quell'immagine come stato. Utilizzare in maniera impropria tecniche di questo tipo - per impossessarsi e ridistribuire in maniera non autorizzata questo tipo di contenuti - è una pratica illegale e passibile di multe e/o sanzioni. E io non mi assumo nessuna responsabilità di come usere questo snippet di codice**