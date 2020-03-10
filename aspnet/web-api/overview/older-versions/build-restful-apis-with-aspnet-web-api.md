---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Creazione di API RESTful con API Web ASP.NET-ASP.NET 4. x
author: rick-anderson
description: "Laboratorio pratico: usare l'API Web in ASP.NET 4. x per creare una semplice API REST per un'applicazione Contact Manager."
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621814"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="cc788-103">Creazione di API RESTful con API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cc788-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="cc788-104">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="cc788-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="cc788-105">Laboratorio pratico: usare l'API Web in ASP.NET 4. x per creare una semplice API REST per un'applicazione Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="cc788-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="cc788-106">Si creerà anche un client per l'utilizzo dell'API.</span><span class="sxs-lookup"><span data-stu-id="cc788-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="cc788-107">Negli ultimi anni è diventato chiaro che HTTP non è solo per servire le pagine HTML.</span><span class="sxs-lookup"><span data-stu-id="cc788-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="cc788-108">È anche una piattaforma potente per la creazione di API Web, usando un numero limitato di verbi (GET, POST e così via) più semplici concetti quali *URI* e *intestazioni*.</span><span class="sxs-lookup"><span data-stu-id="cc788-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="cc788-109">API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc788-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="cc788-110">Poiché si basa sul runtime di ASP.NET MVC, l'API Web gestisce automaticamente i dettagli del trasporto di basso livello di HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc788-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="cc788-111">Allo stesso tempo, l'API Web espone naturalmente il modello di programmazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc788-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="cc788-112">In realtà, uno degli obiettivi dell'API Web è *non* eliminare la realtà del protocollo http.</span><span class="sxs-lookup"><span data-stu-id="cc788-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="cc788-113">Di conseguenza, l'API Web è flessibile e facile da estendere.</span><span class="sxs-lookup"><span data-stu-id="cc788-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="cc788-114">Lo stile di architettura REST si è rivelato un modo efficace per sfruttare il protocollo HTTP, sebbene non sia l'unico approccio valido per HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc788-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="cc788-115">Il gestore dei contatti esporrà l'elenco RESTful per l'aggiunta e la rimozione di contatti, tra gli altri.</span><span class="sxs-lookup"><span data-stu-id="cc788-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="cc788-116">Questo Lab richiede una conoscenza di base di HTTP, REST e presuppone che si disponga di una conoscenza pratica di base di HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="cc788-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="cc788-117">Il sito Web di ASP.NET dispone di un'area dedicata al Framework API Web ASP.NET in [https://asp.net/web-api](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="cc788-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="cc788-118">Questo sito continuerà a fornire informazioni aggiornate, esempi e notizie correlate all'API Web, quindi controllare spesso se si vuole approfondire l'arte della creazione di API Web personalizzate disponibili praticamente per qualsiasi dispositivo o Framework di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="cc788-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="cc788-119">API Web ASP.NET, simile a ASP.NET MVC 4, presenta una grande flessibilità in termini di separazione del livello di servizio dai controller che consentono di usare in modo abbastanza semplice diversi framework di inserimento delle dipendenze disponibili.</span><span class="sxs-lookup"><span data-stu-id="cc788-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="cc788-120">In MSDN è disponibile un esempio che illustra come usare Ninject per l'inserimento di dipendenze in un progetto API Web ASP.NET che è possibile scaricare da [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="cc788-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="cc788-121">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="cc788-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="cc788-122">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="cc788-122">Objectives</span></span>

<span data-ttu-id="cc788-123">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="cc788-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="cc788-124">Implementare un'API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="cc788-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="cc788-125">Chiamare l'API da un client HTML</span><span class="sxs-lookup"><span data-stu-id="cc788-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="cc788-126">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cc788-126">Prerequisites</span></span>

<span data-ttu-id="cc788-127">Per completare questa esercitazione pratica, è necessario quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cc788-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="cc788-128">[Microsoft Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere l' [Appendice B](#AppendixB) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="cc788-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="cc788-129">Configurazione</span><span class="sxs-lookup"><span data-stu-id="cc788-129">Setup</span></span>

<span data-ttu-id="cc788-130">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="cc788-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="cc788-131">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc788-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="cc788-132">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="cc788-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="cc788-133">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice A: uso dei frammenti di codice](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="cc788-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="cc788-134">Esercizi</span><span class="sxs-lookup"><span data-stu-id="cc788-134">Exercises</span></span>

<span data-ttu-id="cc788-135">Questo laboratorio pratico include l'esercizio seguente:</span><span class="sxs-lookup"><span data-stu-id="cc788-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="cc788-136">Esercizio 1: creare un'API Web di sola lettura</span><span class="sxs-lookup"><span data-stu-id="cc788-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="cc788-137">Esercizio 2: creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="cc788-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="cc788-138">Esercizio 3: utilizzo dell'API Web da un client HTML</span><span class="sxs-lookup"><span data-stu-id="cc788-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="cc788-139">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="cc788-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="cc788-140">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="cc788-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="cc788-141">Tempo stimato per il completamento del Lab: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="cc788-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="cc788-142">Esercizio 1: creare un'API Web di sola lettura</span><span class="sxs-lookup"><span data-stu-id="cc788-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="cc788-143">In questo esercizio verrà implementato il metodo GET di sola lettura per Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="cc788-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="cc788-144">Attività 1: creazione del progetto API</span><span class="sxs-lookup"><span data-stu-id="cc788-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="cc788-145">In questa attività verranno usati i nuovi modelli di progetto Web ASP.NET per creare un'applicazione Web per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="cc788-146">Eseguire **Visual Studio 2012 Express per il Web**. a tale scopo, passare a **Start** e digitare **vs Express per il Web** quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="cc788-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="cc788-147">Scegliere **nuovo progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="cc788-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="cc788-148">Selezionare l' **oggetto C# visivo |** Tipo di progetto Web dalla visualizzazione albero dei tipi di progetto, quindi selezionare il tipo di progetto di **applicazione Web MVC 4 ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="cc788-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="cc788-149">Impostare il **nome** del progetto su *ContactManager* e il **nome della soluzione** da *iniziare*, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc788-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="cc788-150">![Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="cc788-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="cc788-151">*Creazione di un nuovo progetto di applicazione Web MVC 4,0 di ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="cc788-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="cc788-152">Nella finestra di dialogo ASP.NET MVC 4 Project Type selezionare il tipo di progetto **API Web** .</span><span class="sxs-lookup"><span data-stu-id="cc788-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="cc788-153">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc788-153">Click **OK**.</span></span>

    <span data-ttu-id="cc788-154">![Specifica del tipo di progetto API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifica del tipo di progetto API Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="cc788-155">*Specifica del tipo di progetto API Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="cc788-156">Attività 2: creazione dei controller dell'API Contact Manager</span><span class="sxs-lookup"><span data-stu-id="cc788-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="cc788-157">In questa attività verranno create le classi controller in cui risiederanno i metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="cc788-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="cc788-158">Eliminare il file denominato **ValuesController.cs** nella cartella **Controllers** dal progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="cc788-159">Fare clic con il pulsante destro del mouse sulla cartella **controller** nel progetto e scegliere **Aggiungi | Controller** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="cc788-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="cc788-160">![Aggiunta di un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Aggiunta di un nuovo controller al progetto")</span><span class="sxs-lookup"><span data-stu-id="cc788-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="cc788-161">*Aggiunta di un nuovo controller al progetto*</span><span class="sxs-lookup"><span data-stu-id="cc788-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="cc788-162">Nella finestra di dialogo **Aggiungi controller** visualizzata selezionare **controller API vuoto** dal menu modello.</span><span class="sxs-lookup"><span data-stu-id="cc788-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="cc788-163">Assegnare alla classe controller il nome **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="cc788-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="cc788-164">Quindi, fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="cc788-164">Then, click **Add.**</span></span>

    <span data-ttu-id="cc788-165">![Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="cc788-166">*Uso della finestra di dialogo Aggiungi controller per creare un nuovo controller API Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="cc788-167">Aggiungere il codice seguente a **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="cc788-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="cc788-168">(Frammento di codice- *Lab API Web-Ex01-metodo API Get*)</span><span class="sxs-lookup"><span data-stu-id="cc788-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="cc788-169">Premere **F5** per eseguire il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="cc788-170">Verrà visualizzata la home page predefinita per un progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="cc788-171">![Home page predefinito di un'applicazione API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Home page predefinito di un'applicazione API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="cc788-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="cc788-172">*Home page predefinito di un'applicazione API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="cc788-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="cc788-173">Nella finestra di Internet Explorer premere il tasto **F12** per aprire la finestra di **strumenti di sviluppo** .</span><span class="sxs-lookup"><span data-stu-id="cc788-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="cc788-174">Fare clic sulla scheda **rete** e quindi fare clic sul pulsante **Avvia acquisizione** per avviare l'acquisizione del traffico di rete nella finestra.</span><span class="sxs-lookup"><span data-stu-id="cc788-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="cc788-175">![Apertura della scheda rete e avvio dell'acquisizione di rete](build-restful-apis-with-aspnet-web-api/_static/image6.png "Apertura della scheda rete e avvio dell'acquisizione di rete")</span><span class="sxs-lookup"><span data-stu-id="cc788-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="cc788-176">*Apertura della scheda rete e avvio dell'acquisizione di rete*</span><span class="sxs-lookup"><span data-stu-id="cc788-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="cc788-177">Aggiungere l'URL nella barra degli indirizzi del browser con **/API/Contact** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="cc788-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="cc788-178">I dettagli della trasmissione verranno visualizzati nella finestra acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="cc788-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="cc788-179">Si noti che il tipo MIME della risposta è **Application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="cc788-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="cc788-180">Viene illustrato come il formato di output predefinito è JSON.</span><span class="sxs-lookup"><span data-stu-id="cc788-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="cc788-181">![Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete")</span><span class="sxs-lookup"><span data-stu-id="cc788-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="cc788-182">*Visualizzazione dell'output della richiesta dell'API Web nella visualizzazione di rete*</span><span class="sxs-lookup"><span data-stu-id="cc788-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-183">Il comportamento predefinito di Internet Explorer 10 a questo punto è chiedere se l'utente desidera salvare o aprire il flusso risultante dalla chiamata all'API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="cc788-184">L'output sarà un file di testo contenente il risultato JSON della chiamata URL dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="cc788-185">Non annullare la finestra di dialogo per poter controllare il contenuto della risposta tramite la finestra degli strumenti degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="cc788-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="cc788-186">Fare clic sul pulsante **Vai a visualizzazione dettagliata** per visualizzare altri dettagli sulla risposta della chiamata API.</span><span class="sxs-lookup"><span data-stu-id="cc788-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="cc788-187">![Passa alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "Passa alla visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="cc788-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="cc788-188">*Passa alla visualizzazione dettagliata*</span><span class="sxs-lookup"><span data-stu-id="cc788-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="cc788-189">Fare clic sulla scheda **corpo risposta** per visualizzare il testo effettivo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="cc788-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="cc788-190">![Visualizzazione del testo di output JSON in Network Monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Visualizzazione del testo di output JSON in Network Monitor")</span><span class="sxs-lookup"><span data-stu-id="cc788-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="cc788-191">*Visualizzazione del testo di output JSON in Network Monitor*</span><span class="sxs-lookup"><span data-stu-id="cc788-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="cc788-192">Attività 3: creazione dei modelli di contatto e aumento del controller di contatto</span><span class="sxs-lookup"><span data-stu-id="cc788-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="cc788-193">In questa attività verranno create le classi controller in cui risiederanno i metodi dell'API.</span><span class="sxs-lookup"><span data-stu-id="cc788-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="cc788-194">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi | Classe...** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="cc788-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="cc788-195">![Aggiunta di un nuovo modello all'applicazione Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Aggiunta di un nuovo modello all'applicazione Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="cc788-196">*Aggiunta di un nuovo modello all'applicazione Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="cc788-197">Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare al nuovo file il nome **Contact.cs** e fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="cc788-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="cc788-198">![Creazione del nuovo file della classe Contact](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creazione del nuovo file della classe Contact")</span><span class="sxs-lookup"><span data-stu-id="cc788-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="cc788-199">*Creazione del nuovo file della classe Contact*</span><span class="sxs-lookup"><span data-stu-id="cc788-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="cc788-200">Aggiungere il codice evidenziato seguente alla classe **Contact** .</span><span class="sxs-lookup"><span data-stu-id="cc788-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="cc788-201">(Frammento di codice- *API Web Lab-Ex01-classe Contact*)</span><span class="sxs-lookup"><span data-stu-id="cc788-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="cc788-202">Nella classe **ContactController** selezionare la parola **stringa** nella definizione del metodo **Get** e digitare la parola *Contact*.</span><span class="sxs-lookup"><span data-stu-id="cc788-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="cc788-203">Quando la parola viene digitata, viene visualizzato un indicatore all'inizio del **contatto**della parola.</span><span class="sxs-lookup"><span data-stu-id="cc788-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="cc788-204">Tenere premuto il tasto **CTRL** e premere il tasto punto (.) oppure fare clic sull'icona con il mouse per aprire la finestra di dialogo di assistenza nell'editor di codice per compilare automaticamente la direttiva **using** per lo spazio dei nomi Models.</span><span class="sxs-lookup"><span data-stu-id="cc788-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Utilizzo dell'assistenza IntelliSense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="cc788-206">*Utilizzo dell'assistenza IntelliSense per le dichiarazioni dello spazio dei nomi*</span><span class="sxs-lookup"><span data-stu-id="cc788-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="cc788-207">Modificare il codice per il metodo **Get** in modo che restituisca una matrice di istanze del modello Contact.</span><span class="sxs-lookup"><span data-stu-id="cc788-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="cc788-208">(Frammento di codice- *Lab API Web-Ex01-restituzione di un elenco di contatti*)</span><span class="sxs-lookup"><span data-stu-id="cc788-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="cc788-209">Premere **F5** per eseguire il debug dell'applicazione Web nel browser.</span><span class="sxs-lookup"><span data-stu-id="cc788-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="cc788-210">Per visualizzare le modifiche apportate all'output della risposta dell'API, seguire questa procedura.</span><span class="sxs-lookup"><span data-stu-id="cc788-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="cc788-211">Quando il browser si apre, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.</span><span class="sxs-lookup"><span data-stu-id="cc788-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="cc788-212">Fare clic sulla scheda **rete** .</span><span class="sxs-lookup"><span data-stu-id="cc788-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="cc788-213">Premere il pulsante **Avvia acquisizione** .</span><span class="sxs-lookup"><span data-stu-id="cc788-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="cc788-214">Aggiungere il suffisso URL **/API/Contact** all'URL nella barra degli indirizzi e premere il tasto **invio** .</span><span class="sxs-lookup"><span data-stu-id="cc788-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="cc788-215">Premere il pulsante **Vai a visualizzazione dettagliata** .</span><span class="sxs-lookup"><span data-stu-id="cc788-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="cc788-216">Selezionare la scheda **corpo risposta** . Verrà visualizzata una stringa JSON che rappresenta il formato serializzato di una matrice di istanze di contatto.</span><span class="sxs-lookup"><span data-stu-id="cc788-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="cc788-217">![Output serializzato JSON di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "Output serializzato JSON di una chiamata al metodo API Web complessa")</span><span class="sxs-lookup"><span data-stu-id="cc788-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="cc788-218">*Output serializzato JSON di una chiamata al metodo API Web complessa*</span><span class="sxs-lookup"><span data-stu-id="cc788-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="cc788-219">Attività 4: estrazione della funzionalità in un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="cc788-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="cc788-220">Questa attività illustra come estrarre le funzionalità in un livello di servizio per semplificare agli sviluppatori la separazione delle funzionalità del servizio dal livello controller, consentendo in tal modo la riusabilità dei servizi che effettivamente eseguono il lavoro.</span><span class="sxs-lookup"><span data-stu-id="cc788-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="cc788-221">Creare una nuova cartella nella radice della soluzione e denominarla **Servizi**.</span><span class="sxs-lookup"><span data-stu-id="cc788-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="cc788-222">A tale scopo, fare clic con il pulsante destro del mouse su progetto **ContactManager** , selezionare **Aggiungi** | **nuova cartella**, assegnare un nome ai *Servizi*it.</span><span class="sxs-lookup"><span data-stu-id="cc788-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="cc788-223">![Creazione della cartella dei servizi](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creazione della cartella dei servizi")</span><span class="sxs-lookup"><span data-stu-id="cc788-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="cc788-224">*Creazione della cartella dei servizi*</span><span class="sxs-lookup"><span data-stu-id="cc788-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="cc788-225">Fare clic con il pulsante destro del mouse sulla cartella **Servizi** e scegliere **Aggiungi | Classe...** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="cc788-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="cc788-226">![Aggiunta di una nuova classe alla cartella Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Aggiunta di una nuova classe alla cartella Services")</span><span class="sxs-lookup"><span data-stu-id="cc788-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="cc788-227">*Aggiunta di una nuova classe alla cartella Services*</span><span class="sxs-lookup"><span data-stu-id="cc788-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="cc788-228">Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare la nuova classe **ContactRepository** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="cc788-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="cc788-229">![Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti")</span><span class="sxs-lookup"><span data-stu-id="cc788-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="cc788-230">*Creazione di un file di classe per contenere il codice per il livello di servizio del repository di contatti*</span><span class="sxs-lookup"><span data-stu-id="cc788-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="cc788-231">Aggiungere una direttiva using al file **ContactRepository.cs** per includere lo spazio dei nomi Models.</span><span class="sxs-lookup"><span data-stu-id="cc788-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="cc788-232">Aggiungere il codice evidenziato seguente al file **ContactRepository.cs** per implementare il metodo GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="cc788-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="cc788-233">(Frammento di codice- *Lab API Web-Ex01-Contact repository*)</span><span class="sxs-lookup"><span data-stu-id="cc788-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="cc788-234">Aprire il file **ContactController.cs** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="cc788-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="cc788-235">Aggiungere l'istruzione using seguente alla sezione della dichiarazione dello spazio dei nomi del file.</span><span class="sxs-lookup"><span data-stu-id="cc788-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="cc788-236">Aggiungere il codice evidenziato seguente alla classe **ContactController.cs** per aggiungere un campo privato per rappresentare l'istanza del repository, in modo che il resto dei membri della classe possa usare l'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="cc788-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="cc788-237">(Frammento di codice- *Web API Lab-Ex01-Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="cc788-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="cc788-238">Modificare il metodo **Get** in modo che usi il servizio repository dei contatti.</span><span class="sxs-lookup"><span data-stu-id="cc788-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="cc788-239">(Frammento di codice- *Lab API Web-Ex01-restituzione di un elenco di contatti tramite il repository*)</span><span class="sxs-lookup"><span data-stu-id="cc788-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="cc788-240">Inserire un punto di interruzione nella definizione del metodo **Get** di **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="cc788-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="cc788-241">![Aggiunta di punti di interruzione al controller contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "Aggiunta di punti di interruzione al controller contatto")</span><span class="sxs-lookup"><span data-stu-id="cc788-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="cc788-242">*Aggiunta di punti di interruzione al controller contatto*</span><span class="sxs-lookup"><span data-stu-id="cc788-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="cc788-243">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="cc788-244">Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="cc788-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="cc788-245">Fare clic sulla scheda **rete** .</span><span class="sxs-lookup"><span data-stu-id="cc788-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="cc788-246">Fare clic sul pulsante **Avvia acquisizione** .</span><span class="sxs-lookup"><span data-stu-id="cc788-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="cc788-247">Aggiungere l'URL nella barra degli indirizzi con il suffisso **/API/Contact** e premere **invio** per caricare il controller API.</span><span class="sxs-lookup"><span data-stu-id="cc788-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="cc788-248">Visual Studio 2012 dovrebbe interrompersi una volta avviata l'esecuzione del metodo **Get** .</span><span class="sxs-lookup"><span data-stu-id="cc788-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="cc788-249">![Suddivisione all'interno del metodo Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Suddivisione all'interno del metodo Get")</span><span class="sxs-lookup"><span data-stu-id="cc788-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="cc788-250">*Suddivisione all'interno del metodo Get*</span><span class="sxs-lookup"><span data-stu-id="cc788-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="cc788-251">Premere **F5** per continuare.</span><span class="sxs-lookup"><span data-stu-id="cc788-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="cc788-252">Tornare a Internet Explorer, se non è già attivo.</span><span class="sxs-lookup"><span data-stu-id="cc788-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="cc788-253">Prendere nota della finestra acquisizione di rete.</span><span class="sxs-lookup"><span data-stu-id="cc788-253">Note the network capture window.</span></span>

    <span data-ttu-id="cc788-254">![Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="cc788-255">*Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata all'API Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="cc788-256">Fare clic sul pulsante **Vai a visualizzazione dettagliata** .</span><span class="sxs-lookup"><span data-stu-id="cc788-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="cc788-257">Fare clic sulla scheda **corpo risposta** . prendere nota dell'output JSON della chiamata API e del modo in cui rappresenta i due contatti recuperati dal livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="cc788-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="cc788-258">![Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo](build-restful-apis-with-aspnet-web-api/_static/image20.png "Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo")</span><span class="sxs-lookup"><span data-stu-id="cc788-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="cc788-259">*Visualizzazione dell'output JSON dall'API Web nella finestra strumenti di sviluppo*</span><span class="sxs-lookup"><span data-stu-id="cc788-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="cc788-260">Esercizio 2: creare un'API Web di lettura/scrittura</span><span class="sxs-lookup"><span data-stu-id="cc788-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="cc788-261">In questo esercizio si implementeranno i metodi POST e PUT per il responsabile del contatto per abilitarla con funzionalità di modifica dei dati.</span><span class="sxs-lookup"><span data-stu-id="cc788-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="cc788-262">Attività 1-apertura del progetto API Web</span><span class="sxs-lookup"><span data-stu-id="cc788-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="cc788-263">In questa attività verrà preparato per migliorare il progetto API Web creato nell'esercizio 1, in modo che possa accettare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="cc788-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="cc788-264">Eseguire **Visual Studio 2012 Express per il Web**. a tale scopo, passare a **Start** e digitare **vs Express per il Web** quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="cc788-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="cc788-265">Aprire la soluzione **Begin** disponibile nella cartella **source/Ex02-ReadWriteWebAPI/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="cc788-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="cc788-266">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="cc788-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cc788-267">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="cc788-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cc788-268">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cc788-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cc788-269">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="cc788-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cc788-270">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cc788-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cc788-271">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cc788-272">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cc788-273">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="cc788-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="cc788-274">Aprire il file **Services/ContactRepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="cc788-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="cc788-275">Attività 2: aggiunta di funzionalità di persistenza dei dati all'implementazione del repository di contatti</span><span class="sxs-lookup"><span data-stu-id="cc788-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="cc788-276">In questa attività verrà aumentata la classe ContactRepository del progetto API Web creato nell'esercizio 1, in modo che possa rendere permanente e accettare l'input dell'utente e le nuove istanze di contatto.</span><span class="sxs-lookup"><span data-stu-id="cc788-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="cc788-277">Aggiungere la costante seguente alla classe **ContactRepository** per rappresentare il nome della chiave dell'elemento della cache del server Web più avanti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="cc788-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="cc788-278">Aggiungere un costruttore a **ContactRepository** contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cc788-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="cc788-279">(Frammento di codice- *API Web Lab-Ex02-contattare il costruttore del repository*)</span><span class="sxs-lookup"><span data-stu-id="cc788-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="cc788-280">Modificare il codice per il metodo **GetAllContacts** , come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="cc788-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="cc788-281">(Frammento di codice- *Lab API Web-Ex02-Ottieni tutti i contatti*)</span><span class="sxs-lookup"><span data-stu-id="cc788-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="cc788-282">Questo esempio è a scopo dimostrativo e utilizzerà la cache del server Web come supporto di archiviazione, in modo che i valori saranno disponibili per più client simultaneamente, anziché usare un meccanismo di archiviazione della sessione o una durata di archiviazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="cc788-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="cc788-283">È possibile utilizzare Entity Framework, archiviazione XML o qualsiasi altra varietà al posto della cache del server Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="cc788-284">Implementare un nuovo metodo denominato **SaveContact** alla classe **ContactRepository** per eseguire il salvataggio di un contatto.</span><span class="sxs-lookup"><span data-stu-id="cc788-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="cc788-285">Il metodo **SaveContact** deve prendere un solo parametro di **contatto** e restituire un valore booleano che indica l'esito positivo o negativo.</span><span class="sxs-lookup"><span data-stu-id="cc788-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="cc788-286">(Frammento di codice- *Lab API Web-Ex02-implementazione del metodo SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="cc788-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="cc788-287">Esercizio 3: utilizzo dell'API Web da un client HTML</span><span class="sxs-lookup"><span data-stu-id="cc788-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="cc788-288">In questo esercizio verrà creato un client HTML per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="cc788-289">Questo client faciliterà lo scambio di dati con l'API Web tramite JavaScript e visualizzerà i risultati in un Web browser usando il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="cc788-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="cc788-290">Attività 1: modifica della visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti</span><span class="sxs-lookup"><span data-stu-id="cc788-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="cc788-291">In questa attività verrà modificata la visualizzazione Indice predefinita dell'applicazione Web per supportare il requisito di visualizzazione dell'elenco dei contatti esistenti in un browser HTML.</span><span class="sxs-lookup"><span data-stu-id="cc788-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="cc788-292">Aprire **Visual Studio 2012 Express per il Web** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="cc788-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="cc788-293">Aprire la soluzione **Begin** disponibile nella cartella **source/Ex03-ConsumingWebAPI/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="cc788-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="cc788-294">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="cc788-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cc788-295">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="cc788-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cc788-296">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cc788-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cc788-297">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="cc788-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cc788-298">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cc788-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cc788-299">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cc788-300">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cc788-301">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="cc788-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="cc788-302">Aprire il file **index. cshtml** che si trova nella cartella **views/Home** .</span><span class="sxs-lookup"><span data-stu-id="cc788-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="cc788-303">Sostituire il codice HTML all'interno dell'elemento div con il **corpo** ID in modo che appaia come il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cc788-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="cc788-304">Aggiungere il codice JavaScript seguente nella parte inferiore del file per eseguire la richiesta HTTP all'API Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="cc788-305">Aprire il file **ContactController.cs** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="cc788-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="cc788-306">Inserire un punto di interruzione nel metodo **Get** della classe **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="cc788-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="cc788-307">![Posizionamento di un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Posizionamento di un punto di interruzione nel metodo Get del controller API")</span><span class="sxs-lookup"><span data-stu-id="cc788-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="cc788-308">*Posizionamento di un punto di interruzione nel metodo Get del controller API*</span><span class="sxs-lookup"><span data-stu-id="cc788-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="cc788-309">Premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="cc788-309">Press **F5** to run the project.</span></span> <span data-ttu-id="cc788-310">Il documento HTML viene caricato dal browser.</span><span class="sxs-lookup"><span data-stu-id="cc788-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-311">Assicurarsi di passare all'URL radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="cc788-312">Dopo il caricamento della pagina e l'esecuzione di JavaScript, il punto di interruzione viene raggiunto e l'esecuzione del codice viene sospesa nel controller.</span><span class="sxs-lookup"><span data-stu-id="cc788-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="cc788-313">![Debug nelle chiamate API Web con VS Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debug nelle chiamate API Web con VS Express per il Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="cc788-314">*Debug nella chiamata API Web con Visual Studio 2012 Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="cc788-315">Rimuovere il punto di interruzione e premere **F5** o il pulsante **continua** della barra degli strumenti di debug per continuare a caricare la visualizzazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="cc788-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="cc788-316">Al termine della chiamata dell'API Web, i contatti restituiti dalla chiamata API Web vengono visualizzati come elementi elenco nel browser.</span><span class="sxs-lookup"><span data-stu-id="cc788-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="cc788-317">![Risultati della chiamata API visualizzata nel browser come elementi elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "Risultati della chiamata API visualizzata nel browser come elementi elenco")</span><span class="sxs-lookup"><span data-stu-id="cc788-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="cc788-318">*Risultati della chiamata API visualizzata nel browser come elementi elenco*</span><span class="sxs-lookup"><span data-stu-id="cc788-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="cc788-319">Terminare il debug.</span><span class="sxs-lookup"><span data-stu-id="cc788-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="cc788-320">Attività 2: modifica della visualizzazione index per fornire un'interfaccia utente grafica per la creazione di contatti</span><span class="sxs-lookup"><span data-stu-id="cc788-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="cc788-321">In questa attività si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="cc788-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="cc788-322">Un modulo verrà aggiunto alla pagina HTML che acquisirà l'input dell'utente e lo invierà all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo del controller API Web per la raccolta della data dall'interfaccia utente grafica.</span><span class="sxs-lookup"><span data-stu-id="cc788-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="cc788-323">Aprire il file **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="cc788-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="cc788-324">Aggiungere un nuovo metodo alla classe controller denominata **post** , come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cc788-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="cc788-325">(Frammento di codice- *Web API Lab-Ex03-metodo post*)</span><span class="sxs-lookup"><span data-stu-id="cc788-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="cc788-326">Aprire il file **index. cshtml** in Visual Studio, se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="cc788-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="cc788-327">Aggiungere il codice HTML seguente al file subito dopo l'elenco non ordinato aggiunto nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="cc788-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="cc788-328">All'interno dell'elemento script nella parte inferiore del documento, aggiungere il codice evidenziato seguente per gestire gli eventi click del pulsante, che invieranno i dati all'API Web usando una chiamata HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="cc788-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="cc788-329">In **ContactController.cs**inserire un punto di interruzione nel metodo **post** .</span><span class="sxs-lookup"><span data-stu-id="cc788-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="cc788-330">Premere **F5** per eseguire l'applicazione nel browser.</span><span class="sxs-lookup"><span data-stu-id="cc788-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="cc788-331">Dopo che la pagina è stata caricata nel browser, digitare un nuovo nome e un ID contatto e fare clic sul pulsante **Salva** .</span><span class="sxs-lookup"><span data-stu-id="cc788-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="cc788-332">![Documento HTML client caricato nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "Documento HTML client caricato nel browser")</span><span class="sxs-lookup"><span data-stu-id="cc788-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="cc788-333">*Documento HTML client caricato nel browser*</span><span class="sxs-lookup"><span data-stu-id="cc788-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="cc788-334">Quando la finestra del debugger si interrompe nel metodo **post** , esaminare le proprietà del parametro **Contact** .</span><span class="sxs-lookup"><span data-stu-id="cc788-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="cc788-335">I valori devono corrispondere ai dati immessi nel form.</span><span class="sxs-lookup"><span data-stu-id="cc788-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="cc788-336">![L'oggetto contatto inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "L'oggetto contatto inviato all'API Web dal client")</span><span class="sxs-lookup"><span data-stu-id="cc788-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="cc788-337">*L'oggetto contatto inviato all'API Web dal client*</span><span class="sxs-lookup"><span data-stu-id="cc788-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="cc788-338">Scorrere il metodo nel debugger fino a quando non viene creata la variabile di **risposta** .</span><span class="sxs-lookup"><span data-stu-id="cc788-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="cc788-339">Dopo l'ispezione nella finestra **variabili locali** del debugger, si noterà che tutte le proprietà sono state impostate.</span><span class="sxs-lookup"><span data-stu-id="cc788-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="cc788-340">![Risposta successiva alla creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "Risposta successiva alla creazione nel debugger")</span><span class="sxs-lookup"><span data-stu-id="cc788-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="cc788-341">*Risposta successiva alla creazione nel debugger*</span><span class="sxs-lookup"><span data-stu-id="cc788-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="cc788-342">Se si preme **F5** o si fa clic su **continua** nel debugger, la richiesta verrà completata.</span><span class="sxs-lookup"><span data-stu-id="cc788-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="cc788-343">Quando si torna al browser, il nuovo contatto è stato aggiunto all'elenco dei contatti archiviati dall'implementazione di **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="cc788-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="cc788-344">![Il browser riflette la creazione della nuova istanza del contatto](build-restful-apis-with-aspnet-web-api/_static/image27.png "Il browser riflette la creazione della nuova istanza del contatto")</span><span class="sxs-lookup"><span data-stu-id="cc788-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="cc788-345">*Il browser riflette la creazione della nuova istanza del contatto*</span><span class="sxs-lookup"><span data-stu-id="cc788-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="cc788-346">Inoltre, è possibile distribuire l'applicazione in Azure dopo [l'Appendice C: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="cc788-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="cc788-347">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="cc788-347">Summary</span></span>

<span data-ttu-id="cc788-348">In questo laboratorio è stato introdotto il nuovo Framework di API Web ASP.NET e l'implementazione di API Web RESTful tramite il Framework.</span><span class="sxs-lookup"><span data-stu-id="cc788-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="cc788-349">Da qui, è possibile creare un nuovo repository che facilita la persistenza dei dati usando un numero qualsiasi di meccanismi e collegare tale servizio anziché quello semplice fornito come esempio in questo Lab.</span><span class="sxs-lookup"><span data-stu-id="cc788-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="cc788-350">API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione della comunicazione da client non HTML scritti in qualsiasi linguaggio che supporti HTTP e JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="cc788-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="cc788-351">È anche possibile ospitare un'API Web all'esterno di una tipica applicazione Web, nonché la possibilità di creare formati di serializzazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="cc788-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="cc788-352">Il sito Web di ASP.NET dispone di un'area dedicata al Framework API Web ASP.NET in [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="cc788-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="cc788-353">Questo sito continuerà a fornire informazioni aggiornate, esempi e notizie correlate all'API Web, quindi controllare spesso se si vuole approfondire l'arte della creazione di API Web personalizzate disponibili praticamente per qualsiasi dispositivo o Framework di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="cc788-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="cc788-354">Appendice A: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="cc788-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="cc788-355">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="cc788-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="cc788-356">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="cc788-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="cc788-357">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="cc788-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="cc788-358">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="cc788-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="cc788-359">Per aggiungere un frammento di codice usando laC# tastiera (solo)</span><span class="sxs-lookup"><span data-stu-id="cc788-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="cc788-360">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="cc788-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="cc788-361">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="cc788-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="cc788-362">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="cc788-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="cc788-363">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="cc788-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="cc788-364">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="cc788-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="cc788-365">![Inizia a digitare il nome del frammento](build-restful-apis-with-aspnet-web-api/_static/image29.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="cc788-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="cc788-366">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="cc788-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="cc788-367">![Premere TAB per selezionare il frammento evidenziato](build-restful-apis-with-aspnet-web-api/_static/image30.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="cc788-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="cc788-368">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="cc788-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="cc788-369">![Premere nuovamente TAB per espandere il frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image31.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="cc788-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="cc788-370">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="cc788-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="cc788-371">Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="cc788-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="cc788-372">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="cc788-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="cc788-373">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="cc788-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="cc788-374">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="cc788-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="cc788-375">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](build-restful-apis-with-aspnet-web-api/_static/image32.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="cc788-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="cc788-376">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="cc788-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="cc788-377">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="cc788-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="cc788-378">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="cc788-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="cc788-379">Appendice B: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="cc788-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="cc788-380">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="cc788-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="cc788-381">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="cc788-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="cc788-382">Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="cc788-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="cc788-383">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="cc788-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="cc788-384">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="cc788-384">Click on **Install Now**.</span></span> <span data-ttu-id="cc788-385">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="cc788-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="cc788-386">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="cc788-387">![Installa Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="cc788-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="cc788-388">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="cc788-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="cc788-389">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="cc788-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="cc788-391">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="cc788-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="cc788-392">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-392">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="cc788-394">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="cc788-394">*Installation progress*</span></span>
6. <span data-ttu-id="cc788-395">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="cc788-395">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="cc788-397">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="cc788-397">*Installation completed*</span></span>
7. <span data-ttu-id="cc788-398">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="cc788-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="cc788-399">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="cc788-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="cc788-401">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="cc788-402">Appendice C: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="cc788-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="cc788-403">In questa appendice verrà illustrato come creare un nuovo sito Web dal portale di Azure e pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="cc788-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="cc788-404">Attività 1: creazione di un nuovo sito Web dal portale di Azure</span><span class="sxs-lookup"><span data-stu-id="cc788-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="cc788-405">Accedere al [portale di gestione di Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="cc788-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-406">Con Azure puoi ospitare gratuitamente 10 siti Web ASP.NET, quindi ridimensionarli in base alla crescita del traffico.</span><span class="sxs-lookup"><span data-stu-id="cc788-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="cc788-407">È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="cc788-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="cc788-408">![Accedere a Windows portale di Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Accedere a Windows portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="cc788-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="cc788-409">*Accedi al portale*</span><span class="sxs-lookup"><span data-stu-id="cc788-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="cc788-410">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="cc788-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="cc788-411">![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creazione di un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="cc788-412">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="cc788-413">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="cc788-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="cc788-414">Quindi selezionare opzione **creazione rapida** .</span><span class="sxs-lookup"><span data-stu-id="cc788-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="cc788-415">Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="cc788-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-416">Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="cc788-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="cc788-417">L'opzione Creazione rapida consente di distribuire un'applicazione Web completata in Azure dall'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="cc788-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="cc788-418">Non sono inclusi i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="cc788-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="cc788-419">![Creazione di un nuovo sito Web mediante creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creazione di un nuovo sito Web mediante creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="cc788-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="cc788-420">*Creazione di un nuovo sito Web mediante creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="cc788-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="cc788-421">Attendere la creazione del nuovo **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="cc788-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="cc788-422">Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** .</span><span class="sxs-lookup"><span data-stu-id="cc788-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="cc788-423">Verificare che il nuovo sito Web sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="cc788-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="cc788-424">![Esplorazione del nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Esplorazione del nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="cc788-425">*Esplorazione del nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="cc788-426">![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "Sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="cc788-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="cc788-427">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="cc788-427">*Web site running*</span></span>
6. <span data-ttu-id="cc788-428">Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="cc788-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="cc788-429">![Apertura delle pagine di gestione del sito Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Apertura delle pagine di gestione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="cc788-430">*Apertura delle pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="cc788-431">Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .</span><span class="sxs-lookup"><span data-stu-id="cc788-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-432">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="cc788-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="cc788-433">Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cc788-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="cc788-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="cc788-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="cc788-435">![Download del profilo di pubblicazione del sito Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Download del profilo di pubblicazione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="cc788-436">*Download del profilo di pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="cc788-437">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="cc788-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="cc788-438">Più avanti in questo esercizio si vedrà come usare questo file per pubblicare un'applicazione Web in Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc788-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="cc788-439">![Salvataggio del file del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "Salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="cc788-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="cc788-440">*Salvataggio del file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="cc788-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="cc788-441">Attività 2: configurazione del server di database</span><span class="sxs-lookup"><span data-stu-id="cc788-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="cc788-442">Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="cc788-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="cc788-443">Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="cc788-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="cc788-444">Per archiviare il database dell'applicazione, sarà necessario un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="cc788-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="cc788-445">È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Azure in **database sql** | **Server** | **Dashboard del server**.</span><span class="sxs-lookup"><span data-stu-id="cc788-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="cc788-446">Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="cc788-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="cc788-447">Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive.</span><span class="sxs-lookup"><span data-stu-id="cc788-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="cc788-448">Non creare ancora il database, perché verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="cc788-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="cc788-449">![Dashboard del server di database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard del server di database SQL")</span><span class="sxs-lookup"><span data-stu-id="cc788-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="cc788-450">*Dashboard del server di database SQL*</span><span class="sxs-lookup"><span data-stu-id="cc788-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="cc788-451">Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server.</span><span class="sxs-lookup"><span data-stu-id="cc788-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="cc788-452">A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](build-restful-apis-with-aspnet-web-api/_static/image48.png).</span><span class="sxs-lookup"><span data-stu-id="cc788-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Aggiunta dell'indirizzo IP del client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="cc788-454">*Aggiunta dell'indirizzo IP del client*</span><span class="sxs-lookup"><span data-stu-id="cc788-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="cc788-455">Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="cc788-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Conferma modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="cc788-457">*Conferma modifiche*</span><span class="sxs-lookup"><span data-stu-id="cc788-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="cc788-458">Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="cc788-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="cc788-459">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cc788-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="cc788-460">Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="cc788-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="cc788-461">![Pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "Pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="cc788-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="cc788-462">*Pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="cc788-463">Importare il profilo di pubblicazione salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="cc788-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="cc788-464">![Importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="cc788-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="cc788-465">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="cc788-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="cc788-466">Fare clic su **convalida connessione**.</span><span class="sxs-lookup"><span data-stu-id="cc788-466">Click **Validate Connection**.</span></span> <span data-ttu-id="cc788-467">Al termine della convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="cc788-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc788-468">La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="cc788-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="cc788-469">![Convalida della connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "Convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="cc788-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="cc788-470">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="cc788-470">*Validating connection*</span></span>
4. <span data-ttu-id="cc788-471">Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.</span><span class="sxs-lookup"><span data-stu-id="cc788-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="cc788-472">![Configurazione distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configurazione distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="cc788-473">*Configurazione distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="cc788-474">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="cc788-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="cc788-475">In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="cc788-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="cc788-476">In **nome utente** Digitare il nome di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="cc788-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="cc788-477">In **password** Digitare la password di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="cc788-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="cc788-478">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="cc788-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="cc788-479">![Configurazione della stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurazione della stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="cc788-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="cc788-480">*Configurazione della stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="cc788-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="cc788-481">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc788-481">Then click **OK**.</span></span> <span data-ttu-id="cc788-482">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="cc788-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="cc788-483">![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="cc788-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="cc788-484">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="cc788-484">*Creating the database*</span></span>
7. <span data-ttu-id="cc788-485">La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita).</span><span class="sxs-lookup"><span data-stu-id="cc788-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="cc788-486">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="cc788-486">Then click **Next**.</span></span>

    <span data-ttu-id="cc788-487">![Stringa di connessione che punta al database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "Stringa di connessione che punta al database SQL")</span><span class="sxs-lookup"><span data-stu-id="cc788-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="cc788-488">*Stringa di connessione che punta al database SQL*</span><span class="sxs-lookup"><span data-stu-id="cc788-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="cc788-489">Nella pagina **Anteprima** fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="cc788-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="cc788-490">![Pubblicazione dell'applicazione Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Pubblicazione dell'applicazione Web")</span><span class="sxs-lookup"><span data-stu-id="cc788-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="cc788-491">*Pubblicazione dell'applicazione Web*</span><span class="sxs-lookup"><span data-stu-id="cc788-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="cc788-492">Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="cc788-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="cc788-493">![Applicazione pubblicata in Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="cc788-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="cc788-494">*Applicazione pubblicata in Azure*</span><span class="sxs-lookup"><span data-stu-id="cc788-494">*Application published to Azure*</span></span>
