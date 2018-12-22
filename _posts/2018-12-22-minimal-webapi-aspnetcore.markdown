---
layout: post
title:  "Minimal Web API con ASP.NET Core"
date:   2018-12-22 9:00:00 +0200
categories: opinion
---
{: style="text-align: justify"}
Con l'avvento di ASP.NET l'idea di rendere tutto il framework più snello rendendo così tutte le nostre applicazioni meno pesanti e impattanti sulla memoria già dal File -> New Project è una cosa che ho sempre trovato di **estremo** valore. Sfortunatamente però, con il tempo ci siamo tutti impigriti e l'idea di aver del plumbing code bello e pronto ci porta comunque ad avere già da subito una marea di dipendenze - grazie al meta package base di ASP.NET Core - nulla in contrario, ma personalmente una Web App snella e con solo le dipendenze minime per l'esposizione di endpoint REST a me risulta molto più appealing.  

{: style="text-align: justify"}
Fortunatamente, il buon Scott Hanselman ha pensato bene di scrivere un post in merito. Lo trovate a questo [link](https://www.hanselman.com/blog/ExploringAMinimalWebAPIWithASPNETCore.aspx). Personalmente, prima di creare nuovamente un servizio con tutto il marasma delle dipendenze *comode* proverò a capire se una soluzione *empty* non possa fare comunque al caso mio.