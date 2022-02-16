---
layout: post
title:  ".NET tooling a supporto di processi DevOps per Dynamics 365 CE"
date:   2018-10-14 9:00:00 +0200
categories: devops
---
{: style="text-align: justify"}
Recentemente sto studiando un possibile approccio DevOps alle progettualità su Dynamics 365 CE. Diciamo che è molto facile cadere nella tentazione di parure con lo sviluppo di tool custom per automatizzare release & deploy, ma fortunatamente DevOps non è solo tool - ma anche processi e persone. Il mio ragionamento dunque verte su tre pillar principali, che saranno poi ovviamente supportati da un tooling custom. I pillar sono i seguenti:

* Il modo migliore per organizzare il lavoro è con una board
* La gerarchia degli item non è una opinione
* Gli stati dei work item non sono una opinione

{: style="text-align: justify"}
Possono sembrare dei concetti banali, ma non in tutti i team questi tre punti sono chiari e rispettati - spesso perchè sono proprio i senior a sottovalurali. Dunque figuriamoci i dev più junior. Io credo che una volta formato il team sull'importanza di gestire in maniera accorta il prorio backlog, è possibile individuare una serie di assunzione che permettono di sviluppare del **tooling a supporto** dei processi di release.

### Gestione del delta positivo
{: style="text-align: justify"}
Tramite l'iniziativa **Modernizing Dynamics 365** che porto avanti all'interno dell'ATCit ho i piano lo sviluppo di un middleware che integrandosi con Azure DevOps permette di gestire il delta (positivo) di change avvenute siano lato backend (plugin & co.) ma anche di tutto ciò che non è versionabile. Parlo quindi di ruoli, entità, form, dashboard, grafici, ecc. Questo è possibile perchè ragionando sul concetto di PBI/User Story possiamo decidere di creare una solution in CRM su ambiente di Dev che dovrà necessariamente contene tutte le change fatte a seguito dei task figli di quella PBI/User Story. Una volta che la PBI è in stato **New** oppure **Committed** creaiamo la solution in CRM (Dev) - quando passa in stato **Done** esportiamo la solution e la importiamo in ambiente **Test** - a questo proposito si potrebbe anche pensare di creare degli stati custom per le PBI/User Story che meglio identificano l'ambiente dove quel set di sviluppi deve andare.

### Replica dei record
{: style="text-align: justify"}
A questo proposito mi è venuto in mente solo un approccio tecnologico. Infatti credo sia abbastanza efficace utilizzare l'integrazione con Dynamics e Service Bug per creare una sorta di DB stile Event Store dove memorizzare tutte le entità che si vuole migrare di ambiente ad ambiente. Tramite poi una logica di replay cronologia posso ricreare ogni singolo record nei vari Test, QA & Prod. Ovviamente il lato negativo di questo approccio è la necessità di avere dei plugin che scattano in maniera async su create, update e delete di *N* entità dove N + indefinito e dipende dal caso.

### Differenze nei metadati
{: style="text-align: justify"}
Su qesto punto ancora mi interrogo, ma credo l'unica via sia quella di utilizzare le Metadata API di Dynamics per identificare i cambiamenti di campi avvenuti dal momento in cui la PBI, e quindi la solution, è stata creata e il momento in cui passa in stato Done. Questo range temporale dovrebbe servire a cancellare dall'ambiente di destinazione (e in qualche modo anche dagli environment diversi da Test) eventuali campi che causerebbero conflitti durante il processo di importazione della solution. Scrivendo questo post mi è anche venuto in mente che si potrebbe creare una sorta di registro di migrazione del datamodel che ragiona sempre in aggiunta, e che tramite una convention è in grado di versionare i campi. In questo modo io potrei evitare di cancellare i campi prima dell'import, non preoccuparmi di eventuali dipendenze e solo dopo la release potrei pulire il mio datamodel da campi non utilizzati.
  
A breve darò il via ai lavori e creero un repository su GitHub dedicato.