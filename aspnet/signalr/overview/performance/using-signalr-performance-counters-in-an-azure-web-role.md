---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Uso dei contatori delle prestazioni di SignalR in un ruolo Web di Azure | Microsoft Docs
author: guardrex
description: Come installare e usare i contatori delle prestazioni di SignalR in un ruolo Web di Azure.
keywords: ASP. NET, SignalR, contatore delle prestazioni, ruolo Web di Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578981"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Uso dei contatori delle prestazioni di SignalR in un ruolo Web di Azure

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

I contatori delle prestazioni di SignalR vengono usati per monitorare le prestazioni dell'app in un ruolo Web di Azure. I contatori vengono acquisiti da Diagnostica di Microsoft Azure. È possibile installare i contatori delle prestazioni di SignalR in Azure con *SignalR. exe*, lo stesso strumento usato per le app autonome o locali. Poiché i ruoli di Azure sono temporanei, è possibile configurare un'app per installare e registrare i contatori delle prestazioni di SignalR all'avvio.

## <a name="prerequisites"></a>Prerequisiti

* Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK per Visual Studio](https://azure.microsoft.com/downloads/) **Nota: riavviare il computer dopo l'installazione dell'SDK.**
* Sottoscrizione di Microsoft Azure: per iscriversi per ottenere un account di prova di Azure gratuito, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Creazione di un'applicazione del ruolo Web di Azure che espone i contatori delle prestazioni di SignalR

1. Aprire Visual Studio.

2. In Visual Studio selezionare **File** > **Nuovo** > **Progetto**.

3. Nella finestra di dialogo **nuovo progetto** selezionare la categoria **Visual C#**  > **cloud** a sinistra e quindi selezionare il modello **servizio cloud di Azure** . Assegnare all'app il nome **SignalRPerfCounters** e selezionare **OK**.

   ![Nuova applicazione cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Se non viene visualizzata la categoria del modello **cloud** o il modello di **servizio cloud di Azure** , è necessario installare il carico di lavoro di **sviluppo di Azure** per Visual Studio 2017. Scegliere il collegamento **apri programma di installazione di Visual Studio** nel lato inferiore sinistro della finestra di dialogo **nuovo progetto** per aprire programma di installazione di Visual Studio. Selezionare il carico di lavoro **sviluppo di Azure** , quindi scegliere **modifica** per avviare l'installazione del carico di lavoro.
   >
   > ![Carico di lavoro di sviluppo di Azure in Programma di installazione di Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. Nella finestra di dialogo **nuovo servizio Cloud Microsoft Azure** selezionare **ruolo Web ASP.NET** e selezionare il pulsante > per aggiungere il ruolo al progetto. Selezionare **OK**.

   ![Aggiungi ruolo Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. Nella finestra di dialogo **nuova applicazione Web ASP.NET-WebRole1** selezionare il modello **MVC** e quindi fare clic su **OK**.

   ![Aggiungere MVC e API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. In **Esplora soluzioni**aprire il file *Diagnostics. wadcfgx* in **WebRole1**.

   ![Esplora soluzioni Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Sostituire il contenuto del file con la configurazione seguente e salvare il file:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Aprire la **console di gestione pacchetti** da **strumenti** > **Gestione pacchetti NuGet**. Immettere i comandi seguenti per installare la versione più recente di SignalR e il pacchetto delle utilità SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Configurare l'app per installare i contatori delle prestazioni di SignalR nell'istanza del ruolo all'avvio o al riciclo. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WebRole1** e scegliere **Aggiungi** > **nuova cartella**. Assegnare un nome all' *avvio*della nuova cartella.

   ![Aggiungi cartella di avvio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Copiare il file *SignalR. exe* (aggiunto con il pacchetto **Microsoft. AspNet. SignalR. Utils** ) da \<cartella del progetto >/SignalRPerfCounters/Packages/Microsoft.AspNet.SignalR.utils.\<versione >/Tools alla cartella di *avvio* creata nel passaggio precedente.

11. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella di *avvio* e scegliere **Aggiungi** > **elemento esistente**. Nella finestra di dialogo visualizzata selezionare *SignalR. exe* e selezionare **Aggiungi**.

    ![Aggiungere SignalR. exe al progetto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Fare clic con il pulsante destro del mouse sulla cartella di *avvio* creata. Selezionare **Aggiungi** > **Nuovo elemento**. Selezionare il nodo **generale** , selezionare **file di testo**e denominare il nuovo elemento *SignalRPerfCounterInstall. cmd*. Questo file di comando installerà i contatori delle prestazioni di SignalR nel ruolo Web.

    ![Crea il file batch di installazione del contatore delle prestazioni SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Quando Visual Studio crea il file *SignalRPerfCounterInstall. cmd* , viene aperto automaticamente nella finestra principale. Sostituire il contenuto del file con lo script seguente, quindi salvare e chiudere il file. Questo script esegue *SignalR. exe*, che aggiunge i contatori delle prestazioni di SignalR all'istanza del ruolo.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Selezionare il file *SignalR. exe* in **Esplora soluzioni**. Nelle **Proprietà**del file impostare copia nella **directory di output** su **copia sempre**.

    ![Imposta copia nella directory di output su copia sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Ripetere il passaggio precedente per il file *SignalRPerfCounterInstall. cmd* .

16. Fare clic con il pulsante destro del mouse sul file *SignalRPerfCounterInstall. cmd* e scegliere **Apri con**. Nella finestra di dialogo visualizzata selezionare **editor binario** e quindi fare clic su **OK**.

    ![Apri con editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. Nell'editor binario selezionare tutti i byte iniziali nel file ed eliminarli. Salvare e chiudere il file.

    ![Elimina byte iniziali](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Aprire *file ServiceDefinition. csdef* e aggiungere un'attività di avvio che esegua il file *SignalrPerfCounterInstall. cmd* all'avvio del servizio:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Aprire `Views/Shared/_Layout.cshtml` e rimuovere lo script del bundle jQuery dalla fine del file.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Aggiungere un client JavaScript che chiama continuamente il metodo `increment` sul server. Aprire `Views/Home/Index.cshtml` e sostituire il contenuto con il codice seguente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Creare una nuova cartella nel progetto **WebRole1** denominata *Hub*. Fare clic con il pulsante destro del mouse sulla cartella *Hub* in **Esplora soluzioni** e scegliere **Aggiungi** > **nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria **Web** > **SignalR** e quindi selezionare il modello di elemento **classe Hub SignalR (v2)** . Denominare il nuovo hub *MyHub.cs* e selezionare **Aggiungi**.

    ![Aggiunta della classe Hub SignalR alla cartella Hub nella finestra di dialogo Aggiungi nuovo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* verrà aperto automaticamente nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Crank. exe](signalr-connection-density-testing-with-crank.md)* è uno strumento di test della densità della connessione fornito con la codebase di SignalR. Poiché Crank richiede una connessione permanente, aggiungerne una al sito da usare durante il test. Aggiungere una nuova cartella al progetto **WebRole1** denominato *PersistentConnections*. Fare clic con il pulsante destro del mouse su questa cartella e scegliere **aggiungi** > **classe**. Denominare il nuovo file di classe *MyPersistentConnections.cs* e selezionare **Aggiungi**.

24. Visual Studio aprirà il file *MyPersistentConnections.cs* nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Utilizzando la classe `Startup`, gli oggetti SignalR vengono avviati all'avvio di OWIN. Aprire o creare *Startup.cs* e sostituire il contenuto con il codice seguente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Nel codice precedente l'attributo `OwinStartup` Contrassegna questa classe per avviare OWIN. Il metodo `Configuration` avvia SignalR.

26. Testare l'applicazione nella emulatore di Microsoft Azure premendo **F5**.

    > [!NOTE]
    > Se si verifica un **FileLoadException** in **MapSignalR**, modificare i reindirizzamenti di binding in *Web. config* nel modo seguente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Attendere circa un minuto. Aprire la finestra degli strumenti Cloud Explorer in Visual Studio (**visualizza** > **Cloud Explorer**) ed espandere il percorso `(Local)/Storage Accounts/(Development)/Tables`. Fare doppio clic su **WADPerformanceCountersTable**. Verranno visualizzati i contatori SignalR nei dati della tabella. Se la tabella non viene visualizzata, potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure. Potrebbe essere necessario selezionare il pulsante **Aggiorna** per visualizzare la tabella in **Cloud Explorer** o selezionare il pulsante **Aggiorna** nella finestra Apri tabella per visualizzare i dati nella tabella.

    ![Selezione della tabella dei contatori delle prestazioni di WAD in Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Visualizzazione dei contatori raccolti nella tabella dei contatori delle prestazioni di WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Per testare l'applicazione nel cloud, aggiornare il file **ServiceConfiguration. cloud. cscfg** e impostare il `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` su una stringa di connessione dell'account di archiviazione di Azure valida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Distribuire l'applicazione nella sottoscrizione di Azure. Per informazioni dettagliate su come distribuire un'applicazione in Azure, vedere [come creare e distribuire un servizio cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Attendere qualche minuto. In **Cloud Explorer**individuare l'account di archiviazione configurato in precedenza e trovare la tabella `WADPerformanceCountersTable`. Verranno visualizzati i contatori SignalR nei dati della tabella. Se la tabella non viene visualizzata, potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure. Potrebbe essere necessario selezionare il pulsante **Aggiorna** per visualizzare la tabella in **Cloud Explorer** o selezionare il pulsante **Aggiorna** nella finestra Apri tabella per visualizzare i dati nella tabella.

Grazie speciale a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) per i contenuti originali usati in questa esercitazione.
