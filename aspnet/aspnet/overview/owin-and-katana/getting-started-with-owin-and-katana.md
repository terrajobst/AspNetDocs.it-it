---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introduzione con OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584672"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="5014b-102">Introduzione a OWIN e Katana</span><span class="sxs-lookup"><span data-stu-id="5014b-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="5014b-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5014b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5014b-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="5014b-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="5014b-105">Disaccoppiando il server Web dall'applicazione, OWIN rende più semplice la creazione di middleware per lo sviluppo Web .NET.</span><span class="sxs-lookup"><span data-stu-id="5014b-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="5014b-106">OWIN semplifica inoltre la portabilità delle applicazioni Web in altri host&#8212;, ad esempio l'hosting self-service in un servizio di Windows o in un altro processo.</span><span class="sxs-lookup"><span data-stu-id="5014b-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="5014b-107">OWIN è una specifica di proprietà della community, non un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="5014b-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="5014b-108">Il progetto Katana è un set di componenti OWIN open source sviluppati da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5014b-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="5014b-109">Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="5014b-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="5014b-110">In questo articolo si passerà direttamente al codice per iniziare.</span><span class="sxs-lookup"><span data-stu-id="5014b-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="5014b-111">Questa esercitazione USA [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5014b-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="5014b-112">Alcuni passaggi sono diversi in Visual Studio 2012, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="5014b-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="5014b-113">OWIN host in IIS</span><span class="sxs-lookup"><span data-stu-id="5014b-113">Host OWIN in IIS</span></span>

<span data-ttu-id="5014b-114">In questa sezione si ospiterà OWIN in IIS.</span><span class="sxs-lookup"><span data-stu-id="5014b-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="5014b-115">Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN insieme al set di funzionalità di IIS più maturo.</span><span class="sxs-lookup"><span data-stu-id="5014b-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="5014b-116">Utilizzando questa opzione, l'applicazione OWIN viene eseguita nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5014b-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="5014b-117">Per prima cosa, creare un nuovo progetto di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5014b-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="5014b-118">(In Visual Studio 2012, utilizzare il tipo di progetto di applicazione Web ASP.NET vuoto).</span><span class="sxs-lookup"><span data-stu-id="5014b-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="5014b-119">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** .</span><span class="sxs-lookup"><span data-stu-id="5014b-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="5014b-120">Aggiungere pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="5014b-120">Add NuGet Packages</span></span>

<span data-ttu-id="5014b-121">Aggiungere quindi i pacchetti NuGet necessari.</span><span class="sxs-lookup"><span data-stu-id="5014b-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="5014b-122">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="5014b-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5014b-123">Nella finestra console di gestione pacchetti digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5014b-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="5014b-124">Aggiungere una classe Startup</span><span class="sxs-lookup"><span data-stu-id="5014b-124">Add a Startup Class</span></span>

<span data-ttu-id="5014b-125">Successivamente, aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="5014b-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="5014b-126">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="5014b-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="5014b-127">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **Owin Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="5014b-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="5014b-128">Per altre informazioni sulla configurazione della classe startup, vedere [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="5014b-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="5014b-129">Aggiungere al metodo `Startup1.Configuration` il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5014b-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="5014b-130">Questo codice aggiunge una semplice porzione di middleware alla pipeline OWIN, implementata come funzione che riceve un'istanza **Microsoft. OWIN. IOwinContext** .</span><span class="sxs-lookup"><span data-stu-id="5014b-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="5014b-131">Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware.</span><span class="sxs-lookup"><span data-stu-id="5014b-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="5014b-132">Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="5014b-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="5014b-133">Il modello della classe di avvio OWIN è disponibile in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5014b-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="5014b-134">Se si usa Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5014b-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="5014b-135">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5014b-135">Run the Application</span></span>

<span data-ttu-id="5014b-136">Premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="5014b-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="5014b-137">In Visual Studio viene aperta una finestra del browser per `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="5014b-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="5014b-138">La pagina dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="5014b-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="5014b-139">OWIN Self-host in un'applicazione console</span><span class="sxs-lookup"><span data-stu-id="5014b-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="5014b-140">È facile convertire questa applicazione dall'hosting di IIS al self-hosting in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="5014b-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="5014b-141">Con l'hosting di IIS, IIS funge sia da server HTTP che dal processo che ospita il servizio.</span><span class="sxs-lookup"><span data-stu-id="5014b-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="5014b-142">Con l'hosting automatico, l'applicazione crea il processo e usa la classe **HttpListener** come server http.</span><span class="sxs-lookup"><span data-stu-id="5014b-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="5014b-143">In Visual Studio creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="5014b-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="5014b-144">Nella finestra console di gestione pacchetti digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5014b-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="5014b-145">Aggiungere una classe `Startup1` dalla parte 1 di questa esercitazione al progetto.</span><span class="sxs-lookup"><span data-stu-id="5014b-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="5014b-146">Non è necessario modificare questa classe.</span><span class="sxs-lookup"><span data-stu-id="5014b-146">You don't need to modify this class.</span></span>

<span data-ttu-id="5014b-147">Implementare il metodo di `Main` dell'applicazione come segue.</span><span class="sxs-lookup"><span data-stu-id="5014b-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="5014b-148">Quando si esegue l'applicazione console, il server inizia l'ascolto della `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="5014b-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="5014b-149">Se si passa a questo indirizzo in un Web browser, verrà visualizzata la pagina "Hello World".</span><span class="sxs-lookup"><span data-stu-id="5014b-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="5014b-150">Aggiungi diagnostica OWIN</span><span class="sxs-lookup"><span data-stu-id="5014b-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="5014b-151">Il pacchetto Microsoft. Owin. Diagnostics contiene il middleware che rileva le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore.</span><span class="sxs-lookup"><span data-stu-id="5014b-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="5014b-152">Questa pagina funziona in modo molto simile alla pagina di errore ASP.NET, a volte detta "[schermata gialla della morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="5014b-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="5014b-153">Analogamente a YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarla in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="5014b-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="5014b-154">Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="5014b-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="5014b-155">Modificare il codice nel metodo di `Startup1.Configuration` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5014b-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="5014b-156">Utilizzare ora CTRL + F5 per eseguire l'applicazione senza eseguire il debug, in modo che Visual Studio non si interrompa in caso di eccezione.</span><span class="sxs-lookup"><span data-stu-id="5014b-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="5014b-157">L'applicazione si comporta come prima, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="5014b-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="5014b-158">Il middleware della pagina di errore rileverà l'eccezione e visualizzerà una pagina HTML con informazioni sull'errore.</span><span class="sxs-lookup"><span data-stu-id="5014b-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="5014b-159">È possibile fare clic sulle schede per visualizzare lo stack, la stringa di query, i cookie, l'intestazione della richiesta e le variabili di ambiente OWIN.</span><span class="sxs-lookup"><span data-stu-id="5014b-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="5014b-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="5014b-160">Next Steps</span></span>

- [<span data-ttu-id="5014b-161">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="5014b-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="5014b-162">Usare OWIN per ospitare autonomamente API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5014b-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="5014b-163">Usare OWIN per l'hosting automatico di SignalR</span><span class="sxs-lookup"><span data-stu-id="5014b-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
