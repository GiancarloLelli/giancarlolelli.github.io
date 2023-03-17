---
layout: post
title:  ".NET 6 è stato rilasciato"
date:   2021-11-15 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Grande settimana quella passata. Essendosi tenuta la .NET Conf 2021 è stato ufficialmente rilasciato **.NET 6** insieme a tutte le relative nuove versione dei prodotti *correlati* quali Entity Framework e ASP.NET Core. E' senza dubio una versione importante, la nuova LTS su cui da best practice è opportuno basare tutti i nuovi sviluppi (e perchè no, migrare gli esistenti) - ovviamente, non è tutto :) anche Visual Studio 2022 ha avuto il su omomento di gloria infatti anche la nuova versione dell'IDE è disponibile e in GA. Per i più pigri vi riporto in basso (citando direttamente il blog post ufficiale) le key feature introdotte dalla nuova versione di .NET 6:

## Official blog post: [Announcing .NET 6 — The Fastest .NET Yet](https://devblogs.microsoft.com/dotnet/announcing-net-6/)

## .NET 6 Highlights:

* Production stress-tested with Microsoft services, cloud apps run by other companies, and open source projects.
* Supported for three years as the latest long term support (LTS) release.
* Unified platform across browser, cloud, desktop, IoT, and mobile apps, all using the same .NET Libraries and the ability to share code easily.
* Performance is greatly improved across the board and for file I/O in particular, which together result in decreased execution time, latency, and memory use.
* C# 10 offers language improvements such as record structs, implicit using, and new lambda capabilities, while the compiler adds incremental source generators. F# 6 adds new features including Task based async, pipeline debugging and numerous performance improvements.
* Visual Basic has improvements in the Visual Studio experience and for Windows Forms project open experience.
* Hot Reload enables you to skip rebuilding and restarting your app to view a new change — while your app is running — supported in Visual Studio 2022 and from the .NET CLI, for C# and Visual Basic.
* Cloud diagnostics have been improved with OpenTelemetry and dotnet monitor, which is now supported in production and available with Azure App Service.
* JSON APIs are more capable and have higher performance with a source generator for the serializer.
* Minimal APIs introduced in ASP.NET Core to simplify the getting started experience and improve the performance of HTTP services.
* Blazor components can now be rendered from JavaScript and integrated with existing JavaScript based apps.
* WebAssembly AOT compilation for Blazor WebAssembly (Wasm) apps, as well as support for runtime relinking and native dependencies.
* Single-page apps built with ASP.NET Core now use a more flexible pattern that can be used with Angular, React, and other popular frontend JavaScript frameworks.
* HTTP/3 has been added so that ASP.NET Core, HttpClient, and gRPC can all interact with HTTP/3 clients and servers.
* File IO now has support for symbolic links and has greatly improved performance with a re-written-from-scratch FileStream.
* Security has been improved with support for OpenSSL 3, the ChaCha20Poly1305 encryption scheme, and runtime defense-in-depth mitigations, specifically W^X and CET.
* Single-file apps (extraction-free) can be published for Linux, macOS, and Windows (previously only Linux).
* IL trimming is now more capable and effective, with new warnings and analyzers to ensure correct final results.
* Source generators and analyzers have been added that help you produce better, safer, and higher performance code.
* Source build enables organizations like Red Hat to build .NET from source and offer their own builds to their users.