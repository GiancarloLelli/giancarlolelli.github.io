---
layout: post
title:  "Condivisione del codice tra ASP.NET e ASP.NET Core"
date:   2022-03-08 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Pochi giorni fa è uscito un articolo molto interessante sul developer blog di .NET che credo valga la pena sottolineare/evidenziare. Per molto può sembrare un concetto banale, ma personalmente mi piace vedere degli how-to pratici e sopratutto chiari che coprono tematiche collegata al mondo della migrazione e/o modernizzazione applicativa. L'articolo in questione è il seguente [Sharing code between ASP.NET and ASP.NET Core](https://devblogs.microsoft.com/dotnet/sharing-code-between-aspnet-and-aspnetcore/) - questo l'estratto del wrapup (per i contenuti vi conviene leggere il post direttamente)

> The ability to share code also includes static content like CSS, JavaScript and images. Step-by-step you can build flexibility into your web app today to make your migration to ASP.NET Core easier.
> If you would like more detailed guidance to migrate the entire `ShoppingCartController.cs` you can follow a full walkthrough with samples at [MvcMusicStoreMigration](https://aka.ms/AAf5wkc). The walkthrough will also demonstrate how you can run both ASP.NET and ASP.NET Core from the same IIS Application Pool to incrementally migrate your web app one controller at a time.
> For those planning to start work on their ASP.NET Core migration we’ll share a few more tips.
> * Upgrade your NuGet packages so you can use netstandard.
> * Change your class libraries to netstandard so you can share code between ASP.NET and ASP.NET Core.
> * Find references to System.Web in your class libraries build interfaces replace them. Use dependency injection so you can easily switch between ASP.NET and ASP.NET Core features.

E non dimentichiamoci della guida completa disponibile al link [Migrate from ASP.NET to ASP.NET Core](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/)