---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Esercitazione: Introduzione con SignalR 1. x e MVC 4 | Microsoft Docs'
author: bradygaster
description: Usare ASP.NET SignalR e ASP.NET MVC 4 per creare un'applicazione di chat in tempo reale.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579569"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="d63ea-103">Esercitazione: Introduzione con SignalR 1. x e MVC 4</span><span class="sxs-lookup"><span data-stu-id="d63ea-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="d63ea-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="d63ea-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d63ea-105">Questa esercitazione illustra come usare ASP.NET SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="d63ea-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="d63ea-106">Si aggiungerà SignalR a un'applicazione MVC 4 e si creerà una visualizzazione chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="d63ea-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="d63ea-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d63ea-107">Overview</span></span>

<span data-ttu-id="d63ea-108">Questa esercitazione illustra lo sviluppo di applicazioni Web in tempo reale con ASP.NET SignalR e ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d63ea-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="d63ea-109">L'esercitazione usa lo stesso codice dell'applicazione di chat di [signalr Introduzione esercitazione](tutorial-getting-started-with-signalr.md), ma Mostra come aggiungerlo a un'applicazione MVC 4 basata sul modello Internet.</span><span class="sxs-lookup"><span data-stu-id="d63ea-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="d63ea-110">In questo argomento si apprenderanno le seguenti attività di sviluppo SignalR:</span><span class="sxs-lookup"><span data-stu-id="d63ea-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="d63ea-111">Aggiunta della libreria SignalR a un'applicazione MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d63ea-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="d63ea-112">Creazione di una classe Hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="d63ea-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="d63ea-113">Uso della libreria jQuery di SignalR in una pagina Web per l'invio di messaggi e la visualizzazione degli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="d63ea-114">Lo screenshot seguente mostra l'applicazione di chat completata in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="d63ea-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Istanze chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="d63ea-116">Sezioni</span><span class="sxs-lookup"><span data-stu-id="d63ea-116">Sections:</span></span>

- [<span data-ttu-id="d63ea-117">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="d63ea-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="d63ea-118">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="d63ea-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="d63ea-119">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="d63ea-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="d63ea-120">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d63ea-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="d63ea-121">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="d63ea-121">Set up the Project</span></span>

<span data-ttu-id="d63ea-122">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="d63ea-122">Prerequisites:</span></span>

- <span data-ttu-id="d63ea-123">Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="d63ea-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="d63ea-124">Se non si dispone di Visual Studio, vedere [download di ASP.NET](https://www.asp.net/downloads) per ottenere lo strumento di sviluppo gratuito visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="d63ea-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="d63ea-125">Per Visual Studio 2010, installare [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="d63ea-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="d63ea-126">Questa sezione illustra come creare un'applicazione ASP.NET MVC 4, aggiungere la libreria SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="d63ea-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="d63ea-127">In Visual Studio creare un'applicazione ASP.NET MVC 4, denominarla SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="d63ea-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="d63ea-128">In VS 2010 selezionare **.NET Framework 4** nel controllo elenco a discesa versione Framework.</span><span class="sxs-lookup"><span data-stu-id="d63ea-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="d63ea-129">Il codice SignalR viene eseguito in .NET Framework versioni 4 e 4,5.</span><span class="sxs-lookup"><span data-stu-id="d63ea-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Crea Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="d63ea-131">Selezionare il modello applicazione Internet, deselezionare l'opzione per **creare un progetto unit test**e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="d63ea-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Crea sito Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="d63ea-133">Aprire **strumenti > gestione pacchetti NuGet > console di gestione pacchetti** ed eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="d63ea-134">Questo passaggio consente di aggiungere al progetto un set di file script e riferimenti ad assembly che abilitano la funzionalità SignalR.</span><span class="sxs-lookup"><span data-stu-id="d63ea-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="d63ea-135">In **Esplora soluzioni** espandere la cartella script.</span><span class="sxs-lookup"><span data-stu-id="d63ea-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="d63ea-136">Si noti che le librerie di script per SignalR sono state aggiunte al progetto.</span><span class="sxs-lookup"><span data-stu-id="d63ea-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Riferimenti alla libreria](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="d63ea-138">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuova cartella**e aggiungere una nuova cartella denominata **Hub**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="d63ea-139">Fare clic con il pulsante destro del mouse sulla cartella **Hub** e scegliere **Aggiungi |** E creare una nuova C# classe denominata **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="d63ea-140">Questa classe verrà usata come hub server SignalR che invia messaggi a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="d63ea-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="d63ea-141">Se si usa Visual Studio 2012 e si è installato l' [aggiornamento di ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), è possibile usare il nuovo modello di elemento SignalR per creare la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="d63ea-142">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Hub** e scegliere **Aggiungi | Nuovo elemento**, selezionare la **classe dell'hub SignalR (V1)** e denominare la classe **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="d63ea-143">Sostituire il codice nella classe **ChatHub** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="d63ea-144">Aprire il file **Global. asax** per il progetto e aggiungere una chiamata al metodo `RouteTable.Routes.MapHubs();` come prima riga di codice nel metodo `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="d63ea-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="d63ea-145">Questo codice registra la route predefinita per gli hub SignalR e deve essere chiamato prima di registrare eventuali altre route.</span><span class="sxs-lookup"><span data-stu-id="d63ea-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="d63ea-146">Il metodo `Application_Start` completato è simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="d63ea-147">Modificare la classe `HomeController` trovata in **Controllers/HomeController. cs** e aggiungere il metodo seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="d63ea-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="d63ea-148">Questo metodo restituisce la visualizzazione **chat** che verrà creata in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="d63ea-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="d63ea-149">Fare clic con il pulsante destro del mouse all'interno del metodo `Chat` appena creato e fare clic su **Aggiungi visualizzazione** per creare un nuovo file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d63ea-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="d63ea-150">Nella finestra di dialogo **Aggiungi visualizzazione** verificare che la casella di controllo sia selezionata per l' **utilizzo di un layout o di una pagina master** (deselezionare le altre caselle di controllo), quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="d63ea-152">Modificare il nuovo file di visualizzazione denominato **chat. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="d63ea-153">Dopo il tag &lt;H2&gt;, incollare la sezione &lt;div&gt; seguente e il blocco di codice `@section scripts` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="d63ea-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="d63ea-154">Questo script consente alla pagina di inviare messaggi di chat e visualizzare messaggi dal server.</span><span class="sxs-lookup"><span data-stu-id="d63ea-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="d63ea-155">Il codice completo per la visualizzazione chat viene visualizzato nel blocco di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d63ea-156">Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, gestione pacchetti potrebbe installare versioni degli script più recenti rispetto alle versioni illustrate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d63ea-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="d63ea-157">Assicurarsi che i riferimenti agli script nel codice corrispondano alle versioni delle librerie di script installate nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d63ea-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="d63ea-158">**Salvare tutto** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d63ea-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="d63ea-159">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="d63ea-159">Run the Sample</span></span>

1. <span data-ttu-id="d63ea-160">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="d63ea-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="d63ea-161">Nella riga dell'indirizzo del browser aggiungere **/Home/chat** all'URL della pagina predefinita per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d63ea-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="d63ea-162">La pagina di chat viene caricata in un'istanza del browser e viene richiesto un nome utente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="d63ea-164">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="d63ea-164">Enter a user name.</span></span>
4. <span data-ttu-id="d63ea-165">Copiare l'URL dalla riga di indirizzi del browser e usarlo per aprire altre due istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="d63ea-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="d63ea-166">In ogni istanza del browser immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="d63ea-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="d63ea-167">In ogni istanza del browser aggiungere un commento e fare clic su **Invia**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="d63ea-168">I commenti devono essere visualizzati in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="d63ea-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d63ea-169">Questa semplice applicazione di chat non mantiene il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="d63ea-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="d63ea-170">L'hub trasmette i commenti a tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="d63ea-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="d63ea-171">Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="d63ea-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="d63ea-172">Lo screenshot seguente illustra l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="d63ea-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="d63ea-174">In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d63ea-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="d63ea-175">Questo nodo è visibile in modalità di debug se si utilizza Internet Explorer come browser.</span><span class="sxs-lookup"><span data-stu-id="d63ea-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="d63ea-176">È disponibile un file script denominato **Hub** che la libreria SignalR genera dinamicamente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d63ea-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="d63ea-177">Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.</span><span class="sxs-lookup"><span data-stu-id="d63ea-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="d63ea-178">Se si usa un browser diverso da Internet Explorer, è anche possibile accedere al file di **Hub** dinamici passando direttamente ad esso, ad esempio http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="d63ea-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script Hub generato](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="d63ea-180">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="d63ea-180">Examine the Code</span></span>

<span data-ttu-id="d63ea-181">L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base: la creazione di un hub come oggetto di coordinamento principale nel server e l'uso della libreria di segnalazione jQuery per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="d63ea-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="d63ea-182">Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="d63ea-182">SignalR Hubs</span></span>

<span data-ttu-id="d63ea-183">Nell'esempio di codice la classe **ChatHub** deriva dalla classe **Microsoft. AspNet. SignalR. Hub** .</span><span class="sxs-lookup"><span data-stu-id="d63ea-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="d63ea-184">La derivazione dalla classe **Hub** è un modo utile per creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="d63ea-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="d63ea-185">È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli da script jQuery in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="d63ea-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="d63ea-186">Nel codice della chat, i client chiamano il metodo **ChatHub. Send** per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="d63ea-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="d63ea-187">L'hub a sua volta invia il messaggio a tutti i client chiamando **clients. all. addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="d63ea-188">Il metodo **Send** illustra diversi concetti sull'hub:</span><span class="sxs-lookup"><span data-stu-id="d63ea-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="d63ea-189">Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.</span><span class="sxs-lookup"><span data-stu-id="d63ea-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="d63ea-190">Usare la proprietà **Microsoft. AspNet. SignalR. Hub. clients** per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="d63ea-191">Chiamare una funzione jQuery sul client (ad esempio la funzione `addNewMessageToPage`) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="d63ea-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="d63ea-192">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="d63ea-192">SignalR and jQuery</span></span>

<span data-ttu-id="d63ea-193">Il file di visualizzazione **chat. cshtml** nell'esempio di codice illustra come usare la libreria jQuery di SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="d63ea-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="d63ea-194">Le attività essenziali del codice creano un riferimento al proxy generato automaticamente per l'hub, dichiarando una funzione che il server può chiamare per eseguire il push del contenuto ai client e avviando una connessione per l'invio di messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="d63ea-195">Il codice seguente dichiara un proxy per un hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="d63ea-196">In jQuery il riferimento alla classe server e ai relativi membri è in caso di cammello.</span><span class="sxs-lookup"><span data-stu-id="d63ea-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="d63ea-197">L'esempio di codice fa C# riferimento alla classe **ChatHub** in jQuery come **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="d63ea-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="d63ea-198">Se si vuole fare riferimento alla classe `ChatHub` in jQuery con l'intelaiatura Pascal convenzionale come si C#farebbe in, modificare il file della classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="d63ea-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="d63ea-199">Aggiungere un'istruzione `using` per fare riferimento allo spazio dei nomi `Microsoft.AspNet.SignalR.Hubs`.</span><span class="sxs-lookup"><span data-stu-id="d63ea-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="d63ea-200">Aggiungere quindi l'attributo `HubName` alla classe `ChatHub`, ad esempio `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="d63ea-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="d63ea-201">Aggiornare infine il riferimento jQuery alla classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="d63ea-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="d63ea-202">Nel codice seguente viene illustrato come creare una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="d63ea-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="d63ea-203">La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="d63ea-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="d63ea-204">La chiamata facoltativa alla funzione `htmlEncode` illustra come codificare il contenuto del messaggio in formato HTML prima di visualizzarlo nella pagina, in modo da impedire l'inserimento di script.</span><span class="sxs-lookup"><span data-stu-id="d63ea-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="d63ea-205">Il codice seguente illustra come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="d63ea-206">Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click nel pulsante **Invia** nella pagina della chat.</span><span class="sxs-lookup"><span data-stu-id="d63ea-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="d63ea-207">Questo approccio assicura che la connessione venga stabilita prima dell'esecuzione del gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="d63ea-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="d63ea-208">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d63ea-208">Next Steps</span></span>

<span data-ttu-id="d63ea-209">Si è appreso che SignalR è un Framework per la creazione di applicazioni Web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="d63ea-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="d63ea-210">Sono state inoltre apprese diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe Hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="d63ea-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="d63ea-211">Per informazioni sui concetti più avanzati relativi agli sviluppi di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:</span><span class="sxs-lookup"><span data-stu-id="d63ea-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="d63ea-212">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="d63ea-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="d63ea-213">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="d63ea-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="d63ea-214">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="d63ea-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
