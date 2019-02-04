---
layout: post
title:  "Chiamare le API della UWP da una Console App .NET Core 3.0"
date:   2019-02-4 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Per la mia demo sulla Digital Customer Experienc al Digital Innovation Saturday di Pordenone della scorsa settimana ho indagato la possibilità che le applicazioni .NET Core 3.0 di chiamare le API della UWP. Dopo diversi vagabondaggi su GitHub sono riuscito finalmente a trovare *la soluzione* o se vogliamo *il modo* per raggiungere questo obbiettivo. E' molto semplice. Data una **Console App** .NET Core 3.0 creata tramite l'apposita CLI con `dotnet new console` non ci rimane da fare altro che editare il file .csproj come segue:
```xml
<ItemGroup>
    <PackageReference Include="System.Runtime.InteropServices.WindowsRuntime" Version="4.3.0" />
    <PackageReference Include="System.Runtime.WindowsRuntime" Version="4.3.0" />
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\10.0.17134.0\Windows.winmd</HintPath>
      <IsWinMDFile>true</IsWinMDFile>
      <Private>false</Private>
    </Reference>
  </ItemGroup>
```
{: style="text-align: justify"}
Come vediamo, gli step sono sostanzialmente due. Il primo è includere due package:
* System.Runtime.InteropServices.WindowsRuntime
* System.Runtime.WindowsRuntime

{: style="text-align: justify"}
Che si occuperanno di fornire dei metodi e quindi garantire e migliorare l'interoperabilità tra il Windows Runtime e il codice managed. Poi, successivamente aggiungere una reference al file dei metadati **Windows.md** per la target SDK che vogliamo utilizzare all'interno della nostra console application.

{: style="text-align: justify"}
Per semplificare tutto questo, ho creato un template di progetto per la CLI di .NET che una volta installato ci evita tutto questo editing manuale. Maggiori dettagli [qui](https://github.com/GiancarloLelli/uwpnetcoreconsoletemplate)