---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornisce filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. I filtri azione sono attributi personalizzati Tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579695"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="c85d6-104">Filtri per azioni personalizzati di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c85d6-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="c85d6-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c85d6-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c85d6-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="c85d6-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c85d6-107">ASP.NET MVC fornisce filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="c85d6-108">I filtri azione sono attributi personalizzati che forniscono strumenti dichiarativi per aggiungere il comportamento di pre-azione e post-azione ai metodi di azione del controller.</span><span class="sxs-lookup"><span data-stu-id="c85d6-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="c85d6-109">In questo laboratorio pratico verrà creato un attributo di filtro azioni personalizzato nella soluzione MvcMusicStore per rilevare le richieste del controller e registrare l'attività di un sito in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="c85d6-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="c85d6-110">Sarà possibile aggiungere il filtro di registrazione per inserimento a qualsiasi controller o azione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="c85d6-111">Infine, verrà visualizzata la visualizzazione log che mostra l'elenco dei visitatori.</span><span class="sxs-lookup"><span data-stu-id="c85d6-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="c85d6-112">Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="c85d6-113">Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC 4 Nozioni di base** sul Lab pratico.</span><span class="sxs-lookup"><span data-stu-id="c85d6-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="c85d6-114">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c85d6-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c85d6-115">Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Custom Action filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="c85d6-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c85d6-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="c85d6-116">Objectives</span></span>

<span data-ttu-id="c85d6-117">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="c85d6-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="c85d6-118">Creare un attributo del filtro azioni personalizzato per estendere le funzionalità di filtro</span><span class="sxs-lookup"><span data-stu-id="c85d6-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="c85d6-119">Applicare un attributo di filtro personalizzato per inserimento a un livello specifico</span><span class="sxs-lookup"><span data-stu-id="c85d6-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="c85d6-120">Registrare i filtri azione personalizzati a livello globale</span><span class="sxs-lookup"><span data-stu-id="c85d6-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c85d6-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c85d6-121">Prerequisites</span></span>

<span data-ttu-id="c85d6-122">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c85d6-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c85d6-123">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="c85d6-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c85d6-124">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c85d6-124">Setup</span></span>

<span data-ttu-id="c85d6-125">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="c85d6-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="c85d6-126">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c85d6-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c85d6-127">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c85d6-128">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c85d6-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c85d6-129">Esercizi</span><span class="sxs-lookup"><span data-stu-id="c85d6-129">Exercises</span></span>

<span data-ttu-id="c85d6-130">Questo laboratorio pratico è costituito dagli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c85d6-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c85d6-131">Esercizio 1: registrazione di azioni</span><span class="sxs-lookup"><span data-stu-id="c85d6-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="c85d6-132">Esercizio 2: gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="c85d6-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="c85d6-133">Tempo stimato per il completamento del Lab: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="c85d6-134">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="c85d6-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c85d6-135">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="c85d6-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="c85d6-136">Esercizio 1: registrazione di azioni</span><span class="sxs-lookup"><span data-stu-id="c85d6-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="c85d6-137">In questo esercizio verrà illustrato come creare un filtro personalizzato per i log delle azioni utilizzando i provider di filtri ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c85d6-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="c85d6-138">A tale scopo, viene applicato un filtro di registrazione al sito di MusicStore che registrerà tutte le attività nei controller selezionati.</span><span class="sxs-lookup"><span data-stu-id="c85d6-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="c85d6-139">Il filtro estenderà **ActionFilterAttributeClass** ed eseguirà l'override del metodo **OnActionExecuting** per intercettare ogni richiesta e quindi eseguire le azioni di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="c85d6-140">Le informazioni di contesto sulle richieste HTTP, l'esecuzione di metodi, risultati e parametri verranno fornite dalla classe **ACTIONEXECUTINGCONTEXT** MVC ASP.NET **.**</span><span class="sxs-lookup"><span data-stu-id="c85d6-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="c85d6-141">Per ASP.NET MVC 4 sono inoltre disponibili provider di filtri predefiniti che è possibile utilizzare senza creare un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="c85d6-142">ASP.NET MVC 4 fornisce i tipi di filtri seguenti:</span><span class="sxs-lookup"><span data-stu-id="c85d6-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="c85d6-143">Filtro di **autorizzazione** , che consente di decidere se eseguire un metodo di azione, ad esempio l'esecuzione dell'autenticazione o la convalida delle proprietà della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c85d6-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="c85d6-144">Filtro **azioni** , che esegue il wrapping dell'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="c85d6-145">Questo filtro può eseguire un'elaborazione aggiuntiva, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="c85d6-146">Filtro dei **risultati** , che esegue il wrapping dell'esecuzione dell'oggetto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="c85d6-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="c85d6-147">Questo filtro può eseguire un'ulteriore elaborazione del risultato, ad esempio la modifica della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c85d6-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="c85d6-148">Filtro **eccezioni** , che viene eseguito se si verifica un'eccezione non gestita in un punto del metodo di azione, iniziando con i filtri di autorizzazione e terminando con l'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="c85d6-149">I filtri eccezioni possono essere utilizzati per attività quali la registrazione o la visualizzazione di una pagina di errore.</span><span class="sxs-lookup"><span data-stu-id="c85d6-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="c85d6-150">Per ulteriori informazioni sui provider di filtri, visitare il collegamento MSDN seguente: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="c85d6-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="c85d6-151">Informazioni sulla funzionalità di registrazione delle applicazioni MVC Music Store</span><span class="sxs-lookup"><span data-stu-id="c85d6-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="c85d6-152">Questa soluzione di Music Store include una nuova tabella del modello di dati per la registrazione del sito, **actionlog**, con i campi seguenti: nome del controller che ha ricevuto una richiesta, chiamata azione, IP client e timestamp.</span><span class="sxs-lookup"><span data-stu-id="c85d6-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="c85d6-153">![Modello di dati. Tabella ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="c85d6-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="c85d6-154">*Modello di dati-tabella ActionLog*</span><span class="sxs-lookup"><span data-stu-id="c85d6-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="c85d6-155">La soluzione fornisce una visualizzazione MVC ASP.NET per il log delle azioni che si trova in **MvcMusicStores/views/actionlog**:</span><span class="sxs-lookup"><span data-stu-id="c85d6-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="c85d6-156">![Visualizzazione del log delle azioni](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizzazione del log delle azioni")</span><span class="sxs-lookup"><span data-stu-id="c85d6-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="c85d6-157">*Visualizzazione del log delle azioni*</span><span class="sxs-lookup"><span data-stu-id="c85d6-157">*Action Log view*</span></span>

<span data-ttu-id="c85d6-158">Con questa struttura specificata, tutte le operazioni saranno incentrate sull'interruzione della richiesta del controller e sull'esecuzione della registrazione tramite il filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="c85d6-159">Attività 1: creazione di un filtro personalizzato per intercettare la richiesta di un controller</span><span class="sxs-lookup"><span data-stu-id="c85d6-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="c85d6-160">In questa attività verrà creata una classe di attributi di filtro personalizzata che conterrà la logica di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="c85d6-161">A tale scopo, si estenderà la classe ASP.NET MVC **ActionFilterAttribute** e si implementerà l'interfaccia **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="c85d6-162">**ActionFilterAttribute** è la classe di base per tutti i filtri di attributo.</span><span class="sxs-lookup"><span data-stu-id="c85d6-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="c85d6-163">Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="c85d6-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="c85d6-164">**OnActionExecuting**(ActionExecutingContext filterContext): immediatamente prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="c85d6-165">**OnActionExecuted**(ActionExecutedContext filterContext): dopo che il metodo di azione è stato chiamato e prima che venga eseguito il risultato (prima del rendering della visualizzazione).</span><span class="sxs-lookup"><span data-stu-id="c85d6-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c85d6-166">**OnResultExecuting**(ResultExecutingContext filterContext): immediatamente prima dell'esecuzione del risultato (prima del rendering della visualizzazione).</span><span class="sxs-lookup"><span data-stu-id="c85d6-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c85d6-167">**OnResultExecuted**(ResultExecutedContext filterContext): dopo l'esecuzione del risultato, dopo il rendering della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="c85d6-168">Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire un codice di filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="c85d6-169">Aprire la soluzione **Begin** disponibile nella cartella **\Source\Ex01-LoggingActions\Begin** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="c85d6-170">Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti.</span><span class="sxs-lookup"><span data-stu-id="c85d6-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="c85d6-171">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c85d6-172">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="c85d6-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c85d6-173">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c85d6-174">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="c85d6-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c85d6-175">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="c85d6-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c85d6-176">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="c85d6-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c85d6-177">Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c85d6-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c85d6-178">Aggiungere una nuova C# classe nella cartella **Filters** e denominarla *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="c85d6-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="c85d6-179">In questa cartella vengono archiviati tutti i filtri personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c85d6-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="c85d6-180">Aprire **CustomActionFilter.cs** e aggiungere un riferimento agli spazi dei nomi **System. Web. Mvc** e **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="c85d6-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="c85d6-181">(Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c85d6-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="c85d6-182">Ereditare la classe **CustomActionFilter** da **ActionFilterAttribute** e quindi fare in modo che la classe **CustomActionFilter** implementi l'interfaccia **IActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="c85d6-183">Make **CustomActionFilter** Class esegue l'override del metodo **OnActionExecuting** e aggiunge la logica necessaria per registrare l'esecuzione del filtro.</span><span class="sxs-lookup"><span data-stu-id="c85d6-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="c85d6-184">A tale scopo, aggiungere il codice evidenziato seguente nella classe **CustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="c85d6-185">(Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="c85d6-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="c85d6-186">Il metodo **OnActionExecuting** USA **Entity Framework** per aggiungere un nuovo registro di actionlog.</span><span class="sxs-lookup"><span data-stu-id="c85d6-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="c85d6-187">Crea e riempie una nuova istanza dell'entità con le informazioni di contesto di **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="c85d6-188">Per altre informazioni sulla classe **ControllerContext** , vedere [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="c85d6-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="c85d6-189">Attività 2: inserire un intercettore di codice nella classe del controller di archiviazione</span><span class="sxs-lookup"><span data-stu-id="c85d6-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="c85d6-190">In questa attività verrà aggiunto il filtro personalizzato inserendolo in tutte le classi controller e le azioni del controller che verranno registrate.</span><span class="sxs-lookup"><span data-stu-id="c85d6-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="c85d6-191">Ai fini di questo esercizio, la classe Store controller avrà un log.</span><span class="sxs-lookup"><span data-stu-id="c85d6-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="c85d6-192">Il metodo **OnActionExecuting** dal filtro personalizzato **ActionLogFilterAttribute** viene eseguito quando viene chiamato un elemento inserito.</span><span class="sxs-lookup"><span data-stu-id="c85d6-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="c85d6-193">È anche possibile intercettare un metodo del controller specifico.</span><span class="sxs-lookup"><span data-stu-id="c85d6-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="c85d6-194">Aprire il **StoreController** in **MvcMusicStore\Controllers** e aggiungere un riferimento allo spazio dei nomi **Filters** :</span><span class="sxs-lookup"><span data-stu-id="c85d6-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="c85d6-195">Inserire il filtro personalizzato **CustomActionFilter** nella classe **StoreController** aggiungendo l'attributo **[CustomActionFilter]** prima della dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="c85d6-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="c85d6-196">Quando un filtro viene inserito in una classe controller, vengono inserite anche tutte le relative azioni.</span><span class="sxs-lookup"><span data-stu-id="c85d6-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="c85d6-197">Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ognuno di essi:</span><span class="sxs-lookup"><span data-stu-id="c85d6-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c85d6-198">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="c85d6-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="c85d6-199">In questa attività verrà testato il funzionamento del filtro di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="c85d6-200">L'applicazione verrà avviata e sarà possibile visitare lo Store, quindi si verificheranno le attività registrate.</span><span class="sxs-lookup"><span data-stu-id="c85d6-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="c85d6-201">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c85d6-202">Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log:</span><span class="sxs-lookup"><span data-stu-id="c85d6-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="c85d6-203">![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "Stato log Tracker prima dell'attività della pagina")</span><span class="sxs-lookup"><span data-stu-id="c85d6-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c85d6-204">*Stato log Tracker prima dell'attività della pagina*</span><span class="sxs-lookup"><span data-stu-id="c85d6-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="c85d6-205">Per impostazione predefinita, viene sempre visualizzato un elemento generato durante il recupero dei generi esistenti per il menu.</span><span class="sxs-lookup"><span data-stu-id="c85d6-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="c85d6-206">Per semplicità, viene eseguita la pulizia della tabella **actionlog** ogni volta che l'applicazione viene eseguita in modo da visualizzare solo i log della verifica di ogni particolare attività.</span><span class="sxs-lookup"><span data-stu-id="c85d6-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="c85d6-207">Potrebbe essere necessario rimuovere il codice seguente dalla **sessione\_metodo Start** (nella classe **Global. asax** ), per salvare un log cronologico per tutte le azioni eseguite all'interno del controller di archivio.</span><span class="sxs-lookup"><span data-stu-id="c85d6-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="c85d6-208">Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="c85d6-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="c85d6-209">Passare a **/actionlog** e, se il registro è vuoto, premere **F5** per aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="c85d6-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="c85d6-210">Verificare che le visite siano state registrate:</span><span class="sxs-lookup"><span data-stu-id="c85d6-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="c85d6-211">![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image4.png "Log delle azioni con attività registrata")</span><span class="sxs-lookup"><span data-stu-id="c85d6-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c85d6-212">*Log delle azioni con attività registrata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="c85d6-213">Esercizio 2: gestione di più filtri azione</span><span class="sxs-lookup"><span data-stu-id="c85d6-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="c85d6-214">In questo esercizio verrà aggiunto un secondo filtro azioni personalizzato alla classe StoreController e verrà definito l'ordine specifico in cui verranno eseguiti entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="c85d6-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="c85d6-215">Il codice verrà quindi aggiornato per registrare il filtro a livello globale.</span><span class="sxs-lookup"><span data-stu-id="c85d6-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="c85d6-216">Sono disponibili diverse opzioni da tenere in considerazione quando si definisce l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="c85d6-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="c85d6-217">Ad esempio, la proprietà Order e l'ambito filters:</span><span class="sxs-lookup"><span data-stu-id="c85d6-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="c85d6-218">È possibile definire un **ambito** per ogni filtro. ad esempio, è possibile definire l'ambito di tutti i filtri azione da eseguire all'interno dell' **ambito del controller**e di tutti i filtri di autorizzazione per l'esecuzione nell' **ambito globale**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="c85d6-219">Gli ambiti hanno un ordine di esecuzione definito.</span><span class="sxs-lookup"><span data-stu-id="c85d6-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="c85d6-220">Ogni filtro azioni dispone inoltre di una proprietà Order utilizzata per determinare l'ordine di esecuzione nell'ambito del filtro.</span><span class="sxs-lookup"><span data-stu-id="c85d6-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="c85d6-221">Per ulteriori informazioni sull'ordine di esecuzione dei filtri azione personalizzati, visitare questo articolo di MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="c85d6-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="c85d6-222">Attività 1: creazione di un nuovo filtro azioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="c85d6-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="c85d6-223">In questa attività verrà creato un nuovo filtro azioni personalizzato da inserire nella classe StoreController, in cui viene illustrato come gestire l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="c85d6-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="c85d6-224">Aprire la soluzione **Begin** disponibile nella cartella **\Source\Ex02-ManagingMultipleActionFilters\Begin** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="c85d6-225">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c85d6-226">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="c85d6-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c85d6-227">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c85d6-228">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="c85d6-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c85d6-229">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c85d6-230">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="c85d6-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c85d6-231">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="c85d6-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c85d6-232">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="c85d6-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="c85d6-233">Per altre informazioni, vedere questo articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c85d6-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c85d6-234">Aggiungere una nuova C# classe nella cartella **Filters** e denominarla *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="c85d6-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="c85d6-235">Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System. Web. Mvc** e allo spazio dei nomi **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="c85d6-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="c85d6-236">(Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c85d6-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="c85d6-237">Sostituire la dichiarazione di classe predefinita con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="c85d6-238">(Frammento di codice: *ASP.NET MVC 4 Custom Action filters-EX2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="c85d6-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="c85d6-239">Questo filtro azioni personalizzato è quasi uguale a quello creato nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="c85d6-240">La differenza principale consiste nel fatto che il *&quot;registrato da&quot;* attributo aggiornato con il nome della nuova classe per identificare quale filtro ha registrato il log.</span><span class="sxs-lookup"><span data-stu-id="c85d6-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="c85d6-241">Attività 2: inserimento di un nuovo intercettore di codice nella classe StoreController</span><span class="sxs-lookup"><span data-stu-id="c85d6-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="c85d6-242">In questa attività si aggiungerà un nuovo filtro personalizzato alla classe StoreController e si eseguirà la soluzione per verificare il funzionamento combinato di entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="c85d6-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="c85d6-243">Aprire la classe **StoreController** che si trova in **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** nella classe **StoreController** , come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="c85d6-244">A questo punto, eseguire l'applicazione per vedere come funzionano questi due filtri azioni personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c85d6-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="c85d6-245">A tale scopo, premere **F5** e attendere che l'applicazione venga avviata.</span><span class="sxs-lookup"><span data-stu-id="c85d6-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c85d6-246">Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log.</span><span class="sxs-lookup"><span data-stu-id="c85d6-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c85d6-247">![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "Stato log Tracker prima dell'attività della pagina")</span><span class="sxs-lookup"><span data-stu-id="c85d6-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c85d6-248">*Stato log Tracker prima dell'attività della pagina*</span><span class="sxs-lookup"><span data-stu-id="c85d6-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c85d6-249">Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="c85d6-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c85d6-250">Verificare questa ora; le visite sono state rilevate due volte: una volta per ogni filtro azioni personalizzato aggiunto nella classe **controller** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="c85d6-251">![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image6.png "Log delle azioni con attività registrata")</span><span class="sxs-lookup"><span data-stu-id="c85d6-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c85d6-252">*Log delle azioni con attività registrata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c85d6-253">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="c85d6-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="c85d6-254">Attività 3: gestione dell'ordinamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="c85d6-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="c85d6-255">In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri usando la proprietà Order.</span><span class="sxs-lookup"><span data-stu-id="c85d6-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="c85d6-256">Aprire la classe **StoreController** che si trova in **MvcMusicStore\Controllers** e specificare la proprietà **Order** in entrambi i filtri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c85d6-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="c85d6-257">A questo punto, verificare il modo in cui vengono eseguiti i filtri a seconda del valore della proprietà Order.</span><span class="sxs-lookup"><span data-stu-id="c85d6-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="c85d6-258">Si noterà che il filtro con il valore di ordine più piccolo (**CustomActionFilter**) è il primo che viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="c85d6-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="c85d6-259">Premere **F5** e attendere che l'applicazione venga avviata.</span><span class="sxs-lookup"><span data-stu-id="c85d6-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c85d6-260">Passare a **/actionlog** per visualizzare lo stato iniziale della visualizzazione log.</span><span class="sxs-lookup"><span data-stu-id="c85d6-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c85d6-261">![Stato log Tracker prima dell'attività della pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "Stato log Tracker prima dell'attività della pagina")</span><span class="sxs-lookup"><span data-stu-id="c85d6-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c85d6-262">*Stato log Tracker prima dell'attività della pagina*</span><span class="sxs-lookup"><span data-stu-id="c85d6-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c85d6-263">Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="c85d6-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c85d6-264">Controllare che questa volta le visite siano state registrate in base ai filtri ' Order Value: **CustomActionFilter** Logs ' First.</span><span class="sxs-lookup"><span data-stu-id="c85d6-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="c85d6-265">![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image8.png "Log delle azioni con attività registrata")</span><span class="sxs-lookup"><span data-stu-id="c85d6-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c85d6-266">*Log delle azioni con attività registrata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c85d6-267">A questo punto, si aggiornerà il valore dell'ordine dei filtri e si verificherà il modo in cui cambia l'ordine di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="c85d6-268">Nella classe **StoreController** aggiornare il valore dell'ordine dei filtri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c85d6-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="c85d6-269">Eseguire di nuovo l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="c85d6-270">Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="c85d6-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="c85d6-271">Verificare che questa volta venga visualizzato per primo i log creati dal filtro **MyNewCustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="c85d6-272">![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image9.png "Log delle azioni con attività registrata")</span><span class="sxs-lookup"><span data-stu-id="c85d6-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c85d6-273">*Log delle azioni con attività registrata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="c85d6-274">Attività 4: registrazione dei filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="c85d6-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="c85d6-275">In questa attività verrà aggiornata la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale.</span><span class="sxs-lookup"><span data-stu-id="c85d6-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="c85d6-276">In questo modo, verrà attivata da tutte le azioni eseguite nell'applicazione e non solo in quelle StoreController come nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="c85d6-277">Nella classe **StoreController** rimuovere l'attributo **[MyNewCustomActionFilter]** e la proprietà Order da **[CustomActionFilter]** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="c85d6-278">Il risultato sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c85d6-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="c85d6-279">Aprire il file **Global. asax** e individuare l' **applicazione\_metodo Start** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="c85d6-280">Si noti che ogni volta che l'applicazione viene avviata, registra i filtri globali chiamando il metodo **RegisterGlobalFilters** nella classe **FilterConfig** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="c85d6-281">![Registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrazione di filtri globali in Global. asax")</span><span class="sxs-lookup"><span data-stu-id="c85d6-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="c85d6-282">*Registrazione di filtri globali in Global. asax*</span><span class="sxs-lookup"><span data-stu-id="c85d6-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="c85d6-283">Aprire il file **FilterConfig.cs** all'interno dell' **app\_** cartella di avvio.</span><span class="sxs-lookup"><span data-stu-id="c85d6-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="c85d6-284">Aggiungere un riferimento a utilizzando System. Web. Mvc; utilizzo di MvcMusicStore. filters; namespace.</span><span class="sxs-lookup"><span data-stu-id="c85d6-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="c85d6-285">Aggiornare il metodo **RegisterGlobalFilters** aggiungendo il filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="c85d6-286">A tale scopo, aggiungere il codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="c85d6-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="c85d6-287">Eseguire l'applicazione premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="c85d6-288">Fare clic su uno dei **generi** dal menu ed eseguire alcune azioni, ad esempio l'esplorazione di un album disponibile.</span><span class="sxs-lookup"><span data-stu-id="c85d6-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="c85d6-289">Controllare che ora **[MyNewCustomActionFilter]** venga inserito anche in HomeController e ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="c85d6-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="c85d6-290">![Log delle azioni con attività registrata](aspnet-mvc-4-custom-action-filters/_static/image11.png "Log delle azioni con attività registrata")</span><span class="sxs-lookup"><span data-stu-id="c85d6-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c85d6-291">*Log delle azioni con attività globale registrata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="c85d6-292">Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c85d6-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c85d6-293">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c85d6-293">Summary</span></span>

<span data-ttu-id="c85d6-294">Completando questa esercitazione pratica si è appreso come estendere un filtro azioni per eseguire azioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="c85d6-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="c85d6-295">Si è inoltre appreso come inserire qualsiasi filtro per i controller di pagina.</span><span class="sxs-lookup"><span data-stu-id="c85d6-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="c85d6-296">Sono stati usati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c85d6-296">The following concepts were used:</span></span>

- <span data-ttu-id="c85d6-297">Come creare filtri azione personalizzati con la classe ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="c85d6-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="c85d6-298">Come inserire filtri nei controller MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c85d6-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="c85d6-299">Come gestire l'ordinamento dei filtri usando la proprietà Order</span><span class="sxs-lookup"><span data-stu-id="c85d6-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="c85d6-300">Come registrare i filtri a livello globale</span><span class="sxs-lookup"><span data-stu-id="c85d6-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c85d6-301">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="c85d6-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c85d6-302">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c85d6-303">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="c85d6-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c85d6-304">Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c85d6-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c85d6-305">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c85d6-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c85d6-306">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-306">Click on **Install Now**.</span></span> <span data-ttu-id="c85d6-307">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="c85d6-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c85d6-308">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c85d6-309">![Installa Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c85d6-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c85d6-310">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c85d6-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c85d6-311">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="c85d6-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="c85d6-313">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="c85d6-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c85d6-314">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-314">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="c85d6-316">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-316">*Installation progress*</span></span>
6. <span data-ttu-id="c85d6-317">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-317">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="c85d6-319">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="c85d6-319">*Installation completed*</span></span>
7. <span data-ttu-id="c85d6-320">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="c85d6-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c85d6-321">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="c85d6-323">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c85d6-324">Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="c85d6-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c85d6-325">In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c85d6-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c85d6-326">Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure</span><span class="sxs-lookup"><span data-stu-id="c85d6-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c85d6-327">Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c85d6-328">Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico.</span><span class="sxs-lookup"><span data-stu-id="c85d6-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c85d6-329">È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c85d6-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c85d6-330">![Accedere a Windows portale di Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Accedere a Windows portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="c85d6-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c85d6-331">*Accedere a portale di gestione di Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="c85d6-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c85d6-332">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c85d6-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c85d6-333">![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creazione di un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c85d6-334">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c85d6-335">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c85d6-336">Quindi selezionare opzione **creazione rapida** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="c85d6-337">Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c85d6-338">Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="c85d6-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c85d6-339">L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="c85d6-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c85d6-340">Non sono inclusi i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="c85d6-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c85d6-341">![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creazione di un nuovo sito Web mediante creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="c85d6-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c85d6-342">*Creazione di un nuovo sito Web mediante creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="c85d6-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c85d6-343">Attendere la creazione del nuovo **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c85d6-344">Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c85d6-345">Verificare che il nuovo sito Web sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="c85d6-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c85d6-346">![Esplorazione del nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Esplorazione del nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c85d6-347">*Esplorazione del nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c85d6-348">![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "Sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="c85d6-349">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-349">*Web site running*</span></span>
6. <span data-ttu-id="c85d6-350">Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c85d6-351">![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Apertura delle pagine di gestione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c85d6-352">*Apertura delle pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c85d6-353">Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .</span><span class="sxs-lookup"><span data-stu-id="c85d6-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c85d6-354">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c85d6-355">Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c85d6-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c85d6-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c85d6-357">![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Download del profilo di pubblicazione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c85d6-358">*Download del profilo di pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c85d6-359">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="c85d6-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c85d6-360">Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c85d6-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c85d6-361">![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "Salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c85d6-362">*Salvataggio del file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c85d6-363">Attività 2: configurazione del server di database</span><span class="sxs-lookup"><span data-stu-id="c85d6-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c85d6-364">Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="c85d6-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c85d6-365">Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="c85d6-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c85d6-366">Per archiviare il database dell'applicazione, sarà necessario un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="c85d6-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c85d6-367">È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c85d6-368">Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c85d6-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c85d6-369">Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive.</span><span class="sxs-lookup"><span data-stu-id="c85d6-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c85d6-370">Non creare ancora il database, perché verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="c85d6-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c85d6-371">![Dashboard del server di database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard del server di database SQL")</span><span class="sxs-lookup"><span data-stu-id="c85d6-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c85d6-372">*Dashboard del server di database SQL*</span><span class="sxs-lookup"><span data-stu-id="c85d6-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c85d6-373">Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server.</span><span class="sxs-lookup"><span data-stu-id="c85d6-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c85d6-374">A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-custom-action-filters/_static/image26.png).</span><span class="sxs-lookup"><span data-stu-id="c85d6-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="c85d6-376">*Aggiunta dell'indirizzo IP del client*</span><span class="sxs-lookup"><span data-stu-id="c85d6-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c85d6-377">Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="c85d6-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Conferma modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="c85d6-379">*Conferma modifiche*</span><span class="sxs-lookup"><span data-stu-id="c85d6-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c85d6-380">Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="c85d6-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c85d6-381">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c85d6-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c85d6-382">Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c85d6-383">![Pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "Pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="c85d6-384">*Pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="c85d6-385">Importare il profilo di pubblicazione salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="c85d6-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c85d6-386">![Importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c85d6-387">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="c85d6-388">Fare clic su **convalida connessione**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-388">Click **Validate Connection**.</span></span> <span data-ttu-id="c85d6-389">Al termine della convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c85d6-390">La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="c85d6-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c85d6-391">![Convalida della connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "Convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="c85d6-392">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-392">*Validating connection*</span></span>
4. <span data-ttu-id="c85d6-393">Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c85d6-394">![Configurazione distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configurazione distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c85d6-395">*Configurazione distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c85d6-396">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="c85d6-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="c85d6-397">In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="c85d6-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="c85d6-398">In **nome utente** Digitare il nome di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="c85d6-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="c85d6-399">In **password** Digitare la password di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="c85d6-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="c85d6-400">Digitare un nuovo nome di database.</span><span class="sxs-lookup"><span data-stu-id="c85d6-400">Type a new database name.</span></span>

     <span data-ttu-id="c85d6-401">![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurazione della stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="c85d6-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="c85d6-402">*Configurazione della stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="c85d6-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c85d6-403">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-403">Then click **OK**.</span></span> <span data-ttu-id="c85d6-404">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c85d6-405">![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="c85d6-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="c85d6-406">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="c85d6-406">*Creating the database*</span></span>
7. <span data-ttu-id="c85d6-407">La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita).</span><span class="sxs-lookup"><span data-stu-id="c85d6-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c85d6-408">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-408">Then click **Next**.</span></span>

    <span data-ttu-id="c85d6-409">![Stringa di connessione che punta al database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "Stringa di connessione che punta al database SQL")</span><span class="sxs-lookup"><span data-stu-id="c85d6-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c85d6-410">*Stringa di connessione che punta al database SQL*</span><span class="sxs-lookup"><span data-stu-id="c85d6-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c85d6-411">Nella pagina **Anteprima** fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c85d6-412">![Pubblicazione dell'applicazione Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Pubblicazione dell'applicazione Web")</span><span class="sxs-lookup"><span data-stu-id="c85d6-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="c85d6-413">*Pubblicazione dell'applicazione Web*</span><span class="sxs-lookup"><span data-stu-id="c85d6-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="c85d6-414">Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="c85d6-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="c85d6-415">Appendice C: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="c85d6-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="c85d6-416">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="c85d6-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c85d6-417">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="c85d6-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c85d6-418">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="c85d6-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c85d6-419">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="c85d6-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c85d6-420">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="c85d6-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c85d6-421">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="c85d6-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c85d6-422">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="c85d6-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c85d6-423">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="c85d6-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c85d6-424">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="c85d6-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c85d6-425">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="c85d6-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c85d6-426">![Inizia a digitare il nome del frammento](aspnet-mvc-4-custom-action-filters/_static/image38.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="c85d6-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c85d6-427">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="c85d6-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="c85d6-428">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-custom-action-filters/_static/image39.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="c85d6-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c85d6-429">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="c85d6-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c85d6-430">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image40.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="c85d6-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c85d6-431">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="c85d6-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c85d6-432">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c85d6-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c85d6-433">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="c85d6-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c85d6-434">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="c85d6-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c85d6-435">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="c85d6-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c85d6-436">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-custom-action-filters/_static/image41.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="c85d6-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c85d6-437">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="c85d6-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c85d6-438">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="c85d6-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c85d6-439">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="c85d6-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
