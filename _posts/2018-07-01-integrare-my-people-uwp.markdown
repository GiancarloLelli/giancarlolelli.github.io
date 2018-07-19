---
layout: post
title:  "Integrare la barra \"My People\" nella UWP"
date:   2018-07-01 9:00:00 +0200
categories: uwp
---
Una funzionalità che ho trovato molto utile nelle ultime versioni di Windows è la barra **My People** - questa funzionalità infatti ci permette di avere una serie di contatti preferiti pinnati all'interno della task bar e avere a disposizione una serie di quick action per interagire con questi contatti. Questa funzionalità è un chiaro esempio di incentivo alla collaborazione e all'empowerment dello user verso una tecnologia che lo aiuta e non lo ostacola in quanto di difficile utilizzo.

Ovviamente, come la maggior parte delle funzionalità di Windows - anche l'integrazione della barra My People è fruibile tramite API della Universal Windows Platform in moda da permettere a noi sviluppatori indipendenti di arricchiere le nostre app e le nostre experiences traimte questa nuova funzionalità.

Va da se che il core di questa funzionalità sta nell'interazione con quello che è il `ContactStore`, quindi lo step numero uno è quello di includere all'interno del manifest della nostra applicazione la capability di interazione con il `Contact Panel`.

```xml
<uap4:Extension Category="windows.contactPanel">
    <uap4:ContactPanel SupportsUnknownContacts="true" />
</uap4:Extension>
```
Fatto questo non ci resta che iniziare a costruire l'esperienza della nostra applicazione - con esperienza intendo definire il modo in cui l'app crea/mostra i contatti all'utente e le azioni che questo può fare su di essi. Fondamentalmente però, nel contesto della barra *My People* quello che vorremmo è che l'utente sia in grado di pinnare il profilo di un contatto. Per fare questo, la porzione di codice è molto semplice e la riporto qui in basso:
```cs
private async Task PinToMyPeople(Contact contact)
{
    PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
    await pinnedContactManager.RequestPinContactAsync(contact, PinnedContactSurface.Taskbar);
}
```
Come possiamo vedere, dallo snippet in alto data una reference ad un oggetto di tipo [*Contact*](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contact) che più in la vedremo che ha bisogno di una serie di *annotazioni* per rendere l'esperienza di ingaggio più ricca, ci occupiamo di ottenere un riferimento al `PinnedContactManager` questo oggetto, tramite il metodo `RequestPinnedContactAsync` i permette in definitiva di aggiungere il contatto scelto alla barra *My People*. Ovviamente, come succede per molte capability di Windows (es: la Geolocalizzazione) verrà chiesto un consenso all'utente per consentire all'applicazione di interagire con la task bar.

Prima accennavo come un contatto necessiti di una serie di *annotazioni* per permettere all'utente una interazione più ricca. In questo contesto mi riferivo alla possibilità che Windows fornisce all'utente di condividere, contattare e modificare o in generale interagire con il contatto tramite la *My People* bar. L'annotazione di un contatto è una operazione che può essere portata a termine o in fase di creazione - qualora la nostra app preveda la creazione di contatti all'interno del `ContactStore` oppure in una fase successiva. In questo paricolare processo abbiamo una serie di classi di riferimento che vi elenco in basso:
* [ContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactmanager)
* [ContactAnnotationStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactannotationstore)
* [ContactAnnotationList](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactannotationlist)
* [ContactAnnotation](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactannotation)

Il flusso di utilizzo di queste quattro può essere descritto come segue: Usando il mio `ContacManager` tramite il metodo `RequestAnnotationStoreAsync` chiedo una istanza dell'oggetto `ContactAnnotationStore` - dato l'annotation store, tramite il metodo `FindAnnotationListAsync`, ottengo una read only list contenente delle `ContactAnnotationList`. Utilizzando la prima o creandono una qualora la read only list sia vuota, salvo tramite il metodo `TrySaveAnnotationListAsync` un nuovo oggetto di tipo `ContactAnnotation` che ho istanziato popolando **obbligatoriamente** i campi *RemoteId* e *ContactId* con le stesse info che trovo nel contatto di riferimento che sto arricchendo e le *SupportedOperations* che identificheranno tramite una combinazione di valori presenti nella enumerazione [ContactAnnotationOperations](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactannotationoperations) ciò che l'utente potrà fare con il contatto pinnato.

Personalmente trovo questa funzionalità molto utile. Ricercando info ed esempi di utilizzo mi sono imbattuto in questo post dettagliato che oltre a dare una panoramica teorica di questa funzionalità confeziona una demo di esempio e delle GIF animate che rendono meglio l'idea, [questo](http://www.shenchauhan.com/blog/2018/1/10/mypeople) è il link.

#KeepDepeveloping\
Giancarlo