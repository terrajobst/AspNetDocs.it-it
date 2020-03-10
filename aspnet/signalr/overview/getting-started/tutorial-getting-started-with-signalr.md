---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: chat in tempo reale con SignalR 2 | Microsoft Docs'
author: bradygaster
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. È possibile aggiungere SignalR a un'applicazione Web ASP.NET vuota.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536981"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="1fb3d-104">Esercitazione: chat in tempo reale con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="1fb3d-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="1fb3d-105">Questa esercitazione illustra come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="1fb3d-106">Si aggiunge SignalR a un'applicazione Web ASP.NET vuota e si crea una pagina HTML per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="1fb3d-107">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1fb3d-108">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="1fb3d-108">Set up the project</span></span>
> * <span data-ttu-id="1fb3d-109">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="1fb3d-109">Run the sample</span></span>
> * <span data-ttu-id="1fb3d-110">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="1fb3d-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="1fb3d-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1fb3d-111">Prerequisites</span></span>

* <span data-ttu-id="1fb3d-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1fb3d-113">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="1fb3d-113">Set up the Project</span></span>

<span data-ttu-id="1fb3d-114">Questa sezione illustra come usare Visual Studio 2017 e SignalR 2 per creare un'applicazione Web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="1fb3d-115">In Visual Studio creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crea Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="1fb3d-117">Nella finestra **nuovo progetto ASP.NET-SignalRChat** lasciare **vuoto** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="1fb3d-118">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="1fb3d-119">In **Aggiungi nuovo elemento-SignalRChat**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .</span><span class="sxs-lookup"><span data-stu-id="1fb3d-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="1fb3d-120">Assegnare alla classe il nome *ChatHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="1fb3d-121">Questo passaggio crea il file di classe *ChatHub.cs* e aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="1fb3d-122">Sostituire il codice nel nuovo file di classe *ChatHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="1fb3d-123">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="1fb3d-124">In **Aggiungi nuovo elemento-SignalRChat** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="1fb3d-125">Denominare l' *avvio* della classe e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="1fb3d-126">Sostituire il codice predefinito nella classe *Startup* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="1fb3d-127">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="1fb3d-128">Denominare il nuovo *Indice* della pagina e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="1fb3d-129">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina HTML creata e selezionare **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="1fb3d-130">Sostituire il codice predefinito nella pagina HTML con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="1fb3d-131">In **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="1fb3d-132">Le librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1fb3d-133">Gestione pacchetti potrebbe avere installato una versione successiva degli script SignalR.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="1fb3d-134">Verificare che i riferimenti allo script nel blocco di codice corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="1fb3d-135">Riferimenti a script dal blocco di codice originale:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="1fb3d-136">Se non corrispondono, aggiornare il file *HTML* .</span><span class="sxs-lookup"><span data-stu-id="1fb3d-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="1fb3d-137">Dalla barra dei menu selezionare **File** > **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="1fb3d-138">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="1fb3d-138">Run the Sample</span></span>

1. <span data-ttu-id="1fb3d-139">Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'esempio in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="1fb3d-141">Quando si apre il browser, immettere un nome per l'identità della chat.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="1fb3d-142">Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="1fb3d-143">In ogni browser immettere un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="1fb3d-144">A questo punto, aggiungere un commento e selezionare **Send (Invia**).</span><span class="sxs-lookup"><span data-stu-id="1fb3d-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="1fb3d-145">Ripetere questa ripetizione negli altri browser.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="1fb3d-146">I commenti vengono visualizzati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fb3d-147">Questa semplice applicazione di chat non mantiene il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="1fb3d-148">L'hub trasmette i commenti a tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="1fb3d-149">Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="1fb3d-150">Scopri in che modo l'applicazione di chat viene eseguita in tre browser diversi.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="1fb3d-151">Quando Tom, Anand e Susan inviano messaggi, tutti i browser vengono aggiornati in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tutti e tre i browser visualizzano la stessa cronologia delle chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="1fb3d-153">In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="1fb3d-154">È presente un file script denominato *Hub* generato dalla libreria SignalR in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="1fb3d-155">Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script Hub generato automaticamente nel nodo documenti script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="1fb3d-157">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="1fb3d-157">Examine the Code</span></span>

<span data-ttu-id="1fb3d-158">L'applicazione SignalRChat illustra due attività di sviluppo SignalR di base.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="1fb3d-159">Viene illustrato come creare un hub.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-159">It shows you how to create a hub.</span></span> <span data-ttu-id="1fb3d-160">Il server utilizza tale hub come oggetto di coordinamento principale.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="1fb3d-161">L'hub usa la libreria di segnalazione jQuery per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="1fb3d-162">Hub SignalR in ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="1fb3d-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="1fb3d-163">Nell'esempio di codice precedente, la classe `ChatHub` deriva dalla classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="1fb3d-164">La derivazione dalla classe `Hub` è un modo utile per compilare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="1fb3d-165">È possibile creare metodi pubblici sulla classe Hub e quindi usare tali metodi chiamandoli dagli script in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="1fb3d-166">Nel codice della chat, i client chiamano il metodo `ChatHub.Send` per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="1fb3d-167">L'Hub invia quindi il messaggio a tutti i client chiamando `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="1fb3d-168">Il metodo `Send` illustra diversi concetti dell'hub:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="1fb3d-169">Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="1fb3d-170">Usare la proprietà dinamica `Microsoft.AspNet.SignalR.Hub.Clients` per comunicare con tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="1fb3d-171">Chiamare una funzione sul client (ad esempio la funzione `broadcastMessage`) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="1fb3d-172">SignalR e jQuery in index. html</span><span class="sxs-lookup"><span data-stu-id="1fb3d-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="1fb3d-173">La pagina *index. html* nell'esempio di codice illustra come usare la libreria di SignalR jQuery per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="1fb3d-174">Il codice esegue molte attività importanti.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-174">The code carries out many important tasks.</span></span> <span data-ttu-id="1fb3d-175">Dichiara un proxy per fare riferimento all'hub, dichiara una funzione che il server può chiamare per eseguire il push del contenuto ai client e avvia una connessione per l'invio di messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="1fb3d-176">In JavaScript il riferimento alla classe server e i relativi membri devono essere camelCase.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="1fb3d-177">L'esempio di codice fa C# riferimento alla classe *ChatHub* in JavaScript come `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="1fb3d-178">In questo blocco di codice si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="1fb3d-179">La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="1fb3d-180">Le due righe che codificano in HTML il contenuto prima di visualizzarlo sono facoltative e mostrano un metodo efficace per impedire l'inserimento di script.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="1fb3d-181">Questo codice apre una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="1fb3d-182">Questo approccio assicura che il codice stabilisca una connessione prima dell'esecuzione del gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="1fb3d-183">Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click sul pulsante **Send** nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="1fb3d-184">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="1fb3d-184">Get the code</span></span>

[<span data-ttu-id="1fb3d-185">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="1fb3d-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="1fb3d-186">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1fb3d-186">Additional resources</span></span>

<span data-ttu-id="1fb3d-187">Per ulteriori informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="1fb3d-188">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="1fb3d-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="1fb3d-189">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="1fb3d-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="1fb3d-190">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="1fb3d-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="1fb3d-191">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1fb3d-191">Next steps</span></span>

<span data-ttu-id="1fb3d-192">In questa esercitazione vengono illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fb3d-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1fb3d-193">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="1fb3d-193">Set up the project</span></span>
> * <span data-ttu-id="1fb3d-194">Esecuzione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="1fb3d-194">Ran the sample</span></span>
> * <span data-ttu-id="1fb3d-195">Esame del codice</span><span class="sxs-lookup"><span data-stu-id="1fb3d-195">Examined the code</span></span>

<span data-ttu-id="1fb3d-196">Passare all'articolo successivo per informazioni su come usare SignalR e MVC 5.</span><span class="sxs-lookup"><span data-stu-id="1fb3d-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1fb3d-197">SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="1fb3d-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)