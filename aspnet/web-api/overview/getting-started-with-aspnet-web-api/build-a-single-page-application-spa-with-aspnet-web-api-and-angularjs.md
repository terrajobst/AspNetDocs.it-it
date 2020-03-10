---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "Laboratorio pratico: creare un'applicazione a pagina singola (SPA) con API Web ASP.NET e angolar. js-ASP.NET 4. x"
author: rick-anderson
description: "Codice dettagliato: compilare un'applicazione a pagina singola (SPA) con API Web ASP.NET e angolar. js per ASP.NET 4. x."
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557043"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratorio pratico: creare un'applicazione a pagina singola (SPA) con API Web ASP.NET e angolar. js

dal [team di Web Camp](https://twitter.com/webcamps)

[Scarica il kit di formazione di Web Camp](https://aka.ms/webcamps-training-kit)

Questo laboratorio pratico Mostra come creare un'applicazione a pagina singola (SPA) con API Web ASP.NET e angolar. js per ASP.NET 4. x.

In questa esercitazione, si utilizzeranno tali tecnologie per implementare un quiz per geek, un sito Web di Trivia basato sul concetto di SPA. Il livello di servizio viene innanzitutto implementato con API Web ASP.NET per esporre gli endpoint necessari per recuperare le domande del quiz e archiviare le risposte. Sarà quindi possibile creare un'interfaccia utente completa e reattiva usando gli effetti di trasformazione AngularJS e CSS3.

Nelle applicazioni Web tradizionali, il client (browser) avvia la comunicazione con il server richiedendo una pagina. Il server elabora quindi la richiesta e invia il codice HTML della pagina al client. Nelle interazioni successive con la pagina, ad esempio l'utente passa a un collegamento o Invia un modulo con i dati, viene inviata una nuova richiesta al server e il flusso viene riavviato: il server elabora la richiesta e invia una nuova pagina al browser in risposta alla nuova richiesta di azione. ed dal client.
> 
> Nelle applicazioni a singola pagina (Spa) l'intera pagina viene caricata nel browser dopo la richiesta iniziale, ma le interazioni successive avvengono tramite richieste AJAX. Questo significa che il browser deve aggiornare solo la parte della pagina che è stata modificata. non è necessario ricaricare l'intera pagina. L'approccio SPA riduce il tempo impiegato dall'applicazione per rispondere alle azioni dell'utente, ottenendo un'esperienza più fluida.
> 
> L'architettura di una SPA comporta alcune problemi che non sono presenti nelle applicazioni Web tradizionali. Tuttavia, le tecnologie emergenti come API Web ASP.NET, i framework JavaScript come AngularJS e le nuove funzionalità di stile offerte da CSS3 semplificano la progettazione e la creazione di Spa.
> 
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Creare un servizio API Web ASP.NET per inviare e ricevere dati JSON
- Creare un'interfaccia utente reattiva usando AngularJS
- Migliorare l'esperienza dell'interfaccia utente con le trasformazioni CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/) o versione successiva

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.

1. Aprire Esplora risorse e passare alla cartella di **origine** del Lab.
2. Fare clic con il pulsante destro del mouse su **Setup. cmd** e scegliere **Esegui come amministratore** per avviare il processo di installazione che configurerà l'ambiente e installerà i frammenti di codice di Visual Studio per questo Lab.
3. Se viene visualizzata la finestra di dialogo controllo account utente, confermare l'azione per continuare.

> [!NOTE]
> Prima di eseguire l'installazione, verificare di aver selezionato tutte le dipendenze per questo Lab.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso dei frammenti di codice

Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornita come Visual Studio Code frammenti, a cui è possibile accedere da Visual Studio 2013 per evitare di aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnato da una soluzione iniziale che si trova nella cartella **Begin** dell'esercizio che consente di seguire ogni esercizio in modo indipendente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio risultano mancanti da queste soluzioni iniziali e potrebbero non funzionare fino a quando non è stato completato l'esercizio. All'interno del codice sorgente per un esercizio, si troverà anche una cartella **finale** contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione pratica include gli esercizi seguenti:

1. [Creazione di un'API Web](#Exercise1)
2. [Creazione di un'interfaccia SPA](#Exercise2)

Tempo stimato per il completamento del Lab: **60 minuti**

> [!NOTE]
> Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** . Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Esercizio 1: creazione di un'API Web

Una delle parti principali di una SPA è il livello di servizio. È responsabile dell'elaborazione delle chiamate AJAX inviate dall'interfaccia utente e della restituzione dei dati in risposta a tale chiamata. I dati recuperati devono essere presentati in un formato leggibile dal computer per essere analizzati e utilizzati dal client.

Il Framework API Web fa parte dello stack ASP.NET ed è progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e la ricezione di dati in formato JSON o XML tramite un'API RESTful. In questo esercizio verrà creato il sito Web per ospitare l'applicazione di quiz per geek e quindi implementare il servizio back-end per esporre e rendere permanente i dati del quiz utilizzando API Web ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Attività 1: creazione del progetto iniziale per il quiz per geek

In questa attività si inizierà a creare un nuovo progetto MVC ASP.NET con il supporto per API Web ASP.NET in base a un tipo di progetto **ASP.NET** che viene incluso in Visual Studio. **Un ASP.NET** unifica tutte le tecnologie ASP.NET e offre la possibilità di combinare e abbinarle in base alle esigenze. Si aggiungeranno quindi le classi del modello del Entity Framework e l'inizializzatore di database per inserire le domande del quiz.

1. Aprire **Visual Studio Express 2013 per il Web** e selezionare **file | Nuovo progetto...** per avviare una nuova soluzione.

    ![Creazione di un nuovo progetto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Creazione di un nuovo progetto")

    *Creazione di un nuovo progetto*
2. Nella finestra di dialogo **nuovo progetto** selezionare **applicazione Web ASP.NET** sotto l'oggetto **visivo C# | Scheda Web** . Assicurarsi che sia selezionato **.NET Framework 4,5** , denominarlo *GeekQuiz*, scegliere un **percorso** e fare clic su **OK**.

    ![Creazione di un nuovo progetto di applicazione Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Creazione di un nuovo progetto di applicazione Web ASP.NET")

    *Creazione di un nuovo progetto di applicazione Web ASP.NET*
3. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **MVC** e selezionare l'opzione **API Web** . Inoltre, assicurarsi che l'opzione **autenticazione** sia impostata su **singoli account utente**. Fare clic su **OK** per continuare.

    ![Creazione di un nuovo progetto con il modello MVC, inclusi i componenti dell'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Creazione di un nuovo progetto con il modello MVC, inclusi i componenti dell'API Web*
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **Models** del progetto **GeekQuiz** e scegliere **Aggiungi | Elemento esistente...**

    ![Aggiunta di un elemento esistente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Aggiunta di un elemento esistente")

    *Aggiunta di un elemento esistente*
5. Nella finestra di dialogo **Aggiungi elemento esistente** passare alla cartella **source/assets/Models** e selezionare tutti i file. Fare clic su **Aggiungi**.

    ![Aggiunta degli asset del modello](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Aggiunta degli asset del modello")

    *Aggiunta degli asset del modello*

    > [!NOTE]
    > Aggiungendo questi file, si aggiungono il modello di dati, il contesto di database Entity Framework e l'inizializzatore di database per l'applicazione di quiz per geek.
    > 
    > **Entity Framework (EF)** è un mapper relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite la programmazione con un modello di applicazione concettuale anziché programmare direttamente utilizzando uno schema di archiviazione relazionale. Per altre informazioni su Entity Framework, vedere [qui](../../../entity-framework.md).
    > 
    > Di seguito è riportata una descrizione delle classi appena aggiunte:
    > 
    > - **TriviaOption:** rappresenta una singola opzione associata a una domanda di quiz
    > - **TriviaQuestion:** rappresenta una domanda di quiz ed espone le opzioni associate tramite la proprietà **options**
    > - **TriviaAnswer:** rappresenta l'opzione selezionata dall'utente in risposta a una domanda di quiz
    > - **TriviaContext:** rappresenta il contesto di database del Entity Framework dell'applicazione di quiz per geek. Questa classe deriva da **DContext** ed espone le proprietà **DbSet** che rappresentano le raccolte delle entità descritte in precedenza.
    > - **TriviaDatabaseInitializer:** l'implementazione dell'inizializzatore di Entity Framework per la classe **TriviaContext** che eredita da **CreateDatabaseIfNotExists**. Il comportamento predefinito di questa classe consiste nel creare il database solo se non esiste, inserendo le entità specificate nel metodo **Seed** .
6. Aprire il file **Global.asax.cs** e aggiungere l'istruzione using seguente.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Aggiungere il codice seguente all'inizio dell' **applicazione\_metodo Start** per impostare **TriviaDatabaseInitializer** come inizializzatore di database.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificare il controller **Home** per limitare l'accesso agli utenti autenticati. A tale scopo, aprire il file **HomeController.cs** nella cartella **Controllers** e aggiungere l'attributo **autorizza** alla definizione della classe **HomeController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Il filtro di **autorizzazione** controlla se l'utente è autenticato. Se l'utente non è autenticato, restituisce il codice di stato HTTP 401 (non autorizzato) senza richiamare l'azione. È possibile applicare il filtro a livello globale, a livello di controller o a livello di singole azioni.
9. A questo punto sarà possibile personalizzare il layout delle pagine Web e la personalizzazione. A tale scopo, aprire il file **\_layout. cshtml** all'interno delle **visualizzazioni | Cartella condivisa** e aggiornare il contenuto dell'elemento **titolo&lt;&gt;** sostituendo l' *applicazione ASP.NET* con il *quiz geek*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Nello stesso file aggiornare la barra di spostamento rimuovendo i collegamenti *About* e *Contact* e rinominando il collegamento *Home* per la *riproduzione*. Inoltre, rinominare il collegamento *nome applicazione* a *quiz geek*. Il codice HTML per la barra di spostamento dovrebbe essere simile al codice seguente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aggiornare il piè di pagina della pagina di layout sostituendo l' *applicazione ASP.NET con il* *quiz geek*. A tale scopo, sostituire il contenuto dell'elemento **&lt;piè** di pagina&gt;con il codice evidenziato seguente.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Attività 2: creazione dell'API Web TriviaController

Nell'attività precedente è stata creata la struttura iniziale dell'applicazione Web Geek quiz. Verrà ora compilato un semplice servizio API Web che interagisce con il modello di dati del quiz ed espone le azioni seguenti:

- **Get/API/Trivia**: Recupera la domanda successiva dall'elenco dei quiz a cui deve rispondere l'utente autenticato.
- **Post/API/Trivia**: archivia la risposta al quiz specificata dall'utente autenticato.

Per creare la linea di base per la classe controller API Web, si useranno gli strumenti di impalcatura ASP.NET forniti da Visual Studio.

1. Aprire il file **WebApiConfig.cs** all'interno dell' **app\_** cartella di avvio. Questo file definisce la configurazione del servizio API Web, come il mapping delle route alle azioni del controller API Web.
2. Aggiungere l'istruzione using seguente all'inizio del file.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Aggiungere il codice evidenziato seguente al metodo **Register** per configurare globalmente il formattatore per i dati JSON recuperati dai metodi di azione dell'API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** converte automaticamente i nomi di proprietà in *Camel* case, ovvero la convenzione generale per i nomi di proprietà in JavaScript.
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **controller** del progetto **GeekQuiz** e scegliere **Aggiungi | Nuovo elemento con impalcatura...**

    ![Creazione di un nuovo elemento con impalcatura](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Creazione di un nuovo elemento con impalcatura")

    *Creazione di un nuovo elemento con impalcatura*
5. Nella finestra di dialogo **Aggiungi impalcatura** , verificare che il nodo **comune** sia selezionato nel riquadro sinistro. Quindi, selezionare il modello **Web API 2 controller-Empty** nel riquadro centrale e fare clic su **Aggiungi**.

    ![Selezione del modello vuoto del controller Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Selezione del modello vuoto del controller Web API 2")

    *Selezione del modello vuoto del controller Web API 2*

    > [!NOTE]
    > L' **impalcatura ASP.NET** è un Framework per la generazione di codice per le applicazioni Web ASP.NET. Visual Studio 2013 include i generatori di codice preinstallati per i progetti MVC e API Web. È consigliabile utilizzare l'impalcatura nel progetto quando si desidera aggiungere rapidamente codice che interagisce con i modelli di dati per ridurre la quantità di tempo necessaria per lo sviluppo di operazioni dati standard.
    > 
    > Il processo di impalcatura garantisce inoltre che tutte le dipendenze necessarie siano installate nel progetto. Se ad esempio si inizia con un progetto ASP.NET vuoto e quindi si usa l'impalcatura per aggiungere un controller API Web, i riferimenti e i pacchetti NuGet dell'API Web richiesti verranno aggiunti automaticamente al progetto.
6. Nella finestra di dialogo **Aggiungi controller** digitare *TriviaController* nella casella di testo **nome controller** , quindi fare clic su **Aggiungi**.

    ![Aggiunta del controller Trivia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Aggiunta del controller Trivia")

    *Aggiunta del controller Trivia*
7. Il file **TriviaController.cs** viene quindi aggiunto alla cartella **Controllers** del progetto **GeekQuiz** , contenente una classe **TriviaController** vuota. Aggiungere le istruzioni using seguenti all'inizio del file.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Aggiungere il codice seguente all'inizio della classe **TriviaController** per definire, inizializzare ed eliminare l'istanza di **TriviaContext** nel controller.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Il metodo **Dispose** di **TriviaController** richiama il metodo **Dispose** dell'istanza **TriviaContext** , che garantisce che tutte le risorse utilizzate dall'oggetto context vengano rilasciate quando l'istanza di **TriviaContext** viene eliminata o sottoposta a Garbage Collection. Ciò include la chiusura di tutte le connessioni di database aperte dal Entity Framework.
9. Aggiungere il seguente metodo helper alla fine della classe **TriviaController** . Questo metodo recupera la domanda di quiz seguente dal database a cui l'utente specificato deve rispondere.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Aggiungere il metodo **Get** Action seguente alla classe **TriviaController** . Questo metodo di azione chiama il metodo helper **NextQuestionAsync** definito nel passaggio precedente per recuperare la domanda successiva per l'utente autenticato.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Aggiungere il seguente metodo helper alla fine della classe **TriviaController** . Questo metodo archivia la risposta specificata nel database e restituisce un valore booleano che indica se la risposta è corretta.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Aggiungere il metodo **post** azione seguente alla classe **TriviaController** . Questo metodo di azione associa la risposta all'utente autenticato e chiama il metodo helper **storeAsync** . Quindi invia una risposta con il valore booleano restituito dal metodo helper.

    (Frammento di codice- *AspNetWebApiSpa-EX1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modificare il controller API Web per limitare l'accesso agli utenti autenticati aggiungendo l'attributo **autorizza** alla definizione della classe **TriviaController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si verificherà che il servizio API Web compilato nell'attività precedente funzioni come previsto. Si utilizzerà il Strumenti di sviluppo di Internet Explorer **F12** per acquisire il traffico di rete e controllare la risposta completa dal servizio API Web.

> [!NOTE]
> Assicurarsi che **Internet Explorer** sia selezionato nel pulsante **Avvia** sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Premere **F5** per eseguire la soluzione. La pagina di **accesso** verrà visualizzata nel browser.

    > [!NOTE]
    > Quando l'applicazione viene avviata, viene attivata la route MVC predefinita, che per impostazione predefinita è mappata all'azione **index** della classe **HomeController** . Poiché **HomeController** è limitato agli utenti autenticati (ricordare che la classe è stata decorata con l'attributo di **autorizzazione** nell'esercizio 1) e non è ancora stato eseguito l'autenticazione dell'utente, l'applicazione reindirizza la richiesta originale alla pagina di accesso.

    ![Esecuzione della soluzione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Esecuzione della soluzione")

    *Esecuzione della soluzione*
2. Fare clic su **registra** per creare un nuovo utente.

    ![Registrazione di un nuovo utente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registrazione di un nuovo utente")

    *Registrazione di un nuovo utente*
3. Nella pagina **Register** immettere un **nome utente** e una **password**e quindi fare clic su **Register (registra**).

    ![Pagina di registrazione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Pagina di registrazione")

    *Pagina di registrazione*
4. L'applicazione registra il nuovo account e l'utente viene autenticato e reindirizzato di nuovo al home page.

    ![L'utente è autenticato](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Utente autenticato")

    *L'utente è autenticato*
5. Nel browser premere **F12** per aprire il pannello **strumenti di sviluppo** . Premere **CTRL + 4** oppure fare clic sull'icona della **rete** , quindi fare clic sul pulsante freccia verde per avviare l'acquisizione del traffico di rete.

    ![Avvio dell'acquisizione di rete dell'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Avvio dell'acquisizione di rete dell'API Web")

    *Avvio dell'acquisizione di rete dell'API Web*
6. Aggiungere **API/Trivia** all'URL nella barra degli indirizzi del browser. Verranno ora esaminati i dettagli della risposta dal metodo **Get** Action in **TriviaController**.

    ![Recupero dei dati delle domande successive tramite l'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Recupero dei dati delle domande successive tramite l'API Web")

    *Recupero dei dati delle domande successive tramite l'API Web*

    > [!NOTE]
    > Al termine del download, verrà richiesto di eseguire un'azione con il file scaricato. Lasciare aperta la finestra di dialogo per poter controllare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.
7. A questo punto, verrà ispezionato il corpo della risposta. A tale scopo, fare clic sulla scheda **Dettagli** e quindi su **corpo della risposta**. È possibile verificare che i dati scaricati siano un oggetto con le **Opzioni** Properties, ovvero un elenco di oggetti **TriviaOption** , **ID** e **title** corrispondenti alla classe **TriviaQuestion** .

    ![Visualizzazione del corpo della risposta dell'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Visualizzazione del corpo della risposta dell'API Web")

    *Visualizzazione del corpo della risposta dell'API Web*
8. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Esercizio 2: creazione dell'interfaccia SPA

In questo esercizio verrà prima di tutto creato il componente front-end Web del quiz per geek, concentrandosi sull'interazione dell'applicazione a pagina singola con **AngularJS**. Sarà quindi possibile migliorare l'esperienza utente con CSS3 per eseguire animazioni complete e fornire un effetto visivo del cambio di contesto durante la transizione da una domanda a quella successiva.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Attività 1: creazione dell'interfaccia SPA mediante AngularJS

In questa attività si userà **AngularJS** per implementare il lato client dell'applicazione di quiz per geek. **AngularJS** è un framework JavaScript open source che consente di aumentare le applicazioni basate su browser con la funzionalità MVC ( *Model-View-Controller* ), semplificando sia lo sviluppo che il test.

Si inizierà installando AngularJS dalla console di gestione pacchetti di Visual Studio. Quindi, si creerà il controller per fornire il comportamento dell'app quiz geek e la visualizzazione per eseguire il rendering delle domande e delle risposte al quiz usando il motore di modelli AngularJS.

> [!NOTE]
> Per ulteriori informazioni su AngularJS, vedere [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Aprire **Visual Studio Express 2013 per il Web** e aprire la soluzione **GeekQuiz. sln** che si trova nella cartella **source/EX2-CreatingASPAInterface/Begin** . In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. Aprire la **console di gestione pacchetti** da **strumenti** > **Gestione pacchetti NuGet**. Digitare il comando seguente per installare il pacchetto NuGet **AngularJS. Core** .

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **script** del progetto **GeekQuiz** e scegliere **Aggiungi | Nuova cartella**. Assegnare un nome all' **app** per la cartella e premere **invio**.
4. Fare clic con il pulsante destro del mouse sulla cartella dell' **app** appena creata e scegliere **Aggiungi | File JavaScript**.

    ![Creazione di un nuovo file JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Creazione di un nuovo file JavaScript*
5. Nella finestra di dialogo **Specifica nome per l'elemento** Digitare *quiz-controller* nella casella di testo **nome elemento** e fare clic su **OK**.

    ![Denominazione del nuovo file JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Denominazione del nuovo file JavaScript*
6. Nel file **quiz-controller. js** aggiungere il codice seguente per dichiarare e inizializzare il controller **QuizCtrl** AngularJS.

    (Frammento di codice- *AspNetWebApiSpa-EX2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La funzione del costruttore del controller **QuizCtrl** prevede un parametro iniettabile denominato **$scope**. Lo stato iniziale dell'ambito deve essere configurato nella funzione del costruttore mediante l'associazione di proprietà all'oggetto **$scope** . Le proprietà contengono il **modello di visualizzazione**e saranno accessibili al modello quando il controller viene registrato.
    > 
    > Il controller **QuizCtrl** viene definito all'interno di un modulo denominato **QuizApp**. I moduli sono unità di lavoro che consentono di suddividere l'applicazione in componenti separati. I principali vantaggi derivanti dall'uso dei moduli è che il codice è più semplice da comprendere e facilita il testing unità, la riusabilità e la gestibilità.
7. A questo punto si aggiungerà il comportamento all'ambito per rispondere agli eventi generati dalla visualizzazione. Aggiungere il codice seguente alla fine del controller **QuizCtrl** per definire la funzione **nextQuestion** nell'oggetto **$scope** .

    (Frammento di codice- *AspNetWebApiSpa-EX2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Questa funzione recupera la domanda successiva dall'API Web di **Trivia** creata nell'esercizio precedente e connette i dati della domanda all'oggetto **$scope** .
8. Inserire il codice seguente alla fine del controller **QuizCtrl** per definire la funzione **sendAnswer** nell'oggetto **$scope** .

    (Frammento di codice- *AspNetWebApiSpa-EX2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Questa funzione Invia la risposta selezionata dall'utente all'API Web **Trivia** e archivia il risultato, ad esempio se la risposta è corretta o meno, nell'oggetto **$scope** .
    > 
    > Le funzioni **nextQuestion** e **sendAnswer** della versione precedente usano l'oggetto AngularJS **$http** per astrarre la comunicazione con l'API Web tramite l'oggetto XMLHttpRequest JavaScript dal browser. AngularJS supporta un altro servizio che offre un livello più elevato di astrazione per l'esecuzione di operazioni CRUD su una risorsa tramite le API RESTful. L'oggetto AngularJS **$Resource** dispone di metodi di azione che forniscono comportamenti di alto livello senza la necessità di interagire con l'oggetto **$http** . Si consiglia di usare l'oggetto **$Resource** in scenari che richiedono il modello CRUD (informazioni di primo piano, vedere la [documentazione $Resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Il passaggio successivo consiste nel creare il modello AngularJS che definisce la visualizzazione per il quiz. A tale scopo, aprire il file **index. cshtml** all'interno delle **visualizzazioni | Home** directory e sostituire il contenuto con il codice seguente.

    (Frammento di codice- *AspNetWebApiSpa-EX2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Il modello AngularJS è una specifica dichiarativa che usa le informazioni del modello e il controller per trasformare il markup statico nella visualizzazione dinamica visualizzata dall'utente nel browser. Di seguito sono riportati esempi di elementi AngularJS ed attributi di elementi che possono essere utilizzati in un modello:
    > 
    > - La direttiva **ng-app** indica a AngularJS l'elemento DOM che rappresenta l'elemento radice dell'applicazione.
    > - La direttiva **ng-controller** connette un controller al Dom nel punto in cui viene dichiarata la direttiva.
    > - La notazione parentesi graffa **{{}}** indica le associazioni alle proprietà dell'ambito definite nel controller.
    > - La direttiva **ng-click** viene utilizzata per richiamare le funzioni definite nell'ambito in risposta ai clic dell'utente.
10. Aprire il file **site. CSS** all'interno della cartella **Content** e aggiungere gli stili evidenziati seguenti alla fine del file per fornire un aspetto per la visualizzazione quiz.

    (Frammento di codice- *AspNetWebApiSpa-EX2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verrà eseguita la soluzione usando la nuova interfaccia utente creata con AngularJS per rispondere ad alcune domande sul quiz.

1. Premere **F5** per eseguire la soluzione.
2. Registrare un nuovo account utente. A tale scopo, seguire la procedura di registrazione descritta in esercizio 1, attività 3.

    > [!NOTE]
    > Se si usa la soluzione dell'esercizio precedente, è possibile accedere con l'account utente creato in precedenza.
3. Verrà visualizzata la **Home** page, che mostra la prima domanda del quiz. Per rispondere alla domanda, fare clic su una delle opzioni. Verrà attivata la funzione **sendAnswer** definita in precedenza, che invia l'opzione selezionata all'API Web **Trivia** .

    ![Risposta a una domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Risposta a una domanda")

    *Risposta a una domanda*
4. Dopo aver fatto clic su uno dei pulsanti, viene visualizzata la risposta. Fare clic su **domanda successiva** per visualizzare la domanda seguente. Verrà attivata la funzione **nextQuestion** definita nel controller.

    ![Richiesta della domanda successiva](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Richiesta della domanda successiva")

    *Richiesta della domanda successiva*
5. Verrà visualizzata la domanda successiva. Continuare a rispondere alle domande tutte le volte che si desidera. Dopo aver completato tutte le domande, è necessario tornare alla prima domanda.

    ![Un'altra domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Un'altra domanda")

    *Prossima domanda*
6. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Attività 3: creazione di un'animazione flip con CSS3

In questa attività si useranno le proprietà CSS3 per eseguire animazioni complete aggiungendo un effetto flip quando viene fornita una risposta a una domanda e quando viene recuperata la domanda successiva.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **contenuto** del progetto **GeekQuiz** e scegliere **Aggiungi | Elemento esistente...**

    ![Aggiunta di un elemento esistente alla cartella Content](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Aggiunta di un elemento esistente alla cartella Content")

    *Aggiunta di un elemento esistente alla cartella Content*
2. Nella finestra di dialogo **Aggiungi elemento esistente** passare alla cartella **source/assets** e selezionare **flip. CSS**. Fare clic su **Aggiungi**.

    ![Aggiunta del file Flip. CSS dagli asset](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Aggiunta del file Flip. CSS dagli asset")

    *Aggiunta del file Flip. CSS dagli asset*
3. Aprire il file **flip. CSS** appena aggiunto ed esaminarne il contenuto.
4. Individuare il commento della **trasformazione Capovolgi** . Gli stili riportati di seguito utilizzano la **prospettiva** CSS e le trasformazioni **rotative** per generare un &quot;Flip&quot; effetto della scheda.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Individuare il **riquadro Nascondi indietro durante** il commento flip. Lo stile sotto il commento nasconde il lato posteriore delle facce quando si trovano al di fuori del Visualizzatore impostando la proprietà CSS **backface-visibility** su *Hidden*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Aprire il file **BundleConfig.cs** all'interno dell' **app\_** cartella di avvio e aggiungere il riferimento al file **flip. css** nel bundle di stile **&quot;~/content/CSS&quot;**

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Premere **F5** per eseguire la soluzione e accedere con le proprie credenziali.
8. Per rispondere a una domanda, fare clic su una delle opzioni. Si noti l'effetto flip durante la transizione tra le visualizzazioni.

    ![Risposta a una domanda con effetto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Risposta a una domanda con effetto flip")

    *Risposta a una domanda con effetto flip*
9. Fare clic su **Next question** per recuperare la domanda seguente. L'effetto flip dovrebbe essere visualizzato di nuovo.

    ![Recupero della domanda seguente con effetto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Recupero della domanda seguente con effetto flip")

    *Recupero della domanda seguente con effetto flip*

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Creare un controller di API Web ASP.NET usando l'impalcatura ASP.NET
- Implementare un'azione Get dell'API Web per recuperare la domanda successiva del quiz
- Implementare un'azione post per l'API Web per archiviare le risposte al quiz
- Installare AngularJS dalla console di gestione pacchetti di Visual Studio
- Implementare modelli e controller AngularJS
- Usare le transizioni CSS3 per eseguire gli effetti di animazione
