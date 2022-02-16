---
layout: post
title:  "Windows Community Toolkit: Teoria introduttiva"
date:   2018-07-15 9:00:00 +0200
categories: uwp
---
{: style="text-align: justify"}
Volevo dedicare questo post ad un progetto Open Source a cui mi sono interessato sin da subito, e che sin dalla prima release ha riscosso un enorme successo all'interno della community dei UWP Developers. Sto pralndo del UWP Community Toolkit, ora rinominato **Windows Community Toolkit**. Questo toolkit, come suggerisce il nome è mantenuto non solo da Microsoft, ma anche dalla community e rappresenta uno strumento indispensabile per chiunque sia accinga a sviluppare applicazioni tramite la Universal Windows Platform.

{: style="text-align: justify"}
I concetti alla base del toolkit sono semplici, raccogliere in un set di classi e controlli custom tutto quel codice boilerplate che spesso ci troviamo a copiare e incollare da progetto a progetto. L'idea è che se un qualcosa solitamente richiede 10 LOC per essere implementato con il Toolkit invece il numero di LOC deve scendere a 1. Trovo questo approccio molto interessante sopratutto perchè il Windows Community Toolkit contiene alcune classi che forniscono dei wrapper ad API REST che spesso è molto noioso scrivere a mano, ma sopratutto è molto difficile scrivere a T0 in maniera riusabile e facilmente manutenibile.

{: style="text-align: justify"}
Come è logico supporre, il toolkit ha un repository GitHub ufficiale disponibile a [questo](https://github.com/Microsoft/WindowsCommunityToolkit) indirizzo. Nel file README del repo, potete trovare gli SDK di Windows attualmente supportati:
* Creators Update (15063)
* Fall Creators Update (16299)
* April 2018 Update (17134)

{: style="text-align: justify"}
Inoltre, sempre file di README è possibile leggere i nomi di tutti i package NuGet (che ovviamente è il metodo di distribuzione per eccellenza per questo tipo di librerie) che è possibile referenziare. I nomi sono autoesplicativi, però per semplicità di lettura riporto direttamente su questo post la tabella presa disponibile nel README di progetto.

| NuGet Package Name | Description |
| --- | --- |
| Microsoft.Toolkit | .NET Standard NuGet package containing common code |
| Microsoft.Toolkit.Parsers | .NET Standard NuGet package containing cross-platform parsers, such as Markdown and RSS |
| Microsoft.Toolkit.Services | .NET Standard NuGet package containing cross-platform services |
| Microsoft.Toolkit.Uwp | Main NuGet package includes code only helpers such as Colors conversion tool, Storage file handling, a Stream helper class, etc. |
| Microsoft.Toolkit.Uwp.Notifications | Notifications Package - Generate tile, toast, and badge notifications for Windows 10 via code.  Includes intellisense support to avoid having to use the XML syntax. |
| Microsoft.Toolkit.Uwp.Notifications.Javascript | Notification Packages for JavaScript |
| Microsoft.Toolkit.Uwp.Services | Services Package - This NuGet package includes the service helpers for Facebook, LinkedIn, Microsoft Graph, Twitter and more |
| Microsoft.Toolkit.Uwp.UI | UI Packages - Brushes, XAML converters, Visual tree extensions, and other extensions and helpers for your XAML UI. |
| Microsoft.Toolkit.Uwp.UI.Animations | Animations and Composition behaviors such as Blur, Fade, Rotate, etc. |
| Microsoft.Toolkit.Uwp.UI.Controls | XAML Controls such as RadialGauge, RangeSelector, etc. | 
| Microsoft.Toolkit.Uwp.UI.Controls.DataGrid | XAML DataGrid control | 
| Microsoft.Toolkit.Uwp.Connectivity | API helpers such as BluetoothLEHelper and Networking | 
| Microsoft.Toolkit.Uwp.DeveloperTools | XAML user controls and services to help developer building their app | 

{: style="text-align: justify"}
Che dire. Ormai questo toolkit è giunto alla versione 3.0 e direi che dalla mia prima avventura, di cui ho parlato nella serie TecHeroes (potete trovare su [Channel9](https://channel9.msdn.com/Shows/TecHeroes/TecHeroes-UWP-Community-Toolkit) la registrazione) ne è passato di tempo. Tempo che però non solo è servito ad estendere e migliorare il progetto ma anche ad arricchire la sample application che mostra in maniera pratica come utilizzare ogni componente del Toolkit. La sample app è disponibile sullo store a [questo](https://www.microsoft.com/it-it/p/windows-community-toolkit-sample-app/9nblggh4tlcq) indirizzo e come potete vedere dagli screen, ma anche dalla mia intervista il suo utilizzo è diretto e proprio grazie ai pannelli e alla funzionalità di "live editing" è possibile sperimentare il codice e poi riportarlo senza troppi problemi all'interno della nostra app, sia che si parli di codice C# oppure XAML.

{: style="text-align: justify"}
Nell'ultimo mese ho adocchiato un paio di Issue a cui mi piacerebbe contribuire. Chissà se troverò il tempo!!