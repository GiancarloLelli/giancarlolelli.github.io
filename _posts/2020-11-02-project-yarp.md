---
layout: post
title:  "Project Yarp: Yet Another Reverse Proxy"
date:   2020-11-02 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Nella mia giornata di allineamento tecnologico ho avuto modo di aggiornarmi su quelle che è un nuovo side project (ormai diventato un vero e proprio Cloud Native Investment da parte di Microsoft) che si chiama **YARP** acronimo di *Yet Another Reverse Proxy* - lo scopo di YARP è quello di creare un reverse proxy server, questo nasce dalla necessità e anche dal fatto che molti team in Microsoft finivano sempre per crearsi la loro versione o venivano ingaggiata per costruirne uno from scratch. Il buon senso ha portato a convergere e centralizzare gli sviluppi in un progetto che fosse moderno e malleabile.

YARP è basato su ASP.NET Core e .NET, quindi adotta il best of breed di entrambi i mondi e ovviamente è **veloce**. L'idea del team è fornire YARP come class library o project template di Visual Studio. Ovviamente il progetto è su GitHub ed è open for contribution: https://github.com/microsoft/reverse-proxy

A questo indirizzo è disponibile la documentazione del tool che ne spiega la configurazione e anche il suo utilizzo: https://microsoft.github.io/reverse-proxy/articles/getting_started.html