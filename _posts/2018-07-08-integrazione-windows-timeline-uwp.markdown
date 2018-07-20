---
layout: post
title:  "Integrazione con la Windows Timeline nella UWP"
date:   2018-07-08 9:00:00 +0200
categories: uwp
---
{: style="text-align: justify"}
Partendo dalla build Insider di Windows *17056* è disponibile la funzionalità *Timeline* - questa feature permette a noi utenti di aver tracciato il nostro comportamento all'interno del *Microsoft Graph* e di poter riprendere le nostre attività cross device, questi significa che il visitare una pagina dal mio (magari per leggere un blog post tecnico) smartphone *Nokia Lumia 1520* traccia un record nella mia Windows Timeline e quindi poi volendo posso riprendere la lettura di quello stesso post dal PC. Questa funzionalità è interessante perchè a mio parere abbatte il concetto di device fisico creando una sorta di quarta dimensione che sfrutta il Cloud come collante.
{: style="text-align: justify"}
Ovviamente grazie anche all'SDK di Windows, è possibile anche alle nostre applicazioni di scrivere dei record di `UserActivities` all'interno della Timeline. Il procedimento è molto semplice e si articola in quella che è l'inclusione di tre pezzi di informazione principali:
1. Un *Deep Link* di attivazione: utilizzato per ri attivare la nostra app con un determinato contesto
2. Le cosidette *Visual*: dei metadati grafici che permettono all'utente di capire a cosa fa riferimento l'activity
3. Dei *Metadati di contenuto*: una serie di informazioni che permettono di classificare tra loro varie `UserActivity`
{: style="text-align: justify"}
L'oggetto che identifica una `UserActivity` richiede una serie di informazioni principali, senza queste l'activity non è valida. Queste informazioni sono il testo di desiplay, lo URI di attivazione e l'id dell'attività. Tramite lo snippet di codice in basso possiamo vedere quanto immediato sia creare una *UserActivity* che rimanda l'utente alla MainPage della nostra app.
```cs
UserActivitySession _currentActivity;
 
private async Task CreateMainPageActivityAsync()
{
    UserActivityChannel channel = UserActivityChannel.GetDefault();

    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
    userActivity.VisualElements.DisplayText = "Jump to the Main Page";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
    await userActivity.SaveAsync();
}
```
{: style="text-align: justify"}
Lo schema concettuale è banale. Dato un gestore di activity andiamo a creare o aggiornate un item identificato dall'id "MainPage" - come precedentemente detto, settiamo il DisplayTest e un ActivationUri e successivamente salviamo il tutto all'interno del Graph. Una cosa importante da tenere a mente è che l'ID è l'informazione che permette di contestualizzare l'attività dell'utente - pertanto, deve essere in qualche modo contestualizzato altrimenti poi tutto il concetto di timeline perde di significato. L'oggetto `UserActivitySession` che nello snippet di codice non è utilizzato ricopre tuttavia un ruolo importante - poichè è grazie ad esso che il raggruppamento di diverse attività è possibile. Tutte le activity create all'interno di una sessione (instanziata tramite il metodo `CreateSession`) identificheranno un momento particolare all'interno della nostra timeline. Il `Dispose` dunque di questo oggetto è fondamentale - poichè ad ogni nuova interazione noi vogliamo assolutamente far corrispondere una nuova sessione.
{: style="text-align: justify"}
Nello snippet di codice sopra possiamo notare come l'app faccia riferimento anche ad uno URI schema proprietario. Questa pratica è molto comune in quanto ci permette di arricchire l'activation URI come megli ocrediamo, ma allo stesso tempo ci vincola alla registrazione di un nuovo *Protocol* altrimenti l'URI risulterebbe sconosciuto a Windows. Maggiori info sui protocolli e la UWP a [questo](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/handle-uri-activation) indirizzo.
{: style="text-align: justify"}
Sempre nel constesto della timeline, una feature molto interessante per costestualizzare e arricchire anche graficamente le *UserActivities* sono le *AdaptiveCards*. L'AdaptiveCard è in sostanza un layout definito tramite JSON che la timeline può utilizzare qualora all'interno della proprietà `VisualElement.Content` si utilizzi l'`AdaptiveCardBuilder` per fare il parsing di un determinato layout (molti esempi posso essere visti a [questo](http://adaptivecards.io/samples/) link).

In conclusione credo che questa feature e molte altre che piano piano si stanno aggiungendo a Windows stiano permettendo di avere una esperienza sempre più coesa e device indipendent...e questo mi piace, sia da un punto di vista dev che da utente.