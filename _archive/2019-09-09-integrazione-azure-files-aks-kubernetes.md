---
layout: post
title:  "Utilizzare Azure Files come volumi su AKS"
date:   2019-09-09 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Nei mesi di pausa sul blog ho lavorato ad una PoC per una azienda dell'industria Telco che voleva valutare l'effort di migrazione della loro piattaforma di videoconferencing attualmente ospitata su VMWare vSphere ad Azure Kubernetes Service. E' stata una PoC interessante, abbastanza lineare ma questa parte è stata quella che anche nei miei esperimenti non avevo mai misto. Sostanzialmente gli step per questa integrazione sono 2:
* Creazione dell'Azure Storage nella propria sottoscrizione
* Aggiugnere un secret su AKS per la gestione del token di Azure Storage
* Definire il volume del manifest di deployment del nostro POD

{: style="text-align: justify"}
Ecco di seguito lo snippet per la creazione del secret su AKS tramite CLI
```yaml
# kubectl create -f aks-fileshare-secret.yml
apiVersion: v1
kind: Secret
metadata:
  name: azshare-secret
type: Opaque
data:
  azurestorageaccountname: <account_name>
  azurestorageaccountkey: <secret>
```
{: style="text-align: justify"}
I valori per l'account name e il secret sono codificati in base64, lo script Powershell per ottenerli è il seguente:
```powershell
$accountName = "myStorageAccount"
$accountNameBytes = [System.Text.Encoding]::UTF8.GetBytes($accountName)
$accountNameBase64 = [Convert]::ToBase64String($accountNameBytes)
Write-Host "Account Name Base 64: " $accountNameBase64

$accountKey = "mySecretKey"
$accountKeyBytes = [System.Text.Encoding]::UTF8.GetBytes($accountKey)
$accountKeyBase64 = [Convert]::ToBase64String($accountKeyBytes)
Write-Host "Account Name Key 64: " $accountKeyBase64
```
{: style="text-align: justify"}
Una volta creato il secret, possiamo usarlo nel nostro manifest in questo modo (esempio di manifest per MongoDB):
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: mongo
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: myregistry/mongo:latest
        volumeMounts:
          - mountPath: "/srv"
            name: srv-volume
          - mountPath: "/data/db"
            name: db-volume
        securityContext:
          privileged: true
        env:
        - name: TERM
          value: "xterm"
        ports:
        - containerPort: 27017
      volumes:
        - name: srv-volume
          azureFile:
            secretName: azshare-secret
            shareName: fs-docker-srv
            readOnly: false
        - name: db-volume
          azureFile:
            secretName: azshare-secret
            shareName: fs-docker-mongodb
            readOnly: false
      imagePullSecrets:
      - name: embrace-auth
```
{: style="text-align: justify"}
Notare sotto **volumes** la sezione **azureFiles** che referenzia il secret e lo **shareName** che è il nome dell'Azure File Share creato dal portale di Azure.