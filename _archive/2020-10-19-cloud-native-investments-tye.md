---
layout: post
title:  "Cloud Native Investments: project Tye"
date:   2020-10-19 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Nel mio catch up tecnologico della scorsa settimana, ho avuto modo di vedere lo status di alcuni progetti che sino a qualche mese fa sembravano solo dei *garage project* di qualche dipendente di Microsoft. Nello specifico mi riferisco a Project Tye - è stato infatti pubblicizzato e mostrato quasi all'inverso simile sia nella keynote che tramite una sessione completamente dedicata a lui. éer chi non lo conoscesse:
> Project Tye is an experimental developer tool that makes developing, testing, and deploying microservices and distributed applications easier.

Il suo placement nella nostra toolchain di sviluppo, è paragonabile a quello di Azure DevSpaces, con l'unica differenza che questo è completamente offline, e direi anche meno adatto a progetti/soluzioni di alta complessità. E' possibile utilizzarlo da linea di comando una volta installato da command line il .NET Global Tool associato:
```powershell
dotnet tool install -g Microsoft.Tye --version "0.2.0-alpha.20258.3"
```
Per chi volesse saperne di più il team a suo tempo ha confezionato un ottimo blog post di lancio disponibile a [questo](https://devblogs.microsoft.com/aspnet/introducing-project-tye/) URL e come al solito, le informazioni più fresche invece le trovate su [GitHub](https://github.com/dotnet/tye)