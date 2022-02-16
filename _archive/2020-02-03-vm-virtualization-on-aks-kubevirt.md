---
layout: post
title:  "Virtualizzazione di VM su AKS con KubeVirt.io"
date:   2020-02-03 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Lavorando ad un progetto di offering mi sono imbattuto in quella che è la proposizione di Google per la migrazione di VM su Cloud tramite l'utilizzo dei Container. Il prodotto si chiama **Migrate for Anthos** e in sostanza utilizza KVM e i persistent storage per creare dei POD che utilizzano delle CRI particolare per eseguire su un cluster K8s una VM (ovviamente Linux based). Questo concetto mi ha incuriosito, e ho provato a vedere se è applicabile anche a Azure Kubernetes Service - sfortunatamente, non esiste nulla di pre confezionato ma grazie a questo prodotto [KubeVirt.io](https://kubevirt.io) possiamo, dopo una serie di step di deploy documentati anche per [Azure](https://kubevirt.io/pages/cloud) - avere una soluzione **non production ready** che fa la stessa cosa. E? un progetto davvero interessante e come avrò qualche ora di tempo (lo so lo dico sempre, ma oh) proverò a giocarci.