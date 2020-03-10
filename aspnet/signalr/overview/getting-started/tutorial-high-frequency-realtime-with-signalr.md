---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "Esercitazione: creare un'app in tempo reale ad alta frequenza con SignalR 2 | Microsoft Docs"
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558625"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="009bf-103">Esercitazione: creare un'app in tempo reale ad alta frequenza con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="009bf-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="009bf-104">Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.</span><span class="sxs-lookup"><span data-stu-id="009bf-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="009bf-105">In questo caso, "messaggistica ad alta frequenza" significa che il server invia aggiornamenti a una frequenza fissa.</span><span class="sxs-lookup"><span data-stu-id="009bf-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="009bf-106">Inviare un massimo di 10 messaggi al secondo.</span><span class="sxs-lookup"><span data-stu-id="009bf-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="009bf-107">L'applicazione creata consente di visualizzare una forma che gli utenti possono trascinare.</span><span class="sxs-lookup"><span data-stu-id="009bf-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="009bf-108">Il server aggiorna la posizione della forma in tutti i browser connessi in modo che corrisponda alla posizione della forma trascinata usando gli aggiornamenti temporizzati.</span><span class="sxs-lookup"><span data-stu-id="009bf-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="009bf-109">I concetti introdotti in questa esercitazione includono applicazioni in giochi in tempo reale e altre applicazioni di simulazione.</span><span class="sxs-lookup"><span data-stu-id="009bf-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="009bf-110">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="009bf-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="009bf-111">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="009bf-111">Set up the project</span></span>
> * <span data-ttu-id="009bf-112">Creare l'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="009bf-112">Create the base application</span></span>
> * <span data-ttu-id="009bf-113">Eseguire il mapping all'hub all'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="009bf-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="009bf-114">Aggiungere il client</span><span class="sxs-lookup"><span data-stu-id="009bf-114">Add the client</span></span>
> * <span data-ttu-id="009bf-115">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="009bf-115">Run the app</span></span>
> * <span data-ttu-id="009bf-116">Aggiungere il ciclo client</span><span class="sxs-lookup"><span data-stu-id="009bf-116">Add the client loop</span></span>
> * <span data-ttu-id="009bf-117">Aggiungere il ciclo server</span><span class="sxs-lookup"><span data-stu-id="009bf-117">Add the server loop</span></span>
> * <span data-ttu-id="009bf-118">Aggiungi animazione liscia</span><span class="sxs-lookup"><span data-stu-id="009bf-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="009bf-119">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="009bf-119">Prerequisites</span></span>

* <span data-ttu-id="009bf-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="009bf-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="009bf-121">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="009bf-121">Set up the project</span></span>

<span data-ttu-id="009bf-122">In questa sezione viene creato il progetto in Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="009bf-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="009bf-123">Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota e aggiungere le librerie SignalR e jQuery. UI.</span><span class="sxs-lookup"><span data-stu-id="009bf-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="009bf-124">In Visual Studio creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="009bf-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crea Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="009bf-126">Nella finestra **nuova applicazione Web ASP.NET-MoveShapeDemo** lasciare **vuoto** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="009bf-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="009bf-127">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="009bf-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="009bf-128">In **Aggiungi nuovo elemento-MoveShapeDemo**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .</span><span class="sxs-lookup"><span data-stu-id="009bf-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="009bf-129">Assegnare alla classe il nome *MoveShapeHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="009bf-130">Questo passaggio crea il file della classe *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="009bf-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="009bf-131">Simultaneamente, aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="009bf-132">Fare clic su **Strumenti** > **Gestione Pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="009bf-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="009bf-133">Nella **console di gestione pacchetti**eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="009bf-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="009bf-134">Il comando installa la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="009bf-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="009bf-135">Viene usato per animare la forma.</span><span class="sxs-lookup"><span data-stu-id="009bf-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="009bf-136">In **Esplora soluzioni**espandere il nodo script.</span><span class="sxs-lookup"><span data-stu-id="009bf-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Riferimenti alla libreria di script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="009bf-138">Le librerie di script per jQuery, jQueryUI e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="009bf-139">Creare l'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="009bf-139">Create the base application</span></span>

<span data-ttu-id="009bf-140">In questa sezione viene creata un'applicazione browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-140">In this section, you create a browser application.</span></span> <span data-ttu-id="009bf-141">L'app invia il percorso della forma al server durante ogni evento di spostamento del mouse.</span><span class="sxs-lookup"><span data-stu-id="009bf-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="009bf-142">Il server trasmette queste informazioni a tutti gli altri client connessi in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="009bf-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="009bf-143">Per ulteriori informazioni su questa applicazione, vedere le sezioni successive.</span><span class="sxs-lookup"><span data-stu-id="009bf-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="009bf-144">Aprire il file *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="009bf-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="009bf-145">Sostituire il codice nel file *MoveShapeHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="009bf-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="009bf-146">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="009bf-146">Save the file.</span></span>

<span data-ttu-id="009bf-147">La classe `MoveShapeHub` è un'implementazione di un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="009bf-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="009bf-148">Come nell'esercitazione [Introduzione con SignalR](tutorial-getting-started-with-signalr.md) , l'Hub dispone di un metodo chiamato direttamente dai client.</span><span class="sxs-lookup"><span data-stu-id="009bf-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="009bf-149">In questo caso, il client invia un oggetto con le nuove coordinate X e Y della forma al server.</span><span class="sxs-lookup"><span data-stu-id="009bf-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="009bf-150">Tali coordinate vengono trasmesse a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="009bf-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="009bf-151">SignalR serializza automaticamente questo oggetto usando JSON.</span><span class="sxs-lookup"><span data-stu-id="009bf-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="009bf-152">L'app invia l'oggetto `ShapeModel` al client.</span><span class="sxs-lookup"><span data-stu-id="009bf-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="009bf-153">Dispone di membri per archiviare la posizione della forma.</span><span class="sxs-lookup"><span data-stu-id="009bf-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="009bf-154">La versione dell'oggetto nel server dispone anche di un membro per tenere traccia dei dati del client archiviati.</span><span class="sxs-lookup"><span data-stu-id="009bf-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="009bf-155">Questo oggetto impedisce al server di inviare i dati di un client a se stesso.</span><span class="sxs-lookup"><span data-stu-id="009bf-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="009bf-156">Questo membro usa l'attributo `JsonIgnore` per impedire alla serializzazione dei dati da parte dell'applicazione e di inviarli di nuovo al client.</span><span class="sxs-lookup"><span data-stu-id="009bf-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="009bf-157">Eseguire il mapping all'hub all'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="009bf-157">Map to the hub when app starts</span></span>

<span data-ttu-id="009bf-158">Si imposta quindi il mapping all'hub all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="009bf-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="009bf-159">In SignalR 2 l'aggiunta di una classe di avvio OWIN crea il mapping.</span><span class="sxs-lookup"><span data-stu-id="009bf-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="009bf-160">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="009bf-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="009bf-161">In **Aggiungi nuovo elemento-MoveShapeDemo** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="009bf-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="009bf-162">Denominare l' *avvio* della classe e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="009bf-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="009bf-163">Sostituire il codice predefinito nel file *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="009bf-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="009bf-164">La classe di avvio OWIN chiama `MapSignalR` quando l'app esegue il `Configuration` metodo.</span><span class="sxs-lookup"><span data-stu-id="009bf-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="009bf-165">L'app aggiunge la classe al processo di avvio di OWIN usando l'attributo `OwinStartup` assembly.</span><span class="sxs-lookup"><span data-stu-id="009bf-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="009bf-166">Aggiungere il client</span><span class="sxs-lookup"><span data-stu-id="009bf-166">Add the client</span></span>

<span data-ttu-id="009bf-167">Aggiungere la pagina HTML per il client.</span><span class="sxs-lookup"><span data-stu-id="009bf-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="009bf-168">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="009bf-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="009bf-169">Assegnare alla pagina il nome **predefinito** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="009bf-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="009bf-170">In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *default. html* e scegliere **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="009bf-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="009bf-171">Sostituire il codice predefinito nel file *default. html* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="009bf-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="009bf-172">In **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="009bf-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="009bf-173">Le librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="009bf-174">Gestione pacchetti installa una versione successiva degli script SignalR.</span><span class="sxs-lookup"><span data-stu-id="009bf-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="009bf-175">Aggiornare i riferimenti allo script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="009bf-176">Questo codice HTML e JavaScript crea una `div` rossa denominata `shape`.</span><span class="sxs-lookup"><span data-stu-id="009bf-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="009bf-177">Consente il comportamento di trascinamento della forma usando la libreria jQuery e usa l'evento `drag` per inviare la posizione della forma al server.</span><span class="sxs-lookup"><span data-stu-id="009bf-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="009bf-178">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="009bf-178">Run the app</span></span>

<span data-ttu-id="009bf-179">È possibile eseguire l'app per se'e il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="009bf-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="009bf-180">Quando si trascina la forma intorno a una finestra del browser, la forma viene spostata anche negli altri browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="009bf-181">Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'applicazione in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="009bf-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Screenshot dell'attivazione della modalità di debug da utente e selezione di Play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="009bf-183">Viene visualizzata una finestra del browser con la forma rossa nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="009bf-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="009bf-184">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="009bf-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="009bf-185">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="009bf-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="009bf-186">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="009bf-187">Segue la forma nell'altra finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="009bf-188">Sebbene l'applicazione funzioni utilizzando questo metodo, non è un modello di programmazione consigliato.</span><span class="sxs-lookup"><span data-stu-id="009bf-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="009bf-189">Non esiste un limite massimo per il numero di messaggi inviati.</span><span class="sxs-lookup"><span data-stu-id="009bf-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="009bf-190">Di conseguenza, i client e il server si sovraccaricano di messaggi e prestazioni.</span><span class="sxs-lookup"><span data-stu-id="009bf-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="009bf-191">Inoltre, l'app Visualizza un'animazione disgiunto nel client.</span><span class="sxs-lookup"><span data-stu-id="009bf-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="009bf-192">Questa animazione a scatti si verifica perché la forma si sposta immediatamente da ogni metodo.</span><span class="sxs-lookup"><span data-stu-id="009bf-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="009bf-193">È preferibile che la forma si sposti senza problemi in ogni nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="009bf-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="009bf-194">Si apprenderà quindi come risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="009bf-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="009bf-195">Aggiungere il ciclo client</span><span class="sxs-lookup"><span data-stu-id="009bf-195">Add the client loop</span></span>

<span data-ttu-id="009bf-196">Inviando la posizione della forma a ogni evento di spostamento del mouse, viene creata una quantità di traffico di rete non necessaria.</span><span class="sxs-lookup"><span data-stu-id="009bf-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="009bf-197">L'app deve limitare i messaggi dal client.</span><span class="sxs-lookup"><span data-stu-id="009bf-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="009bf-198">Utilizzare la funzione JavaScript `setInterval` per configurare un ciclo che invia al server le informazioni di nuova posizione a una frequenza fissa.</span><span class="sxs-lookup"><span data-stu-id="009bf-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="009bf-199">Questo ciclo è una rappresentazione di base di un "ciclo di gioco".</span><span class="sxs-lookup"><span data-stu-id="009bf-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="009bf-200">Si tratta di una funzione chiamata ripetuta che guida tutte le funzionalità di un gioco.</span><span class="sxs-lookup"><span data-stu-id="009bf-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="009bf-201">Sostituire il codice client nel file *default. html* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="009bf-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="009bf-202">È necessario sostituire di nuovo i riferimenti di script.</span><span class="sxs-lookup"><span data-stu-id="009bf-202">You have to replace the script references again.</span></span> <span data-ttu-id="009bf-203">Devono corrispondere alle versioni degli script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="009bf-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="009bf-204">Questo nuovo codice aggiunge la funzione `updateServerModel`.</span><span class="sxs-lookup"><span data-stu-id="009bf-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="009bf-205">Viene chiamato su una frequenza fissa.</span><span class="sxs-lookup"><span data-stu-id="009bf-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="009bf-206">La funzione Invia i dati di posizione al server ogni volta che il flag di `moved` indica che sono presenti nuovi dati di posizione da inviare.</span><span class="sxs-lookup"><span data-stu-id="009bf-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="009bf-207">Selezionare il pulsante Riproduci per avviare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="009bf-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="009bf-208">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="009bf-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="009bf-209">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="009bf-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="009bf-210">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="009bf-211">Segue la forma nell'altra finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="009bf-212">Poiché l'app limita il numero di messaggi che vengono inviati al server, l'animazione non verrà visualizzata come smussata prima.</span><span class="sxs-lookup"><span data-stu-id="009bf-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="009bf-213">Aggiungere il ciclo server</span><span class="sxs-lookup"><span data-stu-id="009bf-213">Add the server loop</span></span>

<span data-ttu-id="009bf-214">Nell'applicazione corrente, i messaggi inviati dal server al client si escono con la stessa frequenza con cui sono stati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="009bf-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="009bf-215">Questo traffico di rete presenta un problema simile a quello visualizzato nel client.</span><span class="sxs-lookup"><span data-stu-id="009bf-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="009bf-216">L'app può inviare messaggi più spesso del necessario.</span><span class="sxs-lookup"><span data-stu-id="009bf-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="009bf-217">Di conseguenza, è possibile che la connessione venga allagata.</span><span class="sxs-lookup"><span data-stu-id="009bf-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="009bf-218">In questa sezione viene descritto come aggiornare il server per aggiungere un timer per limitare la frequenza dei messaggi in uscita.</span><span class="sxs-lookup"><span data-stu-id="009bf-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="009bf-219">Sostituire il contenuto di `MoveShapeHub.cs` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="009bf-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="009bf-220">Selezionare il pulsante Riproduci per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="009bf-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="009bf-221">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="009bf-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="009bf-222">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="009bf-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="009bf-223">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="009bf-224">Questo codice espande il client per aggiungere la classe `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="009bf-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="009bf-225">La nuova classe limita i messaggi in uscita usando la classe `Timer` di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="009bf-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="009bf-226">È opportuno apprendere che l'hub stesso è transitorio.</span><span class="sxs-lookup"><span data-stu-id="009bf-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="009bf-227">Viene creato ogni volta che è necessario.</span><span class="sxs-lookup"><span data-stu-id="009bf-227">It's created every time it's needed.</span></span> <span data-ttu-id="009bf-228">Quindi, l'app crea il `Broadcaster` come singleton.</span><span class="sxs-lookup"><span data-stu-id="009bf-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="009bf-229">Usa l'inizializzazione Lazy per rinviare la creazione del `Broadcaster`fino a quando non è necessario.</span><span class="sxs-lookup"><span data-stu-id="009bf-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="009bf-230">Ciò garantisce che l'app crei completamente la prima istanza dell'hub prima di avviare il timer.</span><span class="sxs-lookup"><span data-stu-id="009bf-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="009bf-231">La chiamata alla funzione `UpdateShape` dei client viene quindi spostata al di fuori del metodo `UpdateModel` dell'hub.</span><span class="sxs-lookup"><span data-stu-id="009bf-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="009bf-232">Non viene più chiamato immediatamente ogni volta che l'app riceve i messaggi in ingresso.</span><span class="sxs-lookup"><span data-stu-id="009bf-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="009bf-233">Al contrario, l'app invia i messaggi ai client con una frequenza di 25 chiamate al secondo.</span><span class="sxs-lookup"><span data-stu-id="009bf-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="009bf-234">Il processo viene gestito dal timer `_broadcastLoop` dall'interno della classe `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="009bf-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="009bf-235">Infine, invece di chiamare direttamente il metodo client dall'hub, la classe `Broadcaster` deve ottenere un riferimento all'hub `_hubContext` operativo corrente.</span><span class="sxs-lookup"><span data-stu-id="009bf-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="009bf-236">Ottiene il riferimento con l'`GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="009bf-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="009bf-237">Aggiungi animazione liscia</span><span class="sxs-lookup"><span data-stu-id="009bf-237">Add smooth animation</span></span>

<span data-ttu-id="009bf-238">L'applicazione è quasi completa, ma è possibile apportare un ulteriore miglioramento.</span><span class="sxs-lookup"><span data-stu-id="009bf-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="009bf-239">L'app sposta la forma sul client in risposta ai messaggi del server.</span><span class="sxs-lookup"><span data-stu-id="009bf-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="009bf-240">Anziché impostare la posizione della forma sulla nuova posizione specificata dal server, utilizzare la funzione `animate` della libreria dell'interfaccia utente JQuery.</span><span class="sxs-lookup"><span data-stu-id="009bf-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="009bf-241">Può spostare la forma senza problemi tra la posizione corrente e quella nuova.</span><span class="sxs-lookup"><span data-stu-id="009bf-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="009bf-242">Aggiornare il metodo `updateShape` del client nel file *default. html* per avere un aspetto simile al codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="009bf-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="009bf-243">Selezionare il pulsante Riproduci per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="009bf-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="009bf-244">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="009bf-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="009bf-245">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="009bf-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="009bf-246">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="009bf-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="009bf-247">Lo spostamento della forma nell'altra finestra appare meno Jerky.</span><span class="sxs-lookup"><span data-stu-id="009bf-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="009bf-248">L'app interpola il proprio movimento nel tempo anziché essere impostato una volta per ogni messaggio in ingresso.</span><span class="sxs-lookup"><span data-stu-id="009bf-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="009bf-249">Questo codice sposta la forma dalla posizione precedente a quella nuova.</span><span class="sxs-lookup"><span data-stu-id="009bf-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="009bf-250">Il server assegna la posizione della forma nel corso dell'intervallo di animazione.</span><span class="sxs-lookup"><span data-stu-id="009bf-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="009bf-251">In questo caso, si tratta di 100 millisecondi.</span><span class="sxs-lookup"><span data-stu-id="009bf-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="009bf-252">L'app Cancella qualsiasi animazione precedente in esecuzione sulla forma prima che la nuova animazione venga avviata.</span><span class="sxs-lookup"><span data-stu-id="009bf-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="009bf-253">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="009bf-253">Get the code</span></span>

[<span data-ttu-id="009bf-254">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="009bf-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="009bf-255">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="009bf-255">Additional resources</span></span>

<span data-ttu-id="009bf-256">Il paradigma di comunicazione appena acquisita è utile per lo sviluppo di giochi online e altre simulazioni, come [il gioco sparatutto creato con SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="009bf-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="009bf-257">Per ulteriori informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="009bf-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="009bf-258">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="009bf-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="009bf-259">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="009bf-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="009bf-260">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="009bf-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="009bf-261">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="009bf-261">Next steps</span></span>

<span data-ttu-id="009bf-262">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="009bf-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="009bf-263">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="009bf-263">Set up the project</span></span>
> * <span data-ttu-id="009bf-264">Creazione dell'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="009bf-264">Created the base application</span></span>
> * <span data-ttu-id="009bf-265">Mappato all'hub all'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="009bf-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="009bf-266">Aggiunta del client</span><span class="sxs-lookup"><span data-stu-id="009bf-266">Added the client</span></span>
> * <span data-ttu-id="009bf-267">Esecuzione dell'app</span><span class="sxs-lookup"><span data-stu-id="009bf-267">Ran the app</span></span>
> * <span data-ttu-id="009bf-268">Aggiunta del ciclo client</span><span class="sxs-lookup"><span data-stu-id="009bf-268">Added the client loop</span></span>
> * <span data-ttu-id="009bf-269">Aggiunta del ciclo server</span><span class="sxs-lookup"><span data-stu-id="009bf-269">Added the server loop</span></span>
> * <span data-ttu-id="009bf-270">Aggiunta animazione uniforme</span><span class="sxs-lookup"><span data-stu-id="009bf-270">Added smooth animation</span></span>

<span data-ttu-id="009bf-271">Passare all'articolo successivo per informazioni su come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di broadcast del server.</span><span class="sxs-lookup"><span data-stu-id="009bf-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="009bf-272">SignalR 2 e broadcast server</span><span class="sxs-lookup"><span data-stu-id="009bf-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)