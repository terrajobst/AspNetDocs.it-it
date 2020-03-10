---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modello Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modello di applicazione Breeze/Knockout a pagina singola
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558191"
---
# <a name="breezeknockout-template"></a>Modello Breeze/Knockout

di [Mads Kristensen](https://github.com/madskristensen)

> Il modello di MVC per la brezza/Knockout è stato scritto da Ward Bell
> 
> [Scaricare il modello di MVC/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)

Hai sentito parlare di "applicazione a pagina singola" (SPA) e chiederti cosa è. Sebbene sia possibile leggere il problema, è preferibile provare a usarlo. Ma chi ha tempo per scaricare un campione? Se si dispone di Visual Studio, sarà disponibile un esempio di SPA in esecuzione in meno di 60 secondi con il modello ASP.NET MVC 4 "Breeze/Knockout single page Application".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Che cos'è il modello Breeze/Knockout SPA?

La maggior parte dei modelli di progetto genera uno scheletro dell'applicazione. È possibile inserirli in tali ossa aggiungendo il codice e infine distribuendo un'applicazione funzionante. Il modello Breeze/Knockout SPA è diverso. Viene generata un'applicazione di esempio da studiare. Viene illustrata la progettazione di un'applicazione SPA e molte delle tecniche per la creazione di una SPA.

Il modello Breeze/Knockout è una variante del [modello knockout Spa](../introduction/knockoutjs-template.md) incluso nell'aggiornamento ASP.NET and Web Tools 2012,2. Il modello Breeze SPA genera un'applicazione con la stessa esperienza utente, ma ha un'implementazione diversa, usando Breeze per la gestione dei dati.

Il modello knockout SPA crea richieste di servizio con jQuery AJAX non elaborato, che è adatto per una semplice applicazione. Tuttavia, le app più sofisticate hanno requisiti di gestione dei dati più complessi. Ad esempio, la maggior parte delle applicazioni:

- Eseguire una query e ripetere la query sul server durante una sessione utente estesa.
- Aggiungere filtri query, ordinamento e paging.
- Condividere gli stessi dati in più schermate.
- Accumulare le modifiche a molti oggetti, quindi salvarli come una singola transazione.
- Convalidare le modifiche nel client, in modo che l'utente possa correggere gli errori prima di eseguire il commit delle modifiche nel database.

La libreria BreezeJS gestisce queste faccende, consentendo di sviluppare la logica dell'applicazione e l'esperienza utente più rilevante.

[**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la creazione di applicazioni Rich data in JavaScript e HTML, i tipi di app che sono stati distribuiti in passato come applicazioni desktop autonome.

Il modello Breeze/Knockout consente di intraprendere questo primo passaggio cruciale verso un'infrastruttura di gestione dati più affidabile. Produce un'applicazione todo di esempio che è esternamente identica al modello di Knockout SPA. All'interno di, sostituisce il livello dati AJAX con Breeze, quindi è possibile confrontare i due approcci side-by-side. Naturalmente, la possibilità di un'applicazione Breeze è poco semplice. Ma si vedrà il funzionamento di Breeze e il minimo necessario per eseguire tale transizione.

Ma veniamo al dunque.

## <a name="create-a-breezeknockout-template-project"></a>Creare un progetto di modello Breeze/Knockout

Scaricare e installare il modello facendo clic sul pulsante di download riportato sopra. Il modello viene incluso nel pacchetto come file VSIX (Visual Studio Extension). Potrebbe essere necessario riavviare Visual Studio.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Assegnare un nome al progetto e fare clic su **OK**.

Nella creazione guidata **nuovo progetto** selezionare **Breeze Knockout Spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Premere CTRL + F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Quando l'applicazione viene eseguita per la prima volta, viene visualizzata una schermata di accesso. Fare clic sul collegamento "iscriversi" per visualizzare una nuova pagina, in cui è possibile immettere un nome utente e una password. Le pagine di accesso e registrazione vengono compilate con ASP.NET MVC. Quando si invia il modulo di registrazione, il server genera un oggetto todo con due elementi per l'account. Quindi li presenta con una nota gialla.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

A questo punto ci si trova nella terra di SPA. Tutto ciò che viene visualizzato ed esperienza durante la manipolazione di Todos viene sottoposto a rendering e gestito sul client con l'ausilio di Knockout e Breeze. Esplorare l'app come utente... ma con un occhio dello sviluppatore. Usare gli strumenti di sviluppo nel browser per acquisire il traffico di rete. (In Internet Explorer: premere F12, selezionare la scheda **rete** e fare clic su **Avvia acquisizione**). A questo punto, provare a eseguire le operazioni seguenti:

- Aggiungere un nuovo elemento todo.
- Fare clic sull'etichetta e modificare il titolo dell'elemento todo
- Selezionare una casella di controllo per contrassegnare l'elemento completato. Si noti che la casella di testo è disabilitata, quindi il titolo non è più modificabile.
- Fare clic su "x" a destra dell'etichetta. L'elemento scompare ed è stato eliminato dal database.
- Selezionare un altro elemento e cancellarne il titolo. Si riceverà un errore di convalida che indica che il titolo è obbligatorio. Dopo una breve pausa, il titolo precedente viene ripristinato.
- Digitare un titolo ridicolmente lungo. Si otterrà un errore di convalida diverso che indica che il titolo è troppo lungo.
- Fare clic sul pulsante "Aggiungi elenco TODO". Viene visualizzato un nuovo elenco a sinistra dell'elenco precedente.
- Riproduci il titolo dell'oggetto todo, attivando le convalide obbligatorie e di lunghezza.
- Fare clic nella casella di testo titolo per cancellare il messaggio di errore.
- Fare clic sulla "x" nel cerchio nell'angolo superiore destro per eliminare l'oggetto todo e la relativa proprietà todo.

La logica di convalida viene eseguita da Breeze sul lato client. Gli attributi di convalida sulle classi del modello server vengono propagati al client ed eseguiti automaticamente prima che il client contatti il server.

Esaminare il traffico di rete. Si noti che non sono state chiamate al server quando Breeze ha rilevato un errore. Ogni modifica valida ha generato una richiesta POST a "/api/Todo/SaveChanges". Breeze raggruppa le modifiche e le invia insieme come singola richiesta al metodo `SaveChanges` del controller API Web. Si tratta di un modello diverso rispetto a quello di Knockout SPA, che consente di inserire, pubblicare ed eliminare richieste singolarmente per ogni elemento.

## <a name="peek-inside"></a>Visualizza all'interno

Questa applicazione ha lato client e lato server. Lo stack sul lato client è costituito da un piccolo HTML e da una combinazione di moduli JavaScript dell'applicazione (nella cartella "app") più librerie JavaScript di terze parti (nella cartella "script").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se è stato esaminato il modello di Knockout SPA, questo dovrebbe avere un aspetto molto familiare. Concentrarsi sulle caselle blu. L'architettura dell'interfaccia utente è Model-View-ViewModel (MVVM), in cui i widget HTML della visualizzazione sono separati chiaramente dal codice di presentazione di supporto nel modello di visualizzazione. Un sistema di data binding (in questo caso Knockout) coordina la vista e il modello di visualizzazione in modo che ognuno possa svolgere il proprio lavoro senza una conoscenza approfondita dell'altro.

Il modello incapsula i dati todo. Le entità nel modello sono costruite da Breeze con proprietà osservabili Knockout, quindi possono essere associate direttamente ai widget nella visualizzazione. Il modello di visualizzazione chiede al contesto dei dati di acquisire e salvare le entità del modello. Il contesto dati delega la maggior parte del lavoro a Breeze.

Lo stack sul lato server è costituito da codice per sviluppatori e da tre librerie .NET principali: API Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è identica a quella del modello knockout SPA. Tuttavia, l'implementazione è molto più semplice: i DTO sono stati eliminati e la maggior parte dei Entity Framework dettagli sono stati delegati a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

Si consiglia di esplorare il codice, guidato dalla [descrizione approfondita](http://www.breezejs.com/spa-template?utm_source=ms-spa) del client e degli stack del server nel sito Web Breeze.

È possibile provare a giocare con la query lato client Breeze. aggiungere alcuni filtri e ordinamenti. È possibile aggiungere altre proprietà del modello e più entità per ottenere un'idea migliore per lo sviluppo di SPA end-to-end. Quando si è sicuri della progettazione, è possibile rimuovere le funzionalità todo e sostituirle con le proprie.

Presto si sarà pronti per il passaggio successivo: aggiungere schermate sul lato client e spostarsi tra di esse. Questo modello di SPA verrà lasciato invariato e si passerà a uno stack di SPA più completo, ad esempio [Hot asciugamano di John Papa](https://github.com/johnpapa/HotTowel#readme "Asciugamano caldo"), che aggiunge Durandal a Breeze and Knockout mix.
