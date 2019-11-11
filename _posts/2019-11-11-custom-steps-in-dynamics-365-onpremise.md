---
layout: post
title:  "Custom plugin in step Internal su Dynamics 365 On-Premise"
date:   2019-11-11 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Nel post di oggi condivido un hack che dopo una serata passando a smanettare sulle tabelle del database di Dynamics 365 On-Premise sono riuscito a mettere in piedi. In particolare si tratta di uno script SQL che se eseguito permette di agganciare a qualsiasi evento messo a disposizione da Dynamics 365 un plugin custom. Come sappiamo infatti, non tutti i *messaggi* che la piattaforma ha a disposizione possono essere *personalizzati* con plugin custom. **Sottolineo** che questo approccio **non è supported** quindi rischia di pregiudicare la vostra migrazione sul Cloud. Use it at your own risk :)

{: style="text-align: justify"}
Questo il codice SQL che permette di rendere visibile dal Plugin Registration Tool il messaggio internal che vogliamo sottoscrivere.
```sql
SELECT F.*
FROM SdkMessageFilter AS F
INNER JOIN SdkMessageBase AS S ON F.SdkMessageId = S.SdkMessageId
WHERE F.SdkMessageIdName LIKE '%Excel'

UPDATE F
SET IsCustomProcessingStepAllowed = 1, Availability = 2
FROM SdkMessageFilter AS F
WHERE F.SdkMessageIdName LIKE '%Excel'


UPDATE S
SET S.IsPrivate = 0, S.Expand = 1, S.Availability = 2, S.AutoTransact = 0, S.CategoryName = 'Export', S.IsReadOnly = 1
FROM SdkMessageFilter AS F
INNER JOIN SdkMessageBase AS S ON F.SdkMessageId = S.SdkMessageId
WHERE F.SdkMessageIdName LIKE '%Excel'
```
{: style="text-align: justify"}
Oltre allo script di SELECT gli altri due sono un esempio relativo al messaggio **ExportToExcel**