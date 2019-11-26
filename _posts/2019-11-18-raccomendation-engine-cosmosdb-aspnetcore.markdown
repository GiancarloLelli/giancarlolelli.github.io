---
layout: post
title:  "Termine PoC su Digital Decoupling"
date:   2019-11-18 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Nei grioni scorso sono finalmente riuscito a terminare la PoC relativa al Digital Decoupling su cui stavo lavorando con la nostra organizzazione Europe. Devo dire che gestire i grafi con Cosmos DB non è semplicissimo e la documentazione disponibile è davvero poca. Ma comunque, provando e sbagliando sono riuscito a creare un Raccomandation Engine su Cosmos DB. La svolta è stata quando ho trovato [questo Lab](https://github.com/MicrosoftLearning/20777A---Implementing-Microsoft-Azure-Cosmos-DB-Solutions/blob/master/Instructions/20777A_MOD05_DEMO.md).

{: style="text-align: justify"}
Di tutto il codice scritto alla fine la magia sta nel metodo *Reccomend* invocato dall'applicativo di Front-End quando si visita il dettaglio di un prodotto:

```csharp
public async Task<IActionResult> Recommend(Guid id)
{
    var recommended = await m_graph.GetRecommendations(id.ToString());
    var crmProducts = await m_dynamics.GetProductByIds(recommended.ToArray());
    var productModelList = new List<ProductModel>();

    foreach (var item in crmProducts)
    {
        var p = new ProductModel
        {
            Id = item.Id,
            ProductName = item.ProductName,
            ProductDesc = item.ProductDesc,
            Price = item.Price,
            ProductImgUrl = "https://via.placeholder.com/150"
        };

        productModelList.Add(p);
    }

    return Json(productModelList);
}
```
