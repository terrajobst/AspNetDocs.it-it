---
uid: single-page-application/overview/templates/breezeangular-template
title: Modello Breeze/angolare | Microsoft Docs
author: madskristensen
description: Modello di applicazione Breeze/a pagina singola angolare
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578540"
---
# <a name="breezeangular-template"></a>Modello Breeze/Angular

di [Mads Kristensen](https://github.com/madskristensen)

> Il modello Breeze/interangolare MVC è stato scritto da Ward Bell
> 
> [Scaricare il modello Breeze/unangolar MVC](https://go.microsoft.com/fwlink/?LinkId=286437)

[AngularJS](http://angularjs.org) è una libreria open source di Google per la creazione di applicazioni a pagina singola (Spa). Offre data binding, l'inserimento delle dipendenze e la gestione dello schermo. Combinarlo con [breezejs](http://www.breezejs.com/?utm_source=ms-spa), un'altra libreria open source per la modellazione dei dati e la gestione dei dati, nonché gli ingredienti essenziali per una grande app client HTML/JavaScript.

Il modello Breeze/angolare SPA è una variante del [modello di Knockout Spa](../introduction/knockoutjs-template.md) incluso nell'aggiornamento ASP.NET and Web Tools 2012,2. Se si dispone di Visual Studio, si avrà un esempio di SPA operativo in meno di 60 secondi.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

All'esterno, l'applicazione ha un aspetto molto simile al modello di Knockout SPA. Ma è molto diverso dietro le quinte. Il modello knockout USA knockout per data binding e AJAX non elaborato per l'accesso ai dati. Il modello Breeze/angolare usa l'angolazione per data binding e Breeze per l'accesso ai dati. Queste librerie consentono funzionalità aggiuntive, tra cui la navigazione e la cronologia delle pagine.

Di seguito è illustrata la pagina about dell'applicazione:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

In questa pagina viene visualizzato un log di eventi in esecuzione durante la sessione utente corrente, tra cui:

- Paging. Si noti la creazione del controller todo in #2 e #7.
- Query remote (#3) e query della cache locale (#7).
- Salvataggio di nuove entità (#5, #6) e modificate (#4).
- Le modifiche vengono convalidate nel client (#9), in modo che l'utente possa correggere gli errori prima di eseguire il commit delle modifiche nel database.

È più necessario esplorare questo modello, tra cui:

- Caricamento dinamico dei modelli di visualizzazione HTML.
- Data binding personalizzati tramite direttive "angolari".
- Modularità e inserimento delle dipendenze.
- Filtri di query, ordinamenti, paging, proiezioni e inclusione di entità correlate.
- Condivisione di dati tra più schermate.
- Salvataggio di più modifiche come una singola transazione.
- Le regole di convalida vengono propagate automaticamente dal server al client JavaScript.

Ma veniamo al dunque.

## <a name="create-a-breezeangular-template-project"></a>Creare un progetto di modello Breeze/angolare

Scaricare e installare il modello facendo clic sul pulsante di download riportato sopra. Il modello viene incluso nel pacchetto come file VSIX (Visual Studio Extension). Potrebbe essere necessario riavviare Visual Studio.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Assegnare un nome al progetto e fare clic su **OK**.

Nella creazione guidata **nuovo progetto** selezionare **Breeze angolare Spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Premere CTRL + F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Fare clic sul collegamento "about" (informazioni su) in alto a destra per visualizzare un log di queste attività.

La logica di convalida viene eseguita da Breeze sul lato client. Gli attributi di convalida sulle classi del modello server vengono propagati al client ed eseguiti automaticamente prima che il client contatti il server.

Esaminare il traffico di rete. Si noti che non sono state chiamate al server quando Breeze ha rilevato un errore. Ogni modifica valida ha generato una richiesta POST a "/api/Todo/SaveChanges". Breeze raggruppa le modifiche e le invia insieme come singola richiesta al metodo `SaveChanges` del controller API Web. Si tratta di un modello diverso rispetto a quello di Knockout SPA, che consente di inserire, pubblicare ed eliminare richieste singolarmente per ogni elemento.

Si noti inoltre che non viene visualizzato alcun traffico di rete quando si passa da una pagina all'altra e informazioni sulle pagine. Ciò è dovuto al fatto che la query è stata vincolata alla cache Breeze locale.

## <a name="peek-inside"></a>Visualizza all'interno

Questa applicazione ha lato client e lato server. Lo stack sul lato client è costituito da un piccolo HTML e da una combinazione di moduli JavaScript dell'applicazione (nella cartella "app") più librerie JavaScript di terze parti (nella cartella "script").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

L'architettura dell'interfaccia utente separa i widget HTML delle visualizzazioni dal codice della presentazione di supporto nei controller. Il sistema di associazione dati angolare coordina le visualizzazioni e i controller in modo che ciascuno possa svolgere il proprio lavoro senza una conoscenza approfondita dell'altro.

Il controller chiede al contesto dei dati di acquisire e salvare le entità del modello. Il contesto dati delega la maggior parte del lavoro a Breeze, che costruisce oggetti modello con rilevamento automatico dai risultati della query JSON.

Lo stack sul lato server è costituito da codice per sviluppatori e da tre librerie .NET principali: API Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è identica a quella del modello knockout SPA. Tuttavia, l'implementazione è molto più semplice: i DTO sono stati eliminati e la maggior parte dei Entity Framework dettagli sono stati delegati a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

Si consiglia di esplorare il codice, guidato dalla [descrizione approfondita](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) del client e degli stack del server nel sito Web Breeze.

È possibile provare a giocare con la query lato client Breeze. aggiungere alcuni filtri e ordinamenti. È possibile aggiungere altre proprietà del modello e più entità per ottenere un'idea migliore per lo sviluppo di SPA end-to-end. Quando si è sicuri della progettazione, è possibile rimuovere le funzionalità todo e sostituirle con le proprie.

Buona codifica!
