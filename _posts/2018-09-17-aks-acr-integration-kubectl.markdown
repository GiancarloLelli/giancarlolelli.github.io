---
layout: post
title:  "Integrazione con AKS e ACR"
date:   2018-09-17 9:00:00 +0200
categories: development
---
{: style="text-align: justify"}
L'altro giorno, 22 Settembre, sono stato speaker a CodeGen 2018 - ho anche scritto un post [qui](https://giancarlolelli.github.io/talks/2018/09/10/upcoming-conferences-giancarlo-lelli.html) con la lista delle prossime conferenze a cui parteciperò. Come parte della mia demo su AKS (Azure Kubernetes Service) ho fatto vedere molto brevemente il modo in cui è possibile registrare un secret all'interno del cluster K8s che contiene le credenziali necessarie al pull di una image da un repository privato, in questo caso ACR (Azure Container Registry).
  
Per mettere in piedi questa integrazione sono necessarie fondamentalmente 2 cose:
* La K8s CLI (kubectl) configurata sul contesto del nostro cluster AKS
* I dati di autenticazione di ACR

Una volta che siamo in possesso di questi due pre requisiti ci basterà digitare il seguente comando, anche da powershell, per caricare il secret su AKS.  
```powershell
kubectl create secret docker-registry <secret_name> --docker-server='<registryname>.azurecr.io' --docker-username='<user>' --docker-password='<password>' --docker-email='Service@AzurePrincipal.com'
```
Notate come per il parametro email specifichiamo un indirizzo "fittizio" - altro dettaglio importante è il nome del *secret* che dovrà essere lo stesso che andremo poi a specificare nel file **yml** che la nostra eventuale pipeline di CI/CD darà in pasto ad AKS. Ecco in basso un esempio di file YAML che utilizza un secret per scaricare l'immagine dal mio ACR.
```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: codegen
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: codegen
    spec:
      containers:
      - name: codegen
        image: cicenter.azurecr.io/codegencore:$(Build.BuildNumber)
        env:
        - name: ASPNETCORE_ENVIRONMENT
          value: "Production"
        imagePullPolicy: Always
        ports:
        - containerPort: 80
      imagePullSecrets:
      - name: acr-auth
```
