---
layout: post
title:  "Migrare gli attori di Service Fabric a Dapr"
date:   2020-11-30 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Con tutto questo parlare di Cloud Native e di nuovi framework spesso ci si dimentica chi è stato il precurso di alcuni dei più "classici" e "complicati" modelli di progettazzione. Se pensiamo a Service Fabric i feedback che ho dalle persone è che era "si bello, ma complicato" oppure "si bello, ma un carrozzone" - non so se dargli torto o ragione, io credo che Service Fabric si aun bel pezzo di tecnologia e che se Azure lo usa per gestire e rendere disponibile a noi alcuni dei suoi servizi forse forse è solo questione di saperlo usare.

Detto questo però, tutti sappiamo che Dapr supporta l'Actor model - quindi mi chiedo: Quanto sarebbe complicato migrare una applicazione scritta su Service Fabric nel suo parallelo scritto utilizzando Dapr e il suo actor model? Ci sarebbero dei benefici in merito al lockin della piattaforma, sicuramente la bolletta Azure sarebbe più leggere e perchè no: la nostra applicazione sarebbe di fatto multi-cloud.

E' un tema interessante - forse nelle vacanze di Natale mi cimenterò nella migrazione di una Sample App Service Fabric al suo equivalente in Dapr. Nel mentre: https://github.com/dapr/dotnet-sdk/blob/master/docs/get-started-dapr-actor.md