---
layout: post
title:  "Object pooling in .NET Core"
date:   2019-04-01 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Il pattern dell'Object Pooling ci permette di riciclare istanze di oggetti che per loro natura sono costosi da istanziare, talmente costosi che la loro inizializzazione può creare problemi di performance alle nostre applicazioni. Tramite un Object Pool noi ci teniamo da parte delle istanze riutilizzabili dell'oggetto X in modo tale che quando necessario abbiamo un oggetto pronto all'uso che può essere rilasciato e rimesso nel poll una volta finito.

{: style="text-align: justify"}
Essendo le performance uno dei punti chiave di questa rivoluzione del framework .NET siamo abbastanza fortunati da avere una libreria ufficiale di Microsoft che mette a didposizione delle classi che implementano questo Creational Design Pattern in modo corretto e che ci evitano di reinventare la ruota. Il package NuGet è disponibile a [questo](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) link e come al solito può essere installato tramite le varie CLI. Su **docs.microsoft.com** è disponibile persino la [documentazione](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.objectpool?view=aspnetcore-2.2) di tutte le classi.

{: style="text-align: justify"}
Volendo sporcarci le mani, ecco un esempio di utilizzo della classe **ObjectPool** in una Console Application .NET Core 2.2.
```csharp
using Microsoft.Extensions.ObjectPool;

namespace ConsoleApp2
{
    class Program
    {
        static void Main(string[] args)
        {
            // Istantiate
            var pool = new DefaultObjectPool<SuperHavyObject>(new SuperHeavyObjectPolicy());

            // Get reference
            var istance = pool.Get();

            // Use
            istance.MethodThatUsesHeavyLogic();

            // Return
            pool.Return(istance);
        }
    }

    class SuperHavyObject
    {
        public void MethodThatUsesHeavyLogic()
        {

        }
    }

    class SuperHeavyObjectPolicy : IPooledObjectPolicy<SuperHavyObject>
    {
        public SuperHavyObject Create()
        {
            return new SuperHavyObject();
        }

        public bool Return(SuperHavyObject obj)
        {
            // Put it back somewhere, maybe a ConcurrentDictionary
            return true;
        }
    }
}
```
