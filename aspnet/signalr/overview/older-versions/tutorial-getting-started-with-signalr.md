---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione con SignalR 1. x | Microsoft Docs'
author: bradygaster
description: Usare ASP.NET SignalR per creare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623543"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="de645-103">Esercitazione: Introduzione con SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="de645-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="de645-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="de645-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="de645-105">In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="de645-106">Si aggiungerà SignalR a un'applicazione Web ASP.NET vuota e si creerà una pagina HTML per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="de645-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="de645-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="de645-107">Overview</span></span>

<span data-ttu-id="de645-108">Questa esercitazione introduce lo sviluppo di SignalR mostrando come creare una semplice applicazione di chat basata su browser.</span><span class="sxs-lookup"><span data-stu-id="de645-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="de645-109">La libreria SignalR viene aggiunta a un'applicazione Web ASP.NET vuota, si crea una classe Hub per l'invio di messaggi ai client e si crea una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat.</span><span class="sxs-lookup"><span data-stu-id="de645-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="de645-110">Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 4 usando una visualizzazione MVC, vedere [Introduzione con SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="de645-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="de645-111">Questa esercitazione usa la versione Release (1. x) di SignalR.</span><span class="sxs-lookup"><span data-stu-id="de645-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="de645-112">Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2,0, vedere [aggiornamento di progetti SignalR 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="de645-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="de645-113">SignalR è una libreria .NET open source per la creazione di applicazioni Web che richiedono l'interazione dell'utente in tempo reale o aggiornamenti dei dati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="de645-114">Esempi di applicazioni di social networking, giochi multiutente, collaborazione aziendale e notizie, meteo o aggiornamenti finanziari.</span><span class="sxs-lookup"><span data-stu-id="de645-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="de645-115">Queste sono spesso denominate applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-115">These are often called real-time applications.</span></span>

<span data-ttu-id="de645-116">SignalR semplifica il processo di compilazione di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="de645-117">Include una libreria del server ASP.NET e una libreria client JavaScript per semplificare la gestione delle connessioni client-server e il push degli aggiornamenti dei contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="de645-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="de645-118">È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere la funzionalità in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="de645-119">Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:</span><span class="sxs-lookup"><span data-stu-id="de645-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="de645-120">Aggiunta della libreria SignalR a un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de645-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="de645-121">Creazione di una classe Hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="de645-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="de645-122">Uso della libreria jQuery di SignalR in una pagina Web per l'invio di messaggi e la visualizzazione degli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="de645-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="de645-123">Lo screenshot seguente illustra l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="de645-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="de645-124">Ogni nuovo utente può inserire commenti e visualizzare i commenti aggiunti dopo che l'utente ha partecipato alla chat.</span><span class="sxs-lookup"><span data-stu-id="de645-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Istanze chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="de645-126">Sezioni</span><span class="sxs-lookup"><span data-stu-id="de645-126">Sections:</span></span>

- [<span data-ttu-id="de645-127">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="de645-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="de645-128">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="de645-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="de645-129">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="de645-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="de645-130">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="de645-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="de645-131">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="de645-131">Set up the Project</span></span>

<span data-ttu-id="de645-132">Questa sezione illustra come creare un'applicazione Web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="de645-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="de645-133">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="de645-133">Prerequisites:</span></span>

- <span data-ttu-id="de645-134">Visual Studio 2010 SP1 o 2012.</span><span class="sxs-lookup"><span data-stu-id="de645-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="de645-135">Se non si dispone di Visual Studio, vedere [download di ASP.NET](https://www.asp.net/downloads) per ottenere lo strumento di sviluppo gratuito visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="de645-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="de645-136">[Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="de645-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="de645-137">Per Visual Studio 2012, questo programma di installazione aggiunge nuove funzionalità di ASP.NET, inclusi i modelli SignalR, a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de645-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="de645-138">Per Visual Studio 2010 SP1, un programma di installazione non è disponibile, ma è possibile completare l'esercitazione installando il pacchetto NuGet SignalR come descritto nei passaggi di installazione.</span><span class="sxs-lookup"><span data-stu-id="de645-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="de645-139">I passaggi seguenti usano Visual Studio 2012 per creare un'applicazione Web ASP.NET vuota e aggiungere la libreria SignalR:</span><span class="sxs-lookup"><span data-stu-id="de645-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="de645-140">In Visual Studio creare un'applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="de645-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Crea Web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="de645-142">Aprire la **console di gestione pacchetti** selezionando **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="de645-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="de645-143">Immettere il comando seguente nella finestra della console:</span><span class="sxs-lookup"><span data-stu-id="de645-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="de645-144">Questo comando installa la versione più recente di SignalR 1. x.</span><span class="sxs-lookup"><span data-stu-id="de645-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="de645-145">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Classe**.</span><span class="sxs-lookup"><span data-stu-id="de645-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="de645-146">Assegnare alla nuova classe il nome **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="de645-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="de645-147">In **Esplora soluzioni** espandere il nodo script.</span><span class="sxs-lookup"><span data-stu-id="de645-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="de645-148">Le librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="de645-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="de645-150">Sostituire il codice nella classe **ChatHub** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="de645-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="de645-151">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="de645-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="de645-152">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **classe applicazione globale** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de645-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Aggiungi globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="de645-154">Aggiungere le seguenti istruzioni `using` dopo le istruzioni `using` fornite nella classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="de645-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="de645-155">Aggiungere la seguente riga di codice nel metodo `Application_Start` della classe globale per registrare la route predefinita per gli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="de645-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="de645-156">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="de645-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="de645-157">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare pagina HTML e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de645-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="de645-158">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina HTML appena creata e scegliere **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="de645-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="de645-159">Sostituire il codice predefinito nella pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="de645-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="de645-160">**Salvare tutto** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="de645-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="de645-161">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="de645-161">Run the Sample</span></span>

1. <span data-ttu-id="de645-162">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="de645-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="de645-163">La pagina HTML viene caricata in un'istanza del browser e viene richiesto un nome utente.</span><span class="sxs-lookup"><span data-stu-id="de645-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="de645-165">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="de645-165">Enter a user name.</span></span>
3. <span data-ttu-id="de645-166">Copiare l'URL dalla riga di indirizzi del browser e usarlo per aprire altre due istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="de645-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="de645-167">In ogni istanza del browser immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="de645-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="de645-168">In ogni istanza del browser aggiungere un commento e fare clic su **Invia**.</span><span class="sxs-lookup"><span data-stu-id="de645-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="de645-169">I commenti devono essere visualizzati in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="de645-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="de645-170">Questa semplice applicazione di chat non mantiene il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="de645-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="de645-171">L'hub trasmette i commenti a tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="de645-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="de645-172">Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="de645-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="de645-173">Lo screenshot seguente illustra l'applicazione di chat in esecuzione in tre istanze del browser, tutte aggiornate quando un'istanza Invia un messaggio:</span><span class="sxs-lookup"><span data-stu-id="de645-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="de645-175">In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="de645-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="de645-176">È disponibile un file script denominato **Hub** che la libreria SignalR genera dinamicamente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="de645-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="de645-177">Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.</span><span class="sxs-lookup"><span data-stu-id="de645-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script Hub generato](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="de645-179">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="de645-179">Examine the Code</span></span>

<span data-ttu-id="de645-180">L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base: la creazione di un hub come oggetto di coordinamento principale nel server e l'uso della libreria di segnalazione jQuery per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="de645-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="de645-181">Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="de645-181">SignalR Hubs</span></span>

<span data-ttu-id="de645-182">Nell'esempio di codice la classe **ChatHub** deriva dalla classe **Microsoft. AspNet. SignalR. Hub** .</span><span class="sxs-lookup"><span data-stu-id="de645-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="de645-183">La derivazione dalla classe **Hub** è un modo utile per creare un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="de645-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="de645-184">È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli da script jQuery in una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="de645-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="de645-185">Nel codice della chat, i client chiamano il metodo **ChatHub. Send** per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="de645-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="de645-186">L'hub a sua volta invia il messaggio a tutti i client chiamando **clients. all. broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="de645-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="de645-187">Il metodo **Send** illustra diversi concetti sull'hub:</span><span class="sxs-lookup"><span data-stu-id="de645-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="de645-188">Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.</span><span class="sxs-lookup"><span data-stu-id="de645-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="de645-189">Usare la proprietà dinamica **Microsoft. AspNet. SignalR. Hub. clients** per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="de645-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="de645-190">Chiamare una funzione jQuery sul client (ad esempio la funzione `broadcastMessage`) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="de645-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="de645-191">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="de645-191">SignalR and jQuery</span></span>

<span data-ttu-id="de645-192">La pagina HTML nell'esempio di codice illustra come usare la libreria di SignalR jQuery per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="de645-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="de645-193">Le attività essenziali del codice stanno dichiarando un proxy per fare riferimento all'hub, dichiarando una funzione che il server può chiamare per eseguire il push del contenuto ai client e avviando una connessione per l'invio di messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="de645-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="de645-194">Il codice seguente dichiara un proxy per un hub.</span><span class="sxs-lookup"><span data-stu-id="de645-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="de645-195">In jQuery il riferimento alla classe server e ai relativi membri è in caso di cammello.</span><span class="sxs-lookup"><span data-stu-id="de645-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="de645-196">L'esempio di codice fa C# riferimento alla classe **ChatHub** in jQuery come **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="de645-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="de645-197">Il codice seguente illustra come creare una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="de645-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="de645-198">La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="de645-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="de645-199">Le due righe che codificano il contenuto HTML prima di visualizzarle sono facoltative e mostrano un modo semplice per impedire l'inserimento di script.</span><span class="sxs-lookup"><span data-stu-id="de645-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="de645-200">Il codice seguente illustra come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="de645-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="de645-201">Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click sul pulsante **Send** nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="de645-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="de645-202">Questo approccio assicura che la connessione venga stabilita prima dell'esecuzione del gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="de645-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="de645-203">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="de645-203">Next Steps</span></span>

<span data-ttu-id="de645-204">Si è appreso che SignalR è un Framework per la creazione di applicazioni Web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="de645-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="de645-205">Sono state inoltre apprese diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe Hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="de645-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="de645-206">È possibile rendere l'applicazione di esempio in questa esercitazione o in altre applicazioni SignalR disponibili su Internet distribuendo tali applicazioni a un provider di hosting.</span><span class="sxs-lookup"><span data-stu-id="de645-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="de645-207">Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un [account di valutazione gratuito di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="de645-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="de645-208">Per una procedura dettagliata su come distribuire l'applicazione SignalR di esempio, vedere [la pagina relativa alla pubblicazione del introduzione di esempio SignalR come sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="de645-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="de645-209">Per informazioni dettagliate su come distribuire un progetto Web di Visual Studio in un sito Web di Microsoft Azure, vedere [distribuzione di un'applicazione ASP.NET in un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="de645-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="de645-210">Nota: il trasporto WebSocket non è attualmente supportato per siti Web di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="de645-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="de645-211">Quando il trasporto WebSocket non è disponibile, SignalR utilizza gli altri trasporti disponibili come descritto nella sezione trasporti dell' [argomento Introduzione a SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="de645-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="de645-212">Per informazioni sui concetti più avanzati relativi agli sviluppi di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:</span><span class="sxs-lookup"><span data-stu-id="de645-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="de645-213">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="de645-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="de645-214">Esempi di SignalR e GitHub</span><span class="sxs-lookup"><span data-stu-id="de645-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="de645-215">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="de645-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
