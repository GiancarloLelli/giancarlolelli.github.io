---
layout: post
title:  "Esperimenti con CoreWCF"
date:   2023-01-30 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Durante la pausa Natalizia mi sono portato avanti con il coding di un servizio che per un cliente mi vedrebbe direttamente coinvolto come dev. Dato che il sistema sorgente comunica solo in SOAP ho provato a vedere se riuscivo a impostare la foundation di questa mini-API utilizzando CoreWCF. Devo dire che è stato abbastanza veloce...in basso il codice che ho usato per impostare la mutua autenticazione tramite certificato.
```csharp
public class Startup
{
    private readonly IConfiguration _configuration;

    public Startup(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddServiceModelServices();
        services.AddServiceModelMetadata();
        services.AddSingleton<IServiceBehavior, UseRequestHeadersForMetadataAddressBehavior>();

        var eventHubName = _configuration["EventHubName"];
        var eventHubNamespace = _configuration["EventHubNamespace"];

        if(string.IsNullOrEmpty(eventHubName) || string.IsNullOrEmpty(eventHubNamespace))
        {
            throw new ArgumentNullException("Errors is Event Hub configuration keys. One or more values empty");
        }

        var producerClient = new EventHubProducerClient(eventHubNamespace, eventHubName, new DefaultAzureCredential());
        services.AddSingleton<EventHubProducerClient>(producerClient);
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseServiceModel(serviceBuilder =>
        {
            var binding = new WSHttpBinding();
            binding.Security.Mode = SecurityMode.TransportWithMessageCredential;
            binding.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
            binding.Security.Message.NegotiateServiceCredential = false;
            binding.Security.Message.EstablishSecurityContext = false;
            binding.Namespace = "http://v1.ws.poslog.xcenter.dtv/";

            serviceBuilder.AddService<PoslogStrReceiverApi>();
            serviceBuilder.ConfigureServiceHostBase(typeof(PoslogStrReceiverApi), (conf) =>
            {
                conf.Credentials.ClientCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.Custom;
                conf.Credentials.ClientCertificate.Authentication.CustomCertificateValidator = new OvsCertificateValidator();
            });

            serviceBuilder.AddServiceEndpoint<PoslogStrReceiverApi, IPoslogStrReceiverApi>(binding, "/PoslogStrReceiverApi.svc");

            var serviceMetadataBehavior = app.ApplicationServices.GetRequiredService<ServiceMetadataBehavior>();
            serviceMetadataBehavior.HttpsGetEnabled = true;
            serviceMetadataBehavior.HttpGetEnabled = false;
        });
    }
}
```
