---
layout: post
title:  "Docker.Net - Una SDK per gestire un Docker Cluster in C# con .NET"
date:   2018-10-28 9:00:00 +0200
categories: tooling
---
{: style="text-align: justify"}
Durante l'Hacktoberfest di Microsoft - l'iniziativa che premiava con una T-Shirt custom chiunque facesse una PR in un repository OpenSource di Microsoft - ho scoperto questo repository contente una SDK che wrappa quasi al 100% tutte le funzionalità della Docker CLI e le rende disponibili dunque ad essere consumate anche da applicazioni custom di orchestrazione. Ovviamente tutto è distribuito tramite NuGet ed installabile con il comando:
```powershell
Install-Package Docker.DotNet
```
Oppure se utilizziamo la CLI di .NET Core
```powershell
dotnet add package Docker.DotNet
```
{: style="text-align: justify"}
### Esempi di utilizzo
Giusto per darvi una idea di quanto è semplice utilizzare questa libreria, guardate questo esempio di codice proveniente dal repository ufficiale della libreria:
```csharp
using Docker.DotNet;
DockerClient client = new DockerClientConfiguration(
    new Uri("http://ubuntu-docker.cloudapp.net:4243"))
     .CreateClient();
```
{: style="text-align: justify"}
Queste 3 righe di codice collegano la nostra applicazione ad un deamon docker che sta da qualche parte nella rete. la libreria supporta anche la connessione al deamon locale e ad Docker engine su Linux. La classe **DockerCLientConfiguration** in questi casi viene parametrizzata opportutamente con l'indirizzo di una pipe/socket.  
  
Tramite questa libreria, una volta istanziato il cliente, effettuare l'equivalente di un *docker images* è semplicissimo.
```csharp
IList<ContainerListResponse> containers = await client.Containers.ListContainersAsync(
	new ContainersListParameters(){
		Limit = 10,
    });
```
La lista di code samples potrebbe continuare, ma credo che il concetto sia chiaro :)  
Questa SDK dupporta anche scenari l'autenticazione con certificato, o comunque anche endpoint non pubblici. Per fare plug-in di questa funzionalità è però necessario un ulteriore package NuGet.  
  
Questo per l'autenticazione tramite certificato
```powershell
dotnet add package Docker.DotNet.X509
```
  
Questo per l'autenticazione basica
```powershell
dotnet add package Docker.DotNet.BasicAuth
```
{: style="text-align: justify"}
### Scenari interessanti
Personalmente credo che questa libreria possa tornare molto utile in scenari di orchestrazione complessi o dove si vuole mettere su un Self Service Portal dove i dev e i tester possono tirare su pezzi di servizi e/o infrastruttura opportunatamente containerizzati a piacimento.

### Wrap up
Questo è un progetto molto interessante e il team sempre reattivo nella ricezione delle PR. Vi consiglio di dargli uno sguardo, anche perchè nel file [README.md](https://github.com/Microsoft/Docker.DotNet) potete trovare molti più esempi di codice e tutti i dettagli necessari e utilizzare questa libreria nei vostri progetti.