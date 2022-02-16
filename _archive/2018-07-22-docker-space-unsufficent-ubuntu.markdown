---
layout: post
title:  "Spazio insufficente per avviare il deamon Docker su Ubuntu"
date:   2018-07-22 9:00:00 +0200
categories: docker
---
{: style="text-align: justify"}
L'altro giorno a lavoro, prima di una demo molto importante mi sono accordo che il enviroment Azure su cui dimostro le capacità di modernizzazione di app tradizionali tramite Docker era giù - dopo una più attenta analisi ho scoperto che era giù perchè il nodo master non riusciva a far partire il deamon di Docker in quanto il disco della VM era pieno. E' stato un bel problema da risolvere, dato che non potendo avviare il deamon non potevo eseguire il comando
```dockerfile
docker system prune
```
{: style="text-align: justify"}
Fortunatamente per me, mi sono imbattuo in questo troubleshoot sul sito ufficiale di Docker che mi ha mostrato come resettare i log dei container manualmente e liberare circa il 50% del disco. I comandi sono i seguenti
```bash
du -d1 -h /var/lib/docker/containers | sort -h
```
{: style="text-align: justify"}
Questo comando ordina la cartella */var/lib/docker/containers* in moda da capire chi ha il log che occupa più spazion
```bash
cat /dev/null > /var/lib/docker/containers/<container_id>/<log_name>-json.log
```
{: style="text-align: justify"}
Mentre invece quest'ultimo, dato il container ID e il nome del log (info leggibili a seguito del primo comando) permette di azzerare il file che abbiamo individuato, ovvero quello con grandezza maggiore.

{: style="text-align: justify"}
Una volta fatto questo il mio cluster Docker EE è tornato su in quanto il daemon riusciva a partire senza problemi.

*[Reference](https://success.docker.com/article/no-space-left-on-device-error)