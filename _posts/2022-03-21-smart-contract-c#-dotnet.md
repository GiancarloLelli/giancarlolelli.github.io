---
layout: post
title:  "Sviluppo di Smart Contract con C# e .NET Core"
date:   2022-03-21 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Settimana scorsa ho deliverato la sessione alla .NET Conf 2022 di Roma dove ho parlato di sviluppo di applicazione Web3 tramite l'ausilio di .NET Core. E' stata una sessione interessante e ho raggiunto forse il mio all time high di partecipanti, 90 :) durante la sessione ho mostrato le capacità del Blockchain Toolkit della NEO N3 Blockchain e come questa aiuta noi dev .NET a rimanere al passo con questo nuovo trend tecnologico. Per invogliarvi a guardare la sessione registrata una volta che sarà live su Youtube, vi incollo lo snippet di codice di una semplice calcolatrice decentralizzata ;)

```csharp
using System;
using System.ComponentModel;

using Neo;
using Neo.SmartContract.Framework;
using Neo.SmartContract.Framework.Attributes;
using Neo.SmartContract.Framework.Native;
using Neo.SmartContract.Framework.Services;

namespace dotnetconf
{
    [DisplayName("DecentralizedCalculatorV1")]
    [ManifestExtra("Author", "Giancarlo Lelli")]
    [ManifestExtra("Email", "giancarlo.lelli@avanade.com")]
    [ManifestExtra("Description", "This is a sample contract on the NEO Blockchain")]
    public class DecentralizedCalculatorV1 : SmartContract
    {
        [DisplayName("sum")]
        public static int Sum(int a, int b)
        {
            return a + b;
        }

        [DisplayName("sub")]
        public static int Sub(int a, int b)
        {
            return a >= b ? (a-b) : 0;
        }

        [DisplayName("mult")]
        public static int Mult(int a, int b)
        {
            return a * b;
        }

        [DisplayName("div")]
        public static int Div(int a, int b)
        {
            return a / b;
        }

        [DisplayName("destroy")]
        public static void Destroy()
        {
            ContractManagement.Destroy();
        }
    }
}
```
