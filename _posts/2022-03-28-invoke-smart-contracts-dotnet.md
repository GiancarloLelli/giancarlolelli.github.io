---
layout: post
title:  "Invocazione di Smart Contract NEO (N3) con C# e .NET Core"
date:   2022-03-28 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Settimana scorsa abbiamo visto come creare un semplice Smart Contract in .NET Core, nel post di questa settimana vediamo come invocarlo. Il processo è semplice:
* Dato un RpcClient collegato ad un nodo NEO N3
* Prepariamo lo script hash della nostra chiamata a funzione
* Utilizziamo l'RpcCLient per eseguire tale chiamata e leggiamo il risultato
```csharp
using System;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Neo;
using Neo.Network.P2P.Payloads;
using Neo.Network.RPC;
using Neo.Network.RPC.Models;
using Neo.SmartContract.Native;
using Neo.VM;
using Neo.VM.Types;

namespace Dotnetconf.NEO.Demo
{
    internal class Program
    {
        static async Task Main(string[] args)
        {
            // Connection and contract preparation
            var client = new RpcClient(new Uri("http://localhost:50012"));
            var contractHash = "0x768851af3c2a8f5a1411ab06b86a72060e073712";
            var scriptHash = (await client.GetContractStateAsync(contractHash)).Hash;

            // Setting up contract functions
            byte[] script = scriptHash.MakeScript("sum", 15, 138);

            // Script invocation
            var scriptInvocationresult = await client.InvokeScriptAsync(script, new Signer
            {
                Account = UInt160.Parse("0x43f632a59e75516e684231aa7278258b1adfb623"),
                Scopes = WitnessScope.None
            });

            if (scriptInvocationresult.Exception == null)
            {
                Console.WriteLine($"GAS used: {scriptInvocationresult.GasConsumed}");
                Console.WriteLine($"State: {scriptInvocationresult.State}");

                var returnValue = scriptInvocationresult.Stack.FirstOrDefault();
                Console.WriteLine($"Returned value: {returnValue.GetInteger()}");
            }
        }
    }
}
```
