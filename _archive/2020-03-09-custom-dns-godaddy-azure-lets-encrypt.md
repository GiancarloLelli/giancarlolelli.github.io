---
layout: post
title:  "Utilizzare Let'Encrypt con GoDaddy + Azure App Service"
date:   2020-03-09 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Da poco ho messo su un progetto personale su Azure e h oavuto la necessità di configurare Let's Encrypt per avere un certificato SSL gratis. Non avevo mai avuto la possibilità di provare questo servizio e devo dire che l'integrazione con Azure grazie al meccanismo delle extensions è davvero comodo, un paio di click e il tuo sito è in HTTPS. Tuttavia, ho dovuto tribolare un pochettino per far terminare con successo il wizard di configurazione a causa di un settaggio DNS che il mio servizio di registrazione **GoDaddy** applica di default.

{: style="text-align: justify"}
Come sapete Azure per verificare la titolarità di un dominio richiede la creazione di un record A e uno TXT - il problema nasce perchè GoDaddy crea automaticamente un'altro record TXT che punta al suo servizio di redirect, questo per permettere di reindirizzare tutti i domini che non sono associati a nulla. Beh questo settaggio pare un edge case non gestito dal wizard di Let's Encrypt infatti finchè non **cancellate** quel record verrà preso in considerazione dal processo di wizard facendolo fallire.