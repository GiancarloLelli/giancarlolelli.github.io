---
layout: post
title:  "Healthchecks in ASP.NET Core"
date:   2018-12-20 09:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Una delle funzionalità introdotte in ASP.NET Core 2.2 (scaricabile da [qua]()) è la possibilità di definire degli Health Checks custom per le nostre applicationi. Il processo è molto semplice. Dopo avrer installto il package NuGet
```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.HealthChecks -Version 2.2.0
```
{: style="text-align: justify"}
Abbiamo a disposizione l'interfaccia **IHealthCheck** che con il solo metodo **CheckHealthAsync** ci permette di restituire un **Task<HealthCheckResult>** che permette di enumerate le casistiche *Healthy*, *Degraded*, *Unhelthy*. Fare il plugin degli Health checks è molto semplice. Vediamo i tre passaggi fondamentali:

#### Aggiunta AddHealthCheck in Startup.cs
Come ogni middleware aggiuntivo, il primo step è aggiungere alla pipeline.
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks().AddCheck<SampleHealthCheck>("sample_check");
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

#### Aggiunta UseHealthChecks in Startup.cs
Una volta aggiunto il middleware, lo ingaggiamo con il solito metodo *Use_qualcosa*
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseDeveloperExceptionPage();
    app.UseHealthChecks("/health", 8082);
    app.UseHttpsRedirection();
    app.UseMvc();
}
```
Notare come posso specificare un endpoint (con tanto di porta custom) dove i miei Health check saranno disponibili.

#### Creazione di un health check custom
Per questo step l'unica cosa da fare è creare una classe che implementa l'interfaccia **IHealthCheck** - poi la logica la decidete voi in base alla natura del check.
```csharp
using Microsoft.Extensions.Diagnostics.HealthChecks;
using System.Threading;
using System.Threading.Tasks;

namespace WebApplication3.Common
{
    public class SampleHealthCheck : IHealthCheck
    {
        public SampleHealthCheck()
        {
            // Potete avere servizi ibjected grazie all DI
        }

        public Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context,
                                                        CancellationToken cancellationToken = default(CancellationToken))
        {
            // Your logic here
            return Task.FromResult<HealthCheckResult>(new HealthCheckResult(HealthStatus.Healthy));
        }
    }
}
```
{: style="text-align: justify"}
Potete trovare tutti i dettagli diquesta interessante feature nella documentazione ufficiale su [Docs.com](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/health-checks?view=aspnetcore-2.2)