---
layout: post
title:  "Semplice pipeline di CI/CD per AKS su Azure DevOps"
date:   2018-09-25 9:00:00 +0200
categories: devops
---
{: style="text-align: justify"}
Per [CodeGen 2018](https://giancarlolelli.github.io/talks/2018/09/10/upcoming-conferences-giancarlo-lelli.html) a Verona ho prepararto una demo su AKS e ho avuto la necessità di impostare una semplice pipeline di CI/CD verso AKS integrata con ACR. La cosa super fantastica, è che per mettere su questa pipeline ho usato l'opzione free per i progetti Open Source introdotta con l'annucio di Azure DevOps (potete leggere maggiori dettagli [qui](https://azure.microsoft.com/sv-se/blog/announcing-azure-pipelines-with-unlimited-ci-cd-minutes-for-open-source/)) In basso vi riporto gli step della parte di build, che prevedeva una sola applicazione ASP.NET Core. Il codice della demo e le slide della conferenza potete trovarle sul mio GitHub.  
### Pipeline di build
![](https://raw.githubusercontent.com/GiancarloLelli/giancarlolelli.github.io/master/_posts/images/build-pipeline-aks-codegen2018.PNG?token=AF6HSmMI8kzq-s8pZVqWCWKDNBKgq8KVks5buHDtwA%3D%3D)

{: style="text-align: justify"}
Come potete vedere è abbastanza semplice. Prima facciamo la build della nostra immagine tramite .dockerfile, poi quest'immagine viene pushata nel nostro Azure Container Registry e infine vengono pubblicati gli artifacts. Nel nostro caso l'artifact è uno ed è solo il file .yml che viene poi dato in pasto a **kubectl**.

### Pipeline di release
![](https://raw.githubusercontent.com/GiancarloLelli/giancarlolelli.github.io/master/_posts/images/reelase-pipeline-aks-codegen2018.PNG?token=AF6HSuJmyIqt4GAdTdtLUfoOhEu-LxpRks5buHFlwA%3D%3D)

{: style="text-align: justify"}
Anche la pipeline di release è molto semplice. Una volta pubblicato il nostro artifact diamo in pasto il file YAML (che ho riportato in [quest'altro](https://giancarlolelli.github.io/development/2018/09/17/aks-acr-integration-kubectl.html) post) a **kubectl** - una volta processato e quindi creato il nostro **deployment** andiamo a sostituire i POD creati con una precedente release della nostra applicazione con quelli contenuti nell nuova immagine. Questo è fatto nello step di update tramite questo comando
```powershell
kubectl set image deployment/codegen codegen=<registry>.azurecr.io/codegencore:$(Build.BuildNumber)
```
