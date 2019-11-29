---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: chat in tempo reale con SignalR 2 e MVC 5 | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. È possibile aggiungere SignalR a un'applicazione MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600478"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="9f9e7-104">Esercitazione: chat in tempo reale con SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="9f9e7-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="9f9e7-105">Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="9f9e7-106">Si aggiunge SignalR a un'applicazione MVC 5 e si crea una visualizzazione chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="9f9e7-107">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f9e7-108">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="9f9e7-108">Set up the project</span></span>
> * <span data-ttu-id="9f9e7-109">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="9f9e7-109">Run the sample</span></span>
> * <span data-ttu-id="9f9e7-110">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="9f9e7-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="9f9e7-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="9f9e7-111">Prerequisites</span></span>

* <span data-ttu-id="9f9e7-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro di **sviluppo ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="9f9e7-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9f9e7-113">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="9f9e7-113">Set up the Project</span></span>

<span data-ttu-id="9f9e7-114">Questa sezione illustra come usare Visual Studio 2017 e SignalR 2 per creare un'applicazione ASP.NET MVC 5 vuota, aggiungere la libreria SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="9f9e7-115">In Visual Studio creare un' C# applicazione ASP.NET destinata a .NET Framework 4,5, denominarla SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Crea Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="9f9e7-117">In **New ASP.NET Web Application-SignalRMvcChat**selezionare **MVC** , quindi selezionare **Change Authentication**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="9f9e7-118">In **Cambia autenticazione**selezionare **Nessuna autenticazione** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Selezionare Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="9f9e7-120">In **New ASP.NET Web Application-SignalRMvcChat**fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="9f9e7-121">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="9f9e7-122">In **Aggiungi nuovo elemento-SignalRChat**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .</span><span class="sxs-lookup"><span data-stu-id="9f9e7-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="9f9e7-123">Assegnare alla classe il nome *ChatHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="9f9e7-124">Questo passaggio crea il file di classe *ChatHub.cs* e aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="9f9e7-125">Sostituire il codice nel nuovo file di classe *ChatHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="9f9e7-126">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="9f9e7-127">Assegnare un nome all' *avvio* della nuova classe e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="9f9e7-128">Sostituire il codice nel file di classe *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="9f9e7-129">In **Esplora soluzioni**selezionare **controller** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="9f9e7-130">Aggiungere questo metodo a *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="9f9e7-131">Questo metodo restituisce la visualizzazione **chat** creata in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="9f9e7-132">In **Esplora soluzioni**fare clic con il pulsante destro del mouse su **views** > **Home**e scegliere **Aggiungi** >  **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="9f9e7-133">In **Aggiungi visualizzazione**assegnare un nome alla **chat** nuova visualizzazione e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="9f9e7-134">Sostituire il contenuto di **chat. cshtml** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="9f9e7-135">In **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="9f9e7-136">Le librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9f9e7-137">Gestione pacchetti potrebbe avere installato una versione successiva degli script SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="9f9e7-138">Verificare che i riferimenti allo script nel blocco di codice corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="9f9e7-139">Riferimenti a script dal blocco di codice originale:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="9f9e7-140">Se non corrispondono, aggiornare il file con *estensione cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9f9e7-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="9f9e7-141">Dalla barra dei menu selezionare **File** > **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="9f9e7-142">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="9f9e7-142">Run the Sample</span></span>

1. <span data-ttu-id="9f9e7-143">Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'esempio in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="9f9e7-145">Quando si apre il browser, immettere un nome per l'identità della chat.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="9f9e7-146">Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="9f9e7-147">In ogni browser immettere un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="9f9e7-148">A questo punto, aggiungere un commento e selezionare **Send (Invia**).</span><span class="sxs-lookup"><span data-stu-id="9f9e7-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="9f9e7-149">Ripetere questa ripetizione negli altri browser.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="9f9e7-150">I commenti vengono visualizzati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9f9e7-151">Questa semplice applicazione di chat non mantiene il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="9f9e7-152">L'hub trasmette i commenti a tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="9f9e7-153">Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="9f9e7-154">Scopri in che modo l'applicazione di chat viene eseguita in tre browser diversi.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="9f9e7-155">Quando Tom, Anand e Susan inviano messaggi, tutti i browser vengono aggiornati in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tutti e tre i browser visualizzano la stessa cronologia delle chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="9f9e7-157">In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="9f9e7-158">È presente un file script denominato *Hub* generato dalla libreria SignalR in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="9f9e7-159">Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script Hub generato automaticamente nel nodo documenti script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="9f9e7-161">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="9f9e7-161">Examine the Code</span></span>

<span data-ttu-id="9f9e7-162">L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="9f9e7-163">Viene illustrato come creare un hub.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-163">It shows you how to create a hub.</span></span> <span data-ttu-id="9f9e7-164">Il server utilizza tale hub come oggetto di coordinamento principale.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="9f9e7-165">L'hub usa la libreria di segnalazione jQuery per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="9f9e7-166">Hub SignalR in ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="9f9e7-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="9f9e7-167">Nell'esempio di codice, la classe `ChatHub` deriva dalla classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="9f9e7-168">La derivazione dalla classe `Hub` è un modo utile per compilare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="9f9e7-169">È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli dagli script in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="9f9e7-170">Nel codice della chat, i client chiamano il metodo `ChatHub.Send` per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="9f9e7-171">L'hub a sua volta invia il messaggio a tutti i client chiamando `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="9f9e7-172">Il metodo `Send` illustra diversi concetti dell'hub:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="9f9e7-173">Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="9f9e7-174">Usare la proprietà dinamica `Microsoft.AspNet.SignalR.Hub.Clients` per comunicare con tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="9f9e7-175">Chiamare una funzione sul client (ad esempio la funzione `addNewMessageToPage`) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="9f9e7-176">SignalR e jQuery chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="9f9e7-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="9f9e7-177">Il file di visualizzazione *chat. cshtml* nell'esempio di codice illustra come usare la libreria jQuery di SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="9f9e7-178">Il codice esegue molte attività importanti.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-178">The code carries out many important tasks.</span></span> <span data-ttu-id="9f9e7-179">Viene creato un riferimento al proxy generato automaticamente per l'hub, viene dichiarata una funzione che il server può chiamare per eseguire il push del contenuto ai client e viene avviata una connessione per l'invio di messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="9f9e7-180">In JavaScript, il riferimento alla classe server e ai relativi membri si trova in camelCase.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="9f9e7-181">L'esempio di codice fa C# riferimento alla classe `ChatHub` in JavaScript come `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="9f9e7-182">In questo blocco di codice si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="9f9e7-183">La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="9f9e7-184">La chiamata facoltativa alla funzione `htmlEncode` illustra come codificare il contenuto del messaggio in formato HTML prima di visualizzarlo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="9f9e7-185">È un modo per impedire l'inserimento di script.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="9f9e7-186">Questo codice apre una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="9f9e7-187">Questo approccio assicura che venga stabilita una connessione prima dell'esecuzione del gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="9f9e7-188">Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click nel pulsante **Invia** nella pagina della chat.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9f9e7-189">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="9f9e7-189">Get the code</span></span>

[<span data-ttu-id="9f9e7-190">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="9f9e7-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="9f9e7-191">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9f9e7-191">Additional resources</span></span>

<span data-ttu-id="9f9e7-192">Per ulteriori informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="9f9e7-193">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="9f9e7-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="9f9e7-194">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="9f9e7-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="9f9e7-195">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="9f9e7-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="9f9e7-196">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9f9e7-196">Next steps</span></span>

<span data-ttu-id="9f9e7-197">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f9e7-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f9e7-198">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="9f9e7-198">Set up the project</span></span>
> * <span data-ttu-id="9f9e7-199">Esecuzione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="9f9e7-199">Ran the sample</span></span>
> * <span data-ttu-id="9f9e7-200">Esame del codice</span><span class="sxs-lookup"><span data-stu-id="9f9e7-200">Examined the code</span></span>

<span data-ttu-id="9f9e7-201">Passare all'articolo successivo per informazioni su come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.</span><span class="sxs-lookup"><span data-stu-id="9f9e7-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9f9e7-202">App Web con messaggistica ad alta frequenza</span><span class="sxs-lookup"><span data-stu-id="9f9e7-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)