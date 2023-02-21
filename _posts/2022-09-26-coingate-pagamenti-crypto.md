---
layout: post
title:  "Coingate per invoicing in Crypto"
date:   2022-09-26 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Nella mia costante esplorazione dell'ecosistema tecnologico delle FinTech collegate al mondo crypto sono riuscito finalmente a trovare un servizio, abbastanza user friendly per l'emissione di "fatture" e/o link di pagamento che si sostanziano nell'acquisto di cryptovalute tramite FIAT (ovvero Euro, Dollaro, Sterlina, ecc..) credo che questi servizi siano particolarmente utili poichè semplificano di molto la necessità di tutta una infrastruttura di pagamento per ricevere compensi per prestazioni occasionali o side project che morirebbero al Day-0 causa macchinosità della legge italiana.

Coingate è il servizio che molto semplicemente assolve a questi task. Il suo utilizzo può essere veicolato tramite widget o API. In basso qualche esempio su come si crea un "Subscriber"

```csharp
using System.Net.Http.Headers;
var client = new HttpClient();
var request = new HttpRequestMessage
{
    Method = HttpMethod.Post,
    RequestUri = new Uri("https://api.coingate.com/api/v2/billing/subscribers"),
    Headers =
    {
        { "accept", "application/json" },
    },
    Content = new FormUrlEncodedContent(new Dictionary<string, string>
    {
        { "email", "subscriber@email.com" },
        { "organisation_name", "Contoso" },
        { "first_name", "Luke" },
        { "last_name", "Cage" },
        { "address", "One Edgeless Way" },
        { "city", "Selargius" },
        { "postal_code", "09047" },
        { "country", "Italy" },
    }),
};
using (var response = await client.SendAsync(request))
{
    response.EnsureSuccessStatusCode();
    var body = await response.Content.ReadAsStringAsync();
    Console.WriteLine(body);
}
```

Una volta fatto questo possiamo creare la "Subscription" e poi attivarla.
```csharp
using System.Net.Http.Headers;
var client = new HttpClient();
var request = new HttpRequestMessage
{
    Method = HttpMethod.Post,
    RequestUri = new Uri("https://api.coingate.com/api/v2/billing/details"),
    Headers =
    {
        { "accept", "application/json" },
    },
    Content = new FormUrlEncodedContent(new Dictionary<string, string>
    {
        { "title", "MyService" },
        { "description", "Description of what you are billing" },
        { "callback_url", "https://mycallback.com/api/payment-callback" },
        { "send_paid_notification", "true" },
        { "send_payment_email", "true" },
        { "underpaid_cover_pct", "0.0" },
        { "payment_method", "monthly" },
        { "price_currency", "EUR" },
        { "receive_currency", "ETH" },
        { "items": [
            {
              "id": 123,
              "description": "My Price Item",
              "quantity": 1,
              "price": "100",
              "item_id": "456"
            }]
        }
    }),
};
using (var response = await client.SendAsync(request))
{
    response.EnsureSuccessStatusCode();
    var body = await response.Content.ReadAsStringAsync();
    Console.WriteLine(body);
}
```


Per maggiori dettagli potete consultare la documentazione ufficiale di Coingate disponibile a questo indirizzo: https://developer.coingate.com/reference/api-overview