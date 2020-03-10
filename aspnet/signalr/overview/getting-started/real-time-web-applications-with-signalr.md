---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratorio pratico: applicazioni Web in tempo reale con SignalR | Microsoft Docs'
author: bradygaster
description: Le applicazioni Web in tempo reale offrono la possibilità di eseguire il push del contenuto lato server ai client connessi come avviene, in tempo reale. Per gli sviluppatori ASP.NET, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537100"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratorio pratico: applicazioni Web in tempo reale con SignalR

dal [team di Web Camp](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Scarica il kit di formazione di Web Camps, versione ottobre 2015](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> Le applicazioni Web in tempo reale offrono la possibilità di eseguire il push del contenuto lato server ai client connessi come avviene, in tempo reale. Per gli sviluppatori ASP.NET, **ASP.NET SignalR** è una libreria che consente di aggiungere funzionalità Web in tempo reale alle applicazioni. Sfrutta i vantaggi di diversi trasporti, selezionando automaticamente il migliore trasporto disponibile, dato il migliore trasporto disponibile per il client e il server. Sfrutta i vantaggi di **WebSocket**, un'API HTML5 che consente la comunicazione bidirezionale tra il browser e il server.
> 
> **SignalR** fornisce anche un'API semplice e di alto livello per eseguire la RPC da server a client (chiamare le funzioni JavaScript nei browser dei client dal codice .NET lato server) nell'applicazione ASP.NET, nonché aggiungere utili hook per la gestione delle connessioni, ad esempio gli eventi di connessione/disconnessione, le connessioni di raggruppamento e l'autorizzazione.
> 
> **SignalR** è un'astrazione di alcuni dei trasporti necessari per eseguire operazioni in tempo reale tra client e server. Una connessione **SignalR** viene avviata come http e viene quindi promossa a una connessione **WebSocket** , se disponibile. **WebSocket** è il trasporto ideale per **SignalR**, dal momento che rende più efficiente l'uso della memoria del server, presenta la latenza più bassa e presenta le funzionalità più sottostanti (ad esempio, la comunicazione duplex completa tra client e server), ma presenta anche i requisiti più severi: **WebSocket** richiede che il server usi **Windows server 2012** o **Windows 8**insieme a **.NET Framework 4,5**. Se questi requisiti non vengono soddisfatti, **SignalR** tenterà di usare altri trasporti per effettuare le connessioni (ad esempio, il *polling lungo Ajax*).
> 
> L'API **SignalR** contiene due modelli per la comunicazione tra client e server: connessioni e **Hub** **permanenti** . Una **connessione** rappresenta un semplice endpoint per l'invio di messaggi a destinatario singolo, raggruppati o trasmessi. Un **Hub** è una pipeline più elevata basata sull'API di connessione che consente al client e al server di chiamare direttamente i metodi tra loro.
> 
> ![Architettura SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, versione ottobre 2015, disponibile all' [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b).  Si noti che il collegamento del programma di installazione nella pagina non funziona più; usare invece uno dei collegamenti nella sezione asset.

<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico si apprenderà come:

- Inviare notifiche dal server al client usando SignalR.
- Scale Out l'applicazione SignalR usando **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione pratica, è necessario quanto segue:

- [Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/) o versione successiva

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.

1. Aprire una finestra di Esplora risorse e passare alla cartella di **origine** del Lab.
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

1. [Utilizzo di dati in tempo reale tramite SignalR](#Exercise1)
2. [Scalabilità orizzontale con SQL Server](#Exercise2)

Tempo stimato per il completamento del Lab: **60 minuti**

> [!NOTE]
> Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** . Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Esercizio 1: utilizzo di dati in tempo reale tramite SignalR

Sebbene la chat venga spesso utilizzata come esempio, è possibile eseguire molte altre operazioni con la funzionalità Web in tempo reale. Ogni volta che un utente aggiorna una pagina Web per visualizzare nuovi dati o la pagina implementa il polling lungo Ajax per recuperare i nuovi dati, è possibile usare SignalR.

SignalR supporta la funzionalità di push o **broadcast** del **Server** . gestisce automaticamente la gestione delle connessioni. Nelle connessioni HTTP classiche per la comunicazione client-server, viene ristabilita la connessione per ogni richiesta, ma SignalR fornisce una connessione permanente tra il client e il server. In SignalR il codice server chiama un codice client nel browser usando RPC (Remote Procedure Call), anziché il modello di richiesta-risposta attualmente noto.

In questo esercizio verrà configurata l'applicazione di **quiz geek** per l'uso di SignalR per visualizzare il Dashboard delle statistiche con le metriche aggiornate senza dover aggiornare l'intera pagina.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Attività 1: esplorazione della pagina statistiche quiz di geek

In questa attività si procederà attraverso l'applicazione e si verificherà come viene visualizzata la pagina statistiche e come è possibile migliorare il modo in cui vengono aggiornate le informazioni.

1. Aprire **Visual Studio Express 2013 per il Web** e aprire la soluzione **GeekQuiz. sln** disponibile nella cartella **Source\Ex1-WorkingWithRealTimeData\Begin** .
2. Premere **F5** per eseguire la soluzione. La pagina di **accesso** verrà visualizzata nel browser.

    ![Esecuzione della soluzione](real-time-web-applications-with-signalr/_static/image2.png "Esecuzione della soluzione")

    *Esecuzione della soluzione*
3. Fare clic su **registra** nell'angolo superiore destro della pagina per creare un nuovo utente nell'applicazione.

    ![Registra collegamento](real-time-web-applications-with-signalr/_static/image3.png "Registra collegamento")

    *Registra collegamento*
4. Nella pagina **Register** immettere un **nome utente** e una **password**e quindi fare clic su **Register (registra**).

    ![Registrazione di un utente](real-time-web-applications-with-signalr/_static/image4.png "Registrazione di un utente")

    *Registrazione di un utente*
5. L'applicazione registra il nuovo account e l'utente viene autenticato e reindirizzato di nuovo all'home page che mostra la prima domanda del quiz.
6. Aprire la pagina **statistiche** in una nuova finestra e inserire la pagina **iniziale** e la pagina **statistiche** affiancata.

    ![Finestre affiancate](real-time-web-applications-with-signalr/_static/image5.png "Finestre affiancate")

    *Finestre affiancate*
7. Nella **Home** page, fare clic su una delle opzioni per rispondere alla domanda.

    ![Risposta a una domanda](real-time-web-applications-with-signalr/_static/image6.png "Risposta a una domanda")

    *Risposta a una domanda*
8. Dopo aver fatto clic su uno dei pulsanti, viene visualizzata la risposta.

    ![Risposta alla domanda corretta](real-time-web-applications-with-signalr/_static/image7.png "Risposta alla domanda corretta")

    *Risposta corretta alla domanda*
9. Si noti che le informazioni fornite nella pagina statistiche sono obsolete. Aggiornare la pagina per visualizzare i risultati aggiornati.

    ![Pagina statistiche](real-time-web-applications-with-signalr/_static/image8.png "Pagina statistiche")

    *Pagina statistiche*
10. Tornare a Visual Studio e arrestare il debug.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Attività 2: aggiunta di SignalR a quiz per geek per visualizzare i grafici online

In questa attività si aggiungerà SignalR alla soluzione e si invieranno gli aggiornamenti ai client automaticamente quando viene inviata una nuova risposta al server.

1. Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**e quindi fare clic su **console di gestione pacchetti**.
2. Nella finestra **console di gestione pacchetti** eseguire il comando seguente:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Installazione del pacchetto SignalR](real-time-web-applications-with-signalr/_static/image9.png "Installazione del pacchetto SignalR")

    *Installazione del pacchetto SignalR*

   > [!NOTE]
   > Quando si installano i pacchetti NuGet di **SignalR** versione 2.0.2 da una nuova applicazione MVC 5, sarà necessario aggiornare manualmente i pacchetti **OWIN** alla versione 2.0.1 (o versione successiva) prima di installare SignalR. A tale scopo, è possibile eseguire lo script seguente nella **console di gestione pacchetti**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > In una versione futura di SignalR le dipendenze di OWIN verranno aggiornate automaticamente.
3. In **Esplora soluzioni**espandere la cartella **Scripts** e notare che i file di SignalR *JS* sono stati aggiunti alla soluzione.

    ![Riferimenti JavaScript a SignalR](real-time-web-applications-with-signalr/_static/image10.png "Riferimenti JavaScript a SignalR")

    *Riferimenti JavaScript a SignalR*
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **GeekQuiz** , selezionare **Aggiungi** | **nuova cartella**e denominarla **Hub**.
5. Fare clic con il pulsante destro del mouse sulla cartella **Hub** e scegliere **Aggiungi | Nuovo elemento**.

    ![Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image11.png "Aggiungi nuovo elemento")

    *Aggiungi nuovo elemento*
6. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare l'oggetto **visivo C# | Web | Nodo SignalR** nel riquadro sinistro, selezionare **classe Hub SignalR (v2)** dal riquadro centrale, denominare il file **StatisticsHub.cs** e fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image12.png "Finestra di dialogo Aggiungi nuovo elemento")

    *Finestra di dialogo Aggiungi nuovo elemento*
7. Sostituire il codice nella classe **StatisticsHub** con il codice seguente.

    (Frammento di codice- *RealTimeSignalR-EX1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Aprire **Startup.cs** e aggiungere la riga seguente alla fine del metodo di **configurazione** .

    (Frammento di codice- *RealTimeSignalR-EX1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Aprire la pagina **StatisticsService.cs** all'interno della cartella **Servizi** e aggiungere le seguenti direttive using.

    (Frammento di codice- *RealTimeSignalR-EX1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Per notificare ai client connessi gli aggiornamenti, è innanzitutto necessario recuperare un oggetto di **contesto** per la connessione corrente. L'oggetto **Hub** contiene i metodi per inviare messaggi a un singolo client o trasmettere a tutti i client connessi. Aggiungere il metodo seguente alla classe **StatisticsService** per trasmettere i dati delle statistiche.

    (Frammento di codice- *RealTimeSignalR-EX1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Nel codice precedente si usa un nome di metodo arbitrario per chiamare una funzione sul client (ad esempio: *UpdateStatistics*). Il nome del metodo specificato viene interpretato come oggetto dinamico, il che significa che non è presente alcuna convalida di IntelliSense o della fase di compilazione. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori del parametro al client. Se il client dispone di un metodo che corrisponde al nome, viene chiamato il metodo e vengono passati i valori dei parametri. Se non viene trovato alcun metodo corrispondente nel client, non viene generato alcun errore. Per altre informazioni, vedere la [Guida dell'API degli hub SignalR di ASP.NET](../guide-to-the-api/hubs-api-guide-server.md).
11. Aprire la pagina **TriviaController.cs** all'interno della cartella **Controllers** e aggiungere le seguenti direttive using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Aggiungere il codice evidenziato seguente al metodo **post** -azione.

    (Frammento di codice- *RealTimeSignalR-EX1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Aprire la pagina **Statistics. cshtml** all'interno delle **visualizzazioni | Home** directory. Individuare la sezione **Scripts** e aggiungere i riferimenti allo script seguenti all'inizio della sezione.

    (Frammento di codice- *RealTimeSignalR-EX1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, gestione pacchetti potrebbe installare una versione del file di script SignalR più recente della versione indicata in questo argomento. Verificare che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.
14. Aggiungere il codice evidenziato seguente per connettere il client all'hub SignalR e aggiornare i dati delle statistiche quando viene ricevuto un nuovo messaggio dall'hub.

    (Frammento di codice- *RealTimeSignalR-EX1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    In questo codice si crea un proxy Hub e si registra un gestore eventi per ascoltare i messaggi inviati dal server. In questo caso, si è in ascolto dei messaggi inviati tramite il metodo *UpdateStatistics* .

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività verrà eseguita la soluzione per verificare che la visualizzazione Statistiche venga aggiornata automaticamente usando SignalR dopo aver risposto a una nuova domanda.

1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Se non è già stato effettuato l'accesso all'applicazione, effettuare l'accesso con l'utente creato nell'attività 1.
2. Aprire la pagina delle **statistiche** in una nuova finestra e inserire la pagina **iniziale** e le **statistiche** affiancate come nell'attività 1.
3. Nella **Home** page, fare clic su una delle opzioni per rispondere alla domanda.

    ![Risposta a un'altra domanda](real-time-web-applications-with-signalr/_static/image13.png "Risposta a un'altra domanda")

    *Risposta a un'altra domanda*
4. Dopo aver fatto clic su uno dei pulsanti, viene visualizzata la risposta. Si noti che le informazioni statistiche sulla pagina vengono aggiornate automaticamente dopo aver risposto alla domanda con le informazioni aggiornate senza dover aggiornare l'intera pagina.

    ![Pagina statistiche aggiornata dopo la risposta](real-time-web-applications-with-signalr/_static/image14.png "Pagina statistiche aggiornata dopo la risposta")

    *Pagina statistiche aggiornata dopo la risposta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Esercizio 2: scalabilità orizzontale con SQL Server

Quando si ridimensiona un'applicazione Web, è in genere possibile scegliere tra le opzioni di *scalabilità verticale* e *orizzontale* . La *scalabilità verticale* indica l'uso di un server di dimensioni maggiori, con più risorse (CPU, RAM e così via), mentre la *scalabilità orizzontale* significa aggiungere altri server per gestire il carico. Il problema con quest'ultimo è che i client possono essere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

È possibile risolvere questi problemi utilizzando un componente denominato *backplane*, per l'invio di messaggi tra server. Con un backplane abilitato, ogni istanza dell'applicazione invia messaggi al backplane e il backplane li inoltra alle altre istanze dell'applicazione.

Esistono attualmente tre tipi di BACKPLAN per SignalR:

- **Bus di servizio di Microsoft Azure**. Il bus di servizio è un'infrastruttura di messaggistica che consente ai componenti di inviare messaggi a regime di controllo libero.
- **SQL Server**. Il backplane SQL Server scrive i messaggi nelle tabelle SQL. Il backplane USA Service Broker per la messaggistica efficiente. Tuttavia, funziona anche se Service Broker non è abilitato.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello di pubblicazione/sottoscrizione ("pub/sub") per l'invio di messaggi.

Ogni messaggio viene inviato tramite un bus di messaggi. Un bus di messaggi implementa l'interfaccia [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , che fornisce un'astrazione di pubblicazione/sottoscrizione. Il backplans funziona sostituendo il **IMessageBus** predefinito con un bus progettato per tale backplane.

Ogni istanza del server si connette al backplane tramite il bus. Quando un messaggio viene inviato, passa al backplane e il backplane lo invia a ogni server. Quando un server riceve un messaggio dal backplane, archivia il messaggio nella cache locale. Il server recapita quindi i messaggi ai client dalla cache locale.

Per ulteriori informazioni sul funzionamento del backplane SignalR, leggere questo [articolo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Esistono scenari in cui un backplane può diventare un collo di bottiglia. Ecco alcuni scenari tipici di SignalR:
> 
> - [Broadcast del server](tutorial-server-broadcast-with-signalr.md) (ad esempio, Stock): i BACKPLAN sono adatti a questo scenario, perché il server controlla la velocità con cui vengono inviati i messaggi.
> - [Da client a client](tutorial-getting-started-with-signalr.md) (ad esempio, chat): in questo scenario, il backplane può costituire un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client; ovvero, se la frequenza dei messaggi aumenta proporzionalmente Man mano che si aggiungono altri client.
> - [Tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) (ad esempio, giochi in tempo reale): un backplane non è consigliato per questo scenario.

In questo esercizio si userà **SQL Server** per distribuire i messaggi nell'applicazione di **quiz per geek** . Queste attività vengono eseguite in un singolo computer di test per apprendere come configurare la configurazione, ma per ottenere l'effetto completo sarà necessario distribuire l'applicazione SignalR a due o più server. È inoltre necessario installare SQL Server su uno dei server o su un server dedicato separato.

![Scale Out con SQL Server diagramma](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Attività 1: informazioni sullo scenario

In questa attività verranno eseguite 2 istanze di **quiz geek** simulando più istanze di IIS nel computer locale. In questo scenario, quando si rispondono a domande semplici su un'applicazione, l'aggiornamento non verrà notificato nella pagina statistiche della seconda istanza. Questa simulazione è simile a un ambiente in cui l'applicazione viene distribuita in più istanze e usando un servizio di bilanciamento del carico per comunicare con loro.

1. Aprire la soluzione **Begin. sln** disponibile nella cartella **source/EX2-ScalingOutWithSQLServer/Begin** . Una volta caricato, si noterà che nella **Esplora server** che la soluzione contiene due progetti con strutture identiche ma nomi diversi. Verrà simulata l'esecuzione di due istanze della stessa applicazione nel computer locale.

    ![Inizia la simulazione della soluzione 2 istanze di quiz geek](real-time-web-applications-with-signalr/_static/image16.png "Inizia la simulazione della soluzione 2 istanze di quiz geek")

    *Inizia la simulazione della soluzione 2 istanze di quiz geek*
2. Per aprire la pagina delle proprietà della soluzione, fare clic con il pulsante destro del mouse sul nodo della soluzione e scegliere **Proprietà**. In **progetto di avvio**selezionare **progetti di avvio multipli** e modificare il valore dell' **azione** per entrambi i progetti su *Avvia*.

    ![Avvio di più progetti](real-time-web-applications-with-signalr/_static/image17.png "Avvio di più progetti")

    *Avvio di più progetti*
3. Premere **F5** per eseguire la soluzione. L'applicazione avvierà due istanze di **quiz geek** in porte diverse, simulando più istanze della stessa applicazione. Aggiungere uno dei browser a sinistra e l'altro a destra dello schermo. Accedere con le proprie credenziali o registrare un nuovo utente. Una volta effettuato l'accesso, Mantieni la pagina di Trivia a sinistra e vai alla pagina delle **statistiche** nel browser a destra.

    ![Quiz geek side-by-side](real-time-web-applications-with-signalr/_static/image18.png)

    *Quiz geek side-by-side*

    ![Quiz per geek in porte diverse](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz per geek in porte diverse*
4. Iniziare a rispondere alle domande nel browser a sinistra. si noterà che la pagina **statistiche** nel browser a destra non è in fase di aggiornamento. Questo perché **SignalR** usa una cache locale per distribuire i messaggi tra i client e questo scenario simula più istanze, quindi la cache non viene condivisa tra di essi. È possibile verificare che **SignalR** funzioni verificando gli stessi passaggi, ma usando una singola app. Nelle attività seguenti viene configurato un backplane per replicare i messaggi tra le istanze.
5. Tornare a Visual Studio e arrestare il debug.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Attività 2: creazione del backplane SQL Server

In questa attività verrà creato un database che fungerà da backplane per l'applicazione di **quiz per geek** . Si utilizzerà **Esplora oggetti di SQL Server** per esplorare il server e inizializzare il database. Inoltre, sarà possibile abilitare il **Service Broker**.

1. In **Visual Studio**aprire la **visualizzazione** menu e selezionare **Esplora oggetti di SQL Server**.
2. Connettersi all'istanza del database locale facendo clic con il pulsante destro del mouse sul nodo **SQL Server** e selezionando l'opzione **Aggiungi SQL Server...** .

    ![Aggiunta di un'istanza di SQL Server](real-time-web-applications-with-signalr/_static/image20.png "Aggiunta di un'istanza di SQL Server")

    *Aggiunta di un'istanza di SQL Server a Esplora oggetti di SQL Server*
3. Impostare il **nome del server** su (local DB *) \V11.0* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connetti** per continuare.

    ![Connessione al database locale](real-time-web-applications-with-signalr/_static/image21.png "Connessione al database locale")

    *Connessione al database locale*
4. Ora che si è connessi all'istanza del database locale, è necessario creare un database che rappresenterà il backplane SQL Server per SignalR. A tale scopo, fare clic con il pulsante destro del mouse sul nodo **database** e scegliere **Aggiungi nuovo database**.

    ![Aggiunta di un nuovo database](real-time-web-applications-with-signalr/_static/image22.png "Aggiunta di un nuovo database")

    *Aggiunta di un nuovo database*
5. Impostare il nome del database su *SignalR* e fare clic su **OK** per crearlo.

    ![Creazione del database SignalR](real-time-web-applications-with-signalr/_static/image23.png "Creazione del database SignalR")

    *Creazione del database SignalR*

    > [!NOTE]
    > È possibile scegliere qualsiasi nome per il database.
6. Per ricevere gli aggiornamenti in modo più efficiente dal backplane, è consigliabile abilitare Service Broker per il database. Service Broker fornisce il supporto nativo per la messaggistica e l'accodamento in SQL Server. Il backplane funziona anche senza Service Broker. Per aprire una nuova query, fare clic con il pulsante destro del mouse sul database e scegliere **nuova query**.

    ![Apertura di una nuova query](real-time-web-applications-with-signalr/_static/image24.png "Apertura di una nuova query")

    *Apertura di una nuova query*
7. Per verificare se la Service Broker è abilitata, eseguire una query sulla colonna **\_Broker\_abilitato** nella vista del catalogo **sys. databases** . Eseguire lo script seguente nella finestra di query aperta di recente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Esecuzione di query sullo stato di Service Broker](real-time-web-applications-with-signalr/_static/image25.png "Esecuzione di query sullo stato di Service Broker")

    *Esecuzione di query sullo stato di Service Broker*
8. Se il valore della colonna **\_broker\_abilitato** nel database è &quot;0&quot;, utilizzare il comando seguente per abilitarlo. Sostituire **&lt;&gt;del database** con il nome impostato durante la creazione del database (ad esempio, SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Abilitazione di Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Abilitazione di Service Broker")

    *Abilitazione di Service Broker*

    > [!NOTE]
    > Se questa query viene visualizzata come deadlock, assicurarsi che non vi siano applicazioni connesse al database.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Attività 3: configurazione dell'applicazione SignalR

In questa attività verrà configurato il **quiz geek** per la connessione al backplane SQL Server. Viene innanzitutto aggiunto il pacchetto NuGet **SignalR. SqlServer** e la stringa di connessione viene impostata sul database backplane.

1. Aprire la **console di gestione pacchetti** da **strumenti** > **Gestione pacchetti NuGet**. Verificare che il progetto **GeekQuiz** sia selezionato nell'elenco a discesa **progetto predefinito** . Digitare il comando seguente per installare il pacchetto NuGet **Microsoft. AspNet. SignalR. SqlServer** .

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Ripetere il passaggio precedente, ma questa volta per il progetto **GeekQuiz2**.
3. Per configurare il backplane di SQL Server, aprire il file **Startup.cs** del progetto **GeekQuiz** e aggiungere il codice seguente al metodo **Configure** . Sostituire **&lt;&gt;del database** con il nome del database usato durante la creazione del backplane SQL Server. Ripetere questo passaggio per il progetto **GeekQuiz2** .

    (Frammento di codice- *RealTimeSignalR-EX2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Ora che entrambi i progetti sono configurati per l'uso del backplane SQL Server, premere **F5** per eseguirli simultaneamente.
5. Anche in questo caso, **Visual Studio** avvierà due istanze di **quiz geek** in porte diverse. Aggiungere uno dei browser a sinistra e l'altro a destra dello schermo e accedere con le proprie credenziali. Mantieni la pagina dei Trivia a sinistra e vai alla pagina delle **statistiche** nel browser a destra.
6. Iniziare a rispondere alle domande nel browser a sinistra. Questa volta, la pagina delle **statistiche** viene aggiornata grazie al backplane. Passare da un'applicazione all'altra (le**statistiche** sono ora a sinistra e la **curiosità** è a destra) e ripetere il test per verificare che funzioni per entrambe le istanze. Il backplane funge da *cache condivisa* dei messaggi per ogni server connesso e ogni server archivia i messaggi nella propria cache locale per la distribuzione ai client connessi.
7. Tornare a Visual Studio e arrestare il debug.
8. Il componente SQL Server backplane genera automaticamente le tabelle necessarie nel database specificato. Nel pannello **Esplora oggetti di SQL Server** aprire il database creato per il backplane (ad esempio, SignalR) ed espandere le relative tabelle. Verranno visualizzate le tabelle seguenti:

    ![Tabelle generate backplane](real-time-web-applications-with-signalr/_static/image27.png)

    *Tabelle generate backplane*
9. Fare clic con il pulsante destro del mouse sulla tabella **SignalR. messages\_0** e selezionare **Visualizza dati**.

    ![Visualizzazione della tabella dei messaggi del backplane SignalR](real-time-web-applications-with-signalr/_static/image28.png)

    *Visualizzazione della tabella dei messaggi del backplane SignalR*
10. È possibile visualizzare i diversi messaggi inviati all' **Hub** quando si rispondono alle domande di Trivia. Il backplane distribuisce questi messaggi a qualsiasi istanza connessa.

    ![Tabella messaggi backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabella messaggi backplane*

---

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico si è appreso come aggiungere **SignalR** all'applicazione e inviare notifiche dal server ai client connessi usando gli **Hub**. Si è inoltre appreso come aumentare la scalabilità orizzontale dell'applicazione usando un componente *backplane* quando l'applicazione viene distribuita in più istanze di IIS.
