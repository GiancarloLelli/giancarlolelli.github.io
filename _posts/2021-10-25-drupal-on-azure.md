---
layout: post
title:  "Drupal on Azure"
date:   2021-10-25 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Questa settimana (la scorsa of course) mi sono trovato davanti al task di installare Drupal on Azure. Onestamente pensavo fosse una cose semplice, un wizard e via...invece ho sbattuto la testa contro i più strani errori relativi al download dei CSS e a comportamente strani tra HTTP/HTTPS. In sostanza l'installazione era containerizzata ma persistita in toto direi su Azure Files - questo per qualche motivo crea problemi con l'immagine standard di Bitnami (dall'Azure Maketplace) e solo dopo aver disabilitato il boundling automatico dei file statici sono riuscito a risolvere il problema.

Sono abbastanza soddisfatto, si tratta di un progetto cotto e mangiato quindi per Natale dovrei riuscire anche a provarlo una volta rilasciato in produzione :)