---
layout: post
title:  "Client certificates on Azure"
date:   2021-11-02 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Settimana scorsa mi sono trovato a risolvere un problema che attanagliava un bel pò di colleghi in azienda. A quanto pare non riuscivano a fare funzionare una chiamata REST verso SAP utilizzato un certificato Client come metodo di signing della richiesta. Leggendo il thread a cui sono stato aggiunto si parla di quasi 6 giorni di tentativi e sbattimenti vari.

Reso in mano il problema, all'inizio anche io ho avuto qualche difficiltà ma poi sono riuscito a venire a capo. La soluzione? attivare a livello di App Service (della Azure Function in questione) il flag **WEBSITE_LOAD_CERTIFICATES** dando come valore 1 e utilizzare la classe **X509Store** per aggiungere al certificate store dell'App Service tutta la catena di certificati necessaria ad avere una corretta validation chain.

Unica ecca della soluzione? Nonostante sia .NET Core gira solo su Windows a causa di una serie di specificaità di come è necessario accedere all'X509Store.