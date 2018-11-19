---
layout: post
title:  "Installare Docker da CLI su Windows Server per la modernizzazione di workload .NET"
date:   2018-11-12 9:00:00 +0200
categories: howto
---
{: style="text-align: justify"}
In vista della demo che presenterò a WPC2018 mi sono imbattuto in quello che è il processo di installazione di un cluster Docker EE. Il processo non è complicato, ma essendo lungo è molto semplice fare degli errori. Sicuramente lo step numero uno è quello di installare Docker sulla macchina che ad esempio vogliamo utilizzare come **worker node**. L'operazione può essere portata a atermine tramite Powershell con i seguenti comandi:
```powershell
Install-Module DockerProvider -Force
Install-Package -Name docker -ProviderName DockerProvider -Force
```
Questi semplici comandi installano sia il runtime che la CLI.  
Possiamo eseguire un'altro semplice step, sempre da Powershell, e abbiamo a disposizione anche *Compose* - il comando è il seguente.
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Invoke-WebRequest "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-Windows-x86_64.exe" -UseBasicParsing -OutFile $Env:ProgramFiles\docker\docker-compose.exe
```
