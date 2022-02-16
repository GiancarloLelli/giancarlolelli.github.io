---
layout: post
title:  "Docker Zap Utility per liberare spazio su disco"
date:   2020-03-30 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Il mio laptop di lavoro è un classico laptop da consulente: storage limitato, RAM quanto basta e CPU che consuma poco - diciamo che questa combinazione quando sperimento con Docker e i container in generale non è il massimo. Il primo problema è lo spazio su Disco, ormai vivo con lo storage full al 90% e non c'è margine di miglioramento. Fortunatamente però nel posti di oggi vi segnalo l'utility **Docker ZAP** che per cho come me ha problemi di storage è una manna dal cielo in quanto permette di nukare le folder *senza pensarci due volte* con le varie cache delle immagini e altre schifezze e liberare uno *shitton* di spazio. Trovate l'utility [qua](https://github.com/moby/docker-ci-zap) e in basso un esempio sul suo utilizzo:
```powershell
.\docker-ci-zap.exe -folder "C:\ProgramData\docker"
```
