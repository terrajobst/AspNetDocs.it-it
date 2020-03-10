---
uid: single-page-application/overview/templates/backbonejs-template
title: Modello backbone | Microsoft Docs
author: madskristensen
description: Modello di SPA backbone. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558226"
---
# <a name="backbone-template"></a>Modello Backbone

di [Mads Kristensen](https://github.com/madskristensen)

> Il modello backbone SPA è stato scritto da Manzur Rashid
> 
> [Scaricare il modello di SPA backbone. js](https://go.microsoft.com/fwlink/?LinkId=293631)

Il modello di SPA backbone. js è progettato per iniziare rapidamente a creare app Web interattive sul lato client con [backbone. js.](http://backbonejs.org/)

Il modello fornisce uno scheletro iniziale per lo sviluppo di un'applicazione backbone. js in ASP.NET MVC. Fornisce funzionalità di base per l'accesso degli utenti, tra cui l'iscrizione, l'accesso, la reimpostazione della password e la conferma dell'utente con i modelli di posta elettronica di base.

Requisiti:

- [Aggiornamento di ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Creare un progetto di modello backbone

Scaricare e installare il modello facendo clic sul pulsante di download riportato sopra. Il modello viene incluso nel pacchetto come file VSIX (Visual Studio Extension). Potrebbe essere necessario riavviare Visual Studio.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Assegnare un nome al progetto e fare clic su **OK**.

![](backbonejs-template/_static/image1.png)

Nella creazione guidata **nuovo progetto** selezionare progetto Spa backbone. js.

![](backbonejs-template/_static/image2.png)

Premere CTRL + F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](backbonejs-template/_static/image3.png)

Facendo clic su "account personale" viene visualizzata la pagina di accesso:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Procedura dettagliata: codice client

Iniziamo con il lato client. Gli script dell'applicazione client si trovano nella cartella ~/Scripts/Application L'applicazione è scritta in [typescript](http://www.typescriptlang.org/) (file con estensione TS) che vengono compilati in JavaScript (file con estensione js).

**Applicazione**

`Application` è definito in Application. TS. Questo oggetto Inizializza l'applicazione e funge da spazio dei nomi radice. Mantiene le informazioni sulla configurazione e sullo stato condivise tra l'applicazione, ad esempio se l'utente ha eseguito l'accesso.

Il metodo `application.start` crea le visualizzazioni modali e connette i gestori eventi per gli eventi a livello di applicazione, ad esempio l'accesso dell'utente. Successivamente, viene creato il router predefinito e viene verificato se è specificato un URL sul lato client. In caso contrario, viene reindirizzato all'URL predefinito (#!/).

**Eventi**

Gli eventi sono sempre importanti quando si sviluppano componenti a regime di controllo libero. Le applicazioni spesso eseguono più operazioni in risposta a un'azione dell'utente. Il backbone fornisce eventi predefiniti con componenti quali modello, raccolta e visualizzazione. Anziché creare tra le dipendenze tra questi componenti, il modello usa un modello "pub/sub": l'oggetto `events`, definito in Events. TS, funge da Hub eventi per la pubblicazione e la sottoscrizione di eventi dell'applicazione. L'oggetto `events` è un singleton. Nel codice seguente viene illustrato come sottoscrivere un evento e quindi attivare l'evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

In backbone. js, un router fornisce metodi per il routing di pagine lato client e la connessione a azioni ed eventi. Il modello definisce un singolo router, in router. TS. Il router crea le visualizzazioni attivabili e mantiene lo stato durante il cambio di viste. (Le viste attivabili sono descritte nella sezione successiva). Inizialmente, il progetto ha due visualizzazioni fittizie, Home e about. Dispone inoltre di una visualizzazione NotFound, che viene visualizzata se la route non è nota.

**Visualizzazioni**

Le visualizzazioni sono definite in ~/scripts/Application/views. Sono disponibili due tipi di visualizzazioni, viste attivabili e visualizzazioni finestra di dialogo modali. Le visualizzazioni attivabili vengono richiamate dal router. Quando viene visualizzata una visualizzazione attivabile, tutte le altre viste attivabili diventano inattive. Per creare una visualizzazione attivabile, estendere la visualizzazione con l'oggetto `Activable`:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Estendendo con `Activable` vengono aggiunti due nuovi metodi alla vista, `activate` e `deactivate`. Il router chiama questi metodi per attivare e disattivare la visualizzazione.

Le visualizzazioni modali vengono implementate come finestre di dialogo modali di [bootstrap di Twitter](https://twitter.github.com/bootstrap/) . Le visualizzazioni `Membership` e `Profile` sono viste modali. Le visualizzazioni modello possono essere richiamate da qualsiasi evento dell'applicazione. Ad esempio, nella vista `Navigation` fare clic sul collegamento "account personale" per visualizzare la visualizzazione `Membership` o la visualizzazione `Profile`, a seconda che l'utente sia connesso o meno. Il `Navigation` connette i gestori eventi click a tutti gli elementi figlio con l'attributo `data-command`. Ecco il markup HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Ecco il codice in Navigation. TS per associare gli eventi:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Models**

I modelli sono definiti in ~/scripts/Application/Models. Tutti i modelli hanno tre elementi di base: gli attributi predefiniti, le regole di convalida e un endpoint lato server. Ecco un esempio tipico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-in**

La cartella ~/Scripts/Application/lib contiene alcuni plug-in jQuery pratici. Il file form. TS definisce un plug-in per l'utilizzo di dati del modulo. Spesso è necessario serializzare o deserializzare i dati del modulo e visualizzare gli eventuali errori di convalida del modello. Il plug-in form. TS include metodi quali `serializeFields`, `deserializeFields`e `showFieldErrors`. Nell'esempio seguente viene illustrato come serializzare un form in un modello.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Il plug-in Flashbar. TS fornisce vari tipi di messaggi di feedback all'utente. I metodi sono `$.showSuccessbar`, `$.showErrorbar` e `$.showInfobar`. Dietro le quinte, vengono usati gli avvisi bootstrap di Twitter per visualizzare i messaggi animati.

Il plug-in Confirm. TS sostituisce la finestra di dialogo di conferma del browser, anche se l'API è leggermente diversa:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Procedura dettagliata: codice server

Esaminiamo ora il lato server.

**Controller**

In un'applicazione a pagina singola, il server svolge solo un piccolo ruolo nell'interfaccia utente. In genere, il server esegue il rendering della pagina iniziale e quindi invia e riceve i dati JSON.

Il modello ha due controller MVC: `HomeController` esegue il rendering della pagina iniziale e `SupportsController` viene usato per confermare nuovi account utente e reimpostare le password. Tutti gli altri controller nel modello sono API Web ASP.NET controller che inviano e ricevono dati JSON. Per impostazione predefinita, i controller utilizzano la nuova classe `WebSecurity` per eseguire attività correlate all'utente. Tuttavia, hanno anche costruttori facoltativi che consentono di passare delegati per queste attività. Questo rende più semplice il test e consente di sostituire `WebSecurity` con altri elementi, usando un contenitore IoC. Ecco un esempio:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Visualizzazioni

Le visualizzazioni sono progettate per essere modulari: ogni sezione di una pagina dispone di una propria visualizzazione dedicata. In un'applicazione a pagina singola, è comune includere le visualizzazioni che non dispongono di un controller corrispondente. È possibile includere una vista chiamando `@Html.Partial('myView')`, ma ciò diventa noioso. Per semplificare questa operazione, il modello definisce un metodo di supporto, `IncludeClientViews`, che esegue il rendering di tutte le visualizzazioni in una cartella specificata:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Se il nome della cartella non è specificato, il nome predefinito della cartella è "ClientViews". Se la visualizzazione client usa anche visualizzazioni parziali, denominare la visualizzazione parziale con un carattere di sottolineatura, ad esempio `_SignUp`. Il metodo `IncludeClientViews` esclude tutte le visualizzazioni il cui nome inizia con un carattere di sottolineatura. Per includere una visualizzazione parziale nella visualizzazione client, chiamare `Html.ClientView('SignUp')` anziché `Html.Partial('_SignUp')`.

**Invio di posta elettronica**

Per inviare messaggi di posta elettronica, il modello USA [Postal](http://aboutcode.net/postal). Tuttavia, Postal viene sottratto dal resto del codice con l'interfaccia `IMailer`, quindi è possibile sostituirlo facilmente con un'altra implementazione. I modelli di posta elettronica si trovano nella cartella visualizzazioni/messaggi di posta elettronica. L'indirizzo di posta elettronica del mittente viene specificato nel file Web. config, nella chiave `sender.email` della sezione **appSettings** . Inoltre, quando `debug="true"` in Web. config, l'applicazione non richiede la conferma dell'indirizzo di posta elettronica dell'utente per velocizzare lo sviluppo.

## <a name="github"></a>GitHub

È anche possibile trovare il modello di SPA backbone. js in [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
