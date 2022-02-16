---
layout: post
title:  "Libreria .NET per login con Application User su Dynamics 365"
date:   2018-12-22 18:00:00 +0200
categories: library
---
{: style="text-align: justify"}
Molto spesso con Dynamics 365 è necessario sviluppare dei servizi batch, o comunque delle console application .NET per eseguire una serie di compiti che spaziano dall'importazione di dati a job di elaborazione generica. Con Dynamics 365 è possibile istrumentare l'SDK (il CrmServiceClient) per permettere il login con un tipo particolare di utenti, gli **ApplicationUser** - un vantaggio di questo tipo di utenti è che dopo la loro creazione e sincronizzazione in Dynamics è possibile disassociare la loro licenza mantenendo la loro capacità di login sul tenant.

{: style="text-align: justify"}
Per questo motivo, ho sviluppato un libreria .NET che ho reso disponibile su NuGet che permette di sfruttare questo potente meccanismo della piattaforma. Il link al package NuGet è [questo](https://www.nuget.org/packages/GL.Dynamics.Sdk.ApplicationUser.Auth) e il link al respository ufficiale è [questo](https://github.com/GiancarloLelli/dynamicsappuserlogin).

{: style="text-align: justify"}
Una volta installato il package con il comando:
```powershell
Install-Package GL.Dynamics.Sdk.ApplicationUser.Auth -Version 1.0.0
```
Possiamo utilizzatlo in questo modo:
```csharp
public void Login_As_Application_User_Test()
{
    ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;

    CrmServiceClient.AuthOverrideHook = new ApplicationUserAuthHook(APPSECRET, ORGURL, APPLICATIONID, TENANTID);

    var dynamics = new Uri(ORGURL);
    var client = new CrmServiceClient(dynamics, useUniqueInstance: true);

    var req = new WhoAmIRequest();
    var resp = client.Execute(req) as WhoAmIResponse;

    Assert.AreEqual(resp.UserId, Guid.Parse(APPUSERID));
}
```
In questo contento è molto importante impostare il SecurityProtocol a *Tls12* altrimenti il *CrmServiceClient* su framework inferiori a 4.6 non riuscirebbe a loggarsi (è una breaking change del flusso di autenticazione, non della libreria :))