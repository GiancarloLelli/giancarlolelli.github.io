---
layout: post
title:  "Confidential AKS Cluster"
date:   2022-10-17 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Oggi ho avuto modo di farle il provisioning del mio primo cluster AKS Confidential - si, mi sto dedicando parecchio a questo tema in ottica di Emerging Technologies - e devo dire che è stato abbastanza semplice. Questo il comando che ho usato:
```powershell
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addons confcom
```
Diverso dal solito è il flag per l'addon confco che di fatto fa l'installazione di tutti i driver e deamonset in grado di rendere mountable le periferiche SGX da manifest YAML di deploy. Per una guida completa sui comandi preparatori e pre requisiti per un cluster AKS confidenziale, è opportuno seguire la guida su [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-enclave-nodes-aks-get-started)