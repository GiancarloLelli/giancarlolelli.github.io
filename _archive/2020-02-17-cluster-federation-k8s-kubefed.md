---
layout: post
title:  "Federazione di cluster K8s con KubeFed"
date:   2020-02-17 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Nel mio lavoro da Solution Architect in Market Unit, mi ritrovo ad esplorare tecnologie e approcci architetturali sempre vari. Da poco, tornando da Roma in treno - un viaggio della speranza di 5 ore - pensavo a come si potesse sfruttare il mondo dell'Hybrid Cloud con appliance come Azure Stack Hub per sviluppare architetture a Microservizi che rimenessero comunque compliant e potessero beneficiare di tutte le potenzialità del Public Cloud.

{: style="text-align: justify"}
A questo proposito, mi sono ricordato il cocnetto di federazione applicabile a cluster K8s e ho pensato: Quanto sarebbe complicato federareun cluster su Azure Stack Hub e uno su Azure Cloud e arrivare a fare dei deployment federati dove i servizi che agiscono e trattano dati stanno "On-Premise" mentre tutto il resto è "On-Cloud"? Fortunatamente la risposta è stata **KubeFed v2** un progetto portto avanti dal SIG di Kubernetes che risponde proprio a questa mia esigenza.

{: style="text-align: justify"}
Il link del repository è [questo](https://github.com/kubernetes-sigs/kubefed) e ho già iniziato a giocarci federando un cluster K8s su Azure Stack Hub e uno su Azure Cloud - il prossimo passo e mettere dentro anche OpenShift ;)