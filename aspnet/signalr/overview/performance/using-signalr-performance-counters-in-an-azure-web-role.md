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
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="ec29a-104">Uso dei contatori delle prestazioni di SignalR in un ruolo Web di Azure</span><span class="sxs-lookup"><span data-stu-id="ec29a-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="ec29a-105">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ec29a-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="ec29a-106">I contatori delle prestazioni di SignalR vengono usati per monitorare le prestazioni dell'app in un ruolo Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="ec29a-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="ec29a-107">I contatori vengono acquisiti da Diagnostica di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ec29a-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="ec29a-108">È possibile installare i contatori delle prestazioni di SignalR in Azure con *SignalR. exe*, lo stesso strumento usato per le app autonome o locali.</span><span class="sxs-lookup"><span data-stu-id="ec29a-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="ec29a-109">Poiché i ruoli di Azure sono temporanei, è possibile configurare un'app per installare e registrare i contatori delle prestazioni di SignalR all'avvio.</span><span class="sxs-lookup"><span data-stu-id="ec29a-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec29a-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ec29a-110">Prerequisites</span></span>

* <span data-ttu-id="ec29a-111">Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="ec29a-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="ec29a-112">[Microsoft Azure SDK per Visual Studio](https://azure.microsoft.com/downloads/) **Nota: riavviare il computer dopo l'installazione dell'SDK.**</span><span class="sxs-lookup"><span data-stu-id="ec29a-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="ec29a-113">Sottoscrizione di Microsoft Azure: per iscriversi per ottenere un account di prova di Azure gratuito, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="ec29a-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="ec29a-114">Creazione di un'applicazione del ruolo Web di Azure che espone i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="ec29a-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="ec29a-115">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec29a-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="ec29a-116">In Visual Studio selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="ec29a-117">Nella finestra di dialogo **nuovo progetto** selezionare la categoria **Visual C#**  > **cloud** a sinistra e quindi selezionare il modello **servizio cloud di Azure** .</span><span class="sxs-lookup"><span data-stu-id="ec29a-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="ec29a-118">Assegnare all'app il nome **SignalRPerfCounters** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nuova applicazione cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="ec29a-120">Se non viene visualizzata la categoria del modello **cloud** o il modello di **servizio cloud di Azure** , è necessario installare il carico di lavoro di **sviluppo di Azure** per Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ec29a-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="ec29a-121">Scegliere il collegamento **apri programma di installazione di Visual Studio** nel lato inferiore sinistro della finestra di dialogo **nuovo progetto** per aprire programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec29a-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="ec29a-122">Selezionare il carico di lavoro **sviluppo di Azure** , quindi scegliere **modifica** per avviare l'installazione del carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="ec29a-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Carico di lavoro di sviluppo di Azure in Programma di installazione di Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="ec29a-124">Nella finestra di dialogo **nuovo servizio Cloud Microsoft Azure** selezionare **ruolo Web ASP.NET** e selezionare il pulsante > per aggiungere il ruolo al progetto.</span><span class="sxs-lookup"><span data-stu-id="ec29a-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="ec29a-125">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-125">Select **OK**.</span></span>

   ![Aggiungi ruolo Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="ec29a-127">Nella finestra di dialogo **nuova applicazione Web ASP.NET-WebRole1** selezionare il modello **MVC** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Aggiungere MVC e API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="ec29a-129">In **Esplora soluzioni**aprire il file *Diagnostics. wadcfgx* in **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Esplora soluzioni Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="ec29a-131">Sostituire il contenuto del file con la configurazione seguente e salvare il file:</span><span class="sxs-lookup"><span data-stu-id="ec29a-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="ec29a-132">Aprire la **console di gestione pacchetti** da **strumenti** > **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="ec29a-133">Immettere i comandi seguenti per installare la versione più recente di SignalR e il pacchetto delle utilità SignalR:</span><span class="sxs-lookup"><span data-stu-id="ec29a-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="ec29a-134">Configurare l'app per installare i contatori delle prestazioni di SignalR nell'istanza del ruolo all'avvio o al riciclo.</span><span class="sxs-lookup"><span data-stu-id="ec29a-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="ec29a-135">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WebRole1** e scegliere **Aggiungi** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="ec29a-136">Assegnare un nome all' *avvio*della nuova cartella.</span><span class="sxs-lookup"><span data-stu-id="ec29a-136">Name the new folder *Startup*.</span></span>

   ![Aggiungi cartella di avvio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="ec29a-138">Copiare il file *SignalR. exe* (aggiunto con il pacchetto **Microsoft. AspNet. SignalR. Utils** ) da \<cartella del progetto >/SignalRPerfCounters/Packages/Microsoft.AspNet.SignalR.utils.\<versione >/Tools alla cartella di *avvio* creata nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="ec29a-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="ec29a-139">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella di *avvio* e scegliere **Aggiungi** > **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="ec29a-140">Nella finestra di dialogo visualizzata selezionare *SignalR. exe* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Aggiungere SignalR. exe al progetto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="ec29a-142">Fare clic con il pulsante destro del mouse sulla cartella di *avvio* creata.</span><span class="sxs-lookup"><span data-stu-id="ec29a-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="ec29a-143">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="ec29a-144">Selezionare il nodo **generale** , selezionare **file di testo**e denominare il nuovo elemento *SignalRPerfCounterInstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="ec29a-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="ec29a-145">Questo file di comando installerà i contatori delle prestazioni di SignalR nel ruolo Web.</span><span class="sxs-lookup"><span data-stu-id="ec29a-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Crea il file batch di installazione del contatore delle prestazioni SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="ec29a-147">Quando Visual Studio crea il file *SignalRPerfCounterInstall. cmd* , viene aperto automaticamente nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="ec29a-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="ec29a-148">Sostituire il contenuto del file con lo script seguente, quindi salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="ec29a-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="ec29a-149">Questo script esegue *SignalR. exe*, che aggiunge i contatori delle prestazioni di SignalR all'istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="ec29a-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="ec29a-150">Selezionare il file *SignalR. exe* in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="ec29a-151">Nelle **Proprietà**del file impostare copia nella **directory di output** su **copia sempre**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Imposta copia nella directory di output su copia sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="ec29a-153">Ripetere il passaggio precedente per il file *SignalRPerfCounterInstall. cmd* .</span><span class="sxs-lookup"><span data-stu-id="ec29a-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="ec29a-154">Fare clic con il pulsante destro del mouse sul file *SignalRPerfCounterInstall. cmd* e scegliere **Apri con**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="ec29a-155">Nella finestra di dialogo visualizzata selezionare **editor binario** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Apri con editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="ec29a-157">Nell'editor binario selezionare tutti i byte iniziali nel file ed eliminarli.</span><span class="sxs-lookup"><span data-stu-id="ec29a-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="ec29a-158">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="ec29a-158">Save and close the file.</span></span>

    ![Elimina byte iniziali](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="ec29a-160">Aprire *file ServiceDefinition. csdef* e aggiungere un'attività di avvio che esegua il file *SignalrPerfCounterInstall. cmd* all'avvio del servizio:</span><span class="sxs-lookup"><span data-stu-id="ec29a-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="ec29a-161">Aprire `Views/Shared/_Layout.cshtml` e rimuovere lo script del bundle jQuery dalla fine del file.</span><span class="sxs-lookup"><span data-stu-id="ec29a-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="ec29a-162">Aggiungere un client JavaScript che chiama continuamente il metodo `increment` sul server.</span><span class="sxs-lookup"><span data-stu-id="ec29a-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="ec29a-163">Aprire `Views/Home/Index.cshtml` e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ec29a-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="ec29a-164">Creare una nuova cartella nel progetto **WebRole1** denominata *Hub*.</span><span class="sxs-lookup"><span data-stu-id="ec29a-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="ec29a-165">Fare clic con il pulsante destro del mouse sulla cartella *Hub* in **Esplora soluzioni** e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="ec29a-166">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare la categoria **Web** > **SignalR** e quindi selezionare il modello di elemento **classe Hub SignalR (v2)** .</span><span class="sxs-lookup"><span data-stu-id="ec29a-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="ec29a-167">Denominare il nuovo hub *MyHub.cs* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Aggiunta della classe Hub SignalR alla cartella Hub nella finestra di dialogo Aggiungi nuovo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="ec29a-169">*MyHub.cs* verrà aperto automaticamente nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="ec29a-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="ec29a-170">Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="ec29a-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="ec29a-171">*[Crank. exe](signalr-connection-density-testing-with-crank.md)* è uno strumento di test della densità della connessione fornito con la codebase di SignalR.</span><span class="sxs-lookup"><span data-stu-id="ec29a-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="ec29a-172">Poiché Crank richiede una connessione permanente, aggiungerne una al sito da usare durante il test.</span><span class="sxs-lookup"><span data-stu-id="ec29a-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="ec29a-173">Aggiungere una nuova cartella al progetto **WebRole1** denominato *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="ec29a-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="ec29a-174">Fare clic con il pulsante destro del mouse su questa cartella e scegliere **aggiungi** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="ec29a-175">Denominare il nuovo file di classe *MyPersistentConnections.cs* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="ec29a-176">Visual Studio aprirà il file *MyPersistentConnections.cs* nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="ec29a-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="ec29a-177">Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="ec29a-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="ec29a-178">Utilizzando la classe `Startup`, gli oggetti SignalR vengono avviati all'avvio di OWIN.</span><span class="sxs-lookup"><span data-stu-id="ec29a-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="ec29a-179">Aprire o creare *Startup.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ec29a-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="ec29a-180">Nel codice precedente l'attributo `OwinStartup` Contrassegna questa classe per avviare OWIN.</span><span class="sxs-lookup"><span data-stu-id="ec29a-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="ec29a-181">Il metodo `Configuration` avvia SignalR.</span><span class="sxs-lookup"><span data-stu-id="ec29a-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="ec29a-182">Testare l'applicazione nella emulatore di Microsoft Azure premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec29a-183">Se si verifica un **FileLoadException** in **MapSignalR**, modificare i reindirizzamenti di binding in *Web. config* nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="ec29a-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="ec29a-184">Attendere circa un minuto.</span><span class="sxs-lookup"><span data-stu-id="ec29a-184">Wait about one minute.</span></span> <span data-ttu-id="ec29a-185">Aprire la finestra degli strumenti Cloud Explorer in Visual Studio (**visualizza** > **Cloud Explorer**) ed espandere il percorso `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="ec29a-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="ec29a-186">Fare doppio clic su **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="ec29a-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="ec29a-187">Verranno visualizzati i contatori SignalR nei dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="ec29a-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ec29a-188">Se la tabella non viene visualizzata, potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ec29a-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ec29a-189">Potrebbe essere necessario selezionare il pulsante **Aggiorna** per visualizzare la tabella in **Cloud Explorer** o selezionare il pulsante **Aggiorna** nella finestra Apri tabella per visualizzare i dati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="ec29a-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Selezione della tabella dei contatori delle prestazioni di WAD in Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Visualizzazione dei contatori raccolti nella tabella dei contatori delle prestazioni di WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="ec29a-192">Per testare l'applicazione nel cloud, aggiornare il file **ServiceConfiguration. cloud. cscfg** e impostare il `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` su una stringa di connessione dell'account di archiviazione di Azure valida.</span><span class="sxs-lookup"><span data-stu-id="ec29a-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="ec29a-193">Distribuire l'applicazione nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ec29a-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="ec29a-194">Per informazioni dettagliate su come distribuire un'applicazione in Azure, vedere [come creare e distribuire un servizio cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="ec29a-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="ec29a-195">Attendere qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="ec29a-195">Wait a few minutes.</span></span> <span data-ttu-id="ec29a-196">In **Cloud Explorer**individuare l'account di archiviazione configurato in precedenza e trovare la tabella `WADPerformanceCountersTable`.</span><span class="sxs-lookup"><span data-stu-id="ec29a-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="ec29a-197">Verranno visualizzati i contatori SignalR nei dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="ec29a-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ec29a-198">Se la tabella non viene visualizzata, potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ec29a-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ec29a-199">Potrebbe essere necessario selezionare il pulsante **Aggiorna** per visualizzare la tabella in **Cloud Explorer** o selezionare il pulsante **Aggiorna** nella finestra Apri tabella per visualizzare i dati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="ec29a-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="ec29a-200">Grazie speciale a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) per i contenuti originali usati in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ec29a-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
