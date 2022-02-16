---
layout: post
title:  "Usare MSI in una applicazione ASP.NET Core"
date:   2019-05-29 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Le MSI sono un concetto relativamente nuovo su Azure ma che personamente apprezo molto perchè permettono una comunicazione tra servizi Azure molto immediata e diretta, senza quindi doversi preoccupare troppo dei vari internals di securitu di ogni servizio. Questa funzionalità è consumabile con un semplice package NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication). La documentazione ufficiale MS su Docs.com è davvero dettagliata è presenta vari scenari applicativi, [questo](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication) il link. In basso n esempio di codice che mostra come è semplice richiedere un Access Token per parlare con Azure Key Valut usando le MSI.
```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;

// Instantiate a new KeyVaultClient object, with an access token to Key Vault
var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

// Optional: Request an access token to other Azure services
var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
```
