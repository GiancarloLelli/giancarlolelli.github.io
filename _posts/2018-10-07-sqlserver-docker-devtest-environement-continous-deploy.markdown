---
layout: post
title:  "Continuous deployment con Azure DevOps di SQL Server DB Project su SQL Server 2017 on Docker EE (1/2)"
date:   2018-10-07 9:00:00 +0200
categories: devops
---
{: style="text-align: justify"}
In vista del mio speech a [SQL Saturday #777](https://giancarlolelli.github.io/talk/2018/10/01/upcoming-speeches-giancarlo-lelli.html) ho iniziato a lavorare alla demo. Dato che le demo con i container e le pipeline di CI/CD spesso e volentieri mi abbandonano nei momenti più importanti ho deciso di prendermi qualche giorno di preavviso :)  

{: style="text-align: justify"}
Diciamo che pensavo di metterci più tempo, ma dopo solo 4hh sono riuscito a mettere in piedi il 90% della demo. Ho dovuto rovistare nei meandri dei repository GitHub dei vari product group ma alla fine tutte le tech più nuove funzionano nell'armonia più totale.  

{: style="text-align: justify"}
Quali sono gli ingredienti della demo? Beh molto semplicemente:
* Docker
* SQL Server Data Tools di Visual Studio (per avere i SQL Database project)
* [SqlPackage](https://docs.microsoft.com/en-us/sql/tools/sqlpackage?view=sql-server-2017) - utility di SQL che utilizziamo per orchestrare l'import del nostro file *.dacpac* contente lo schema del DB
* Azure Dev Ops per la build & release pipeline

{: style="text-align: justify"}
Il setup è semplice. Si parte sempre da un SQL Server Database project configurato sul TF **4.5.2** dove abbiamo opportunamente riportato tutti gli script di creazione tabelle, viste, schema, ecc del nostro database. Allo stesso livello della solution poi abbiamo il classico *Dockerfile* e una serie di artifact a contorno che permettono:
- Installazione dell'utility SqlPackage sul container SQL Server 2017 Developer Edition
- Script Powershell per l'inizializzazione del Database e importazione *.dacpac*
- Seed dei dati sul DB di destinazione

{: style="text-align: justify"}
La cosa più rognosa di tutto questo "step" di preparazione della demo è il fatto che nelle ultime immagini di SQL Server che Microsoft ha rilasciato, l'utility **SqlPackage** non è più preinstallata. Pertanto, ho dovuto adattare il mio Dockerfile affinchè eseguisse l'installazione dell' MSI e successivamente andare a vedere il setup posiziona l'eseguibile che mi interessava. Come potete vedere dal file Powershell nella cartella artifacts, rispetto alla versione dell'immagine precedente cambia solo il numero di versione - fortunatamente. Maggiori info su questa breaking change li trovate nella issue ufficiale su GitHub [qui](https://github.com/Microsoft/mssql-docker/issues/135). Il commento di risposta invece, che mi ha aiutato a capire come risolvere il mi problema invece è [questo](https://github.com/Microsoft/mssql-docker/issues/135#issuecomment-389245587) qua.

{: style="text-align: justify"}
Di seguito la folder structure. Ovviamente tutto il codice è hostato su GitHub e l'indirizzo del repository è [questo](https://github.com/GiancarloLelli/sqlsaturday777). Nel prossimo post dettaglierò brevemente anche le pipeline che andrò a creare per il deploy su Docker EE che per comodità ho installato su una Docker Farm su Azure.

![](images/docker-devops-sqlserver-2017.PNG)