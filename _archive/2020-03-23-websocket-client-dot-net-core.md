---
layout: post
title:  "Client di WebSocket con .NET Core"
date:   2020-03-23 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
In uno dei mei ultimi esperimenti ho avuto la necessità di instaurare una connessione con un WebSocket da una console application .NET Core 3.0 - cercando su Google la cosa sembra ben documentata qualora si voglia realizzare un server Web Socket tramite lo stack messo a disposizione da ASP.NET Core, quando si parla di client invece, tutto abbastanza lasciato intendere tra le righe. Per questo motivo riporto il codice che mi ha permesso di realizzare una connessione Web Socket, e leggere il relativo payload (nel mio caso in formato Protopuf):

{: style="text-align: justify"}
Importante referenziare la libreria che contiene la classe **ClientWebSocket**

```powershell
dotnet add package System.Net.WebSockets --version 4.3.0
```

```csharp
public async void Subscribe(string symbol)
{
    var token = new CancellationToken();
    var sendToken = new CancellationToken();
    var receiveToken = new CancellationToken();

    var client = new ClientWebSocket();
    await client.ConnectAsync(new Uri("wss://streamer.finance.yahoo.com/"), token);

    if (client.State == WebSocketState.Open)
    {
        var subscribe = JsonConvert.SerializeObject(new SubscribeObject(Uri.UnescapeDataString(symbol)));
        var subscribeSegment = new ArraySegment<byte>(System.Text.Encoding.UTF8.GetBytes(subscribe));

        await client.SendAsync(subscribeSegment, WebSocketMessageType.Text, true, sendToken);

        var receiveBuffer = new ArraySegment<byte>(new byte[4096]);
        WebSocketReceiveResult socketReceiveResult;
        while ((socketReceiveResult = await client.ReceiveAsync(receiveBuffer, receiveToken)) != null)
        {
            var notNulls = receiveBuffer.Where(x => x != default(byte)).ToArray();
            var stringProto = Encoding.UTF8.GetString(notNulls);
            var buffer = Convert.FromBase64String(stringProto);

            using (var mem = new MemoryStream(buffer))
            {
                try
                {
                    var des = Serializer.Deserialize<YahooSocketData>(mem);
                    LivePriceReceived?.Invoke(this, des);
                }
                catch (Exception)
                {
                    ColoredOutput.WriteLine(ConsoleColor.Red, "[LIVE] => Unable to decode live price");
                }
            }
        }
    }
}
```
