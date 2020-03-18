---
layout: post
title:  "Utilizzare Let'Encrypt con GoDaddy + Azure App Service"
date:   2020-03-09 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Durante l'MVP Summit che quest'anno è diventato Virtual ho avuto la necessità di estrarre da un canale Teams le informazioni delle sessioni che venivano mano mano pubblicate, in maniera tale da potermi organizzare al meglio. Sono riuscito a confezionare uno script eseguibile dalla console del browser che vi riporto di seguito:
```javascript
$.each($('[data-tid="meetingMessageContent"] div'), function(k,v) { console.log(k + ': ' + v.innerText)})
```
