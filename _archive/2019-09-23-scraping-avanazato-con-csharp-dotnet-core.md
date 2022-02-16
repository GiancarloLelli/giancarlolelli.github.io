---
layout: post
title:  "Scraping avanazato con .NET Core"
date:   2019-09-23 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Poco tempo fa, stavo facendo degli esperimenti su un Web Site che avendo un limite sulle API e una procedura di whitelist dell'applicazone abbastanza complicata rendeva molto difficile l'uso di metodi supported per consulare dei dati in modalità read only. Analizzando il sorgente della pagina mi sono accorto che i dati di cui avevo bisogno, venivano salvati in un oggetto Javascript nel corpo della pagina. In maniera molto ingenua, ho aperto un console app .NET Core installato HTMLAgilityPack e ho provato a fare il parse della pagina - risultato: picche. Questo perchè l'oggetto JS veniva popolato solo dopo che la pagina veniva renderizzata in toto. Dopo qualche Googlata ho scoperto che per questi scenari esiste una libreria che mette a disposizione l'engine di Chromium e che simula in tutto e per tutto un browser reale. Di seguito il link al sample di codice di **CefSharp** che mi ha permesso di portare a termine l'esperimento. Ovviamente, dopo aver ottenuto l'HTML completamente renderizzato, ho utilizzato **HTMLAgilityPack** per interrogarli via XPath. [CefSharp.MinimalExample.OffScreen](https://github.com/cefsharp/CefSharp.MinimalExample/blob/master/CefSharp.MinimalExample.OffScreen/Program.cs)
