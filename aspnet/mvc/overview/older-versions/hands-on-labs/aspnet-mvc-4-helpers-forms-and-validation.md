---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Helper, form e convalida di MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: Nei modelli ASP.NET MVC 4 e nell'esercitazione pratica di accesso ai dati è stato caricato e visualizzato dati dal database. In questo laboratorio pratico verrà aggiunto al...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539578"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="fac5f-104">Helper, moduli e convalida di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fac5f-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="fac5f-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="fac5f-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="fac5f-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="fac5f-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="fac5f-107">Nei **modelli ASP.NET MVC 4 e** nell'esercitazione pratica di accesso ai dati è stato caricato e visualizzato dati dal database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="fac5f-108">In questo laboratorio pratico verrà aggiunto all'applicazione **Music Store** la possibilità di modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="fac5f-109">Tenendo presente questo obiettivo, prima di tutto si creerà il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) degli album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="fac5f-110">Si genererà un modello di vista indice che sfrutta i vantaggi della funzionalità di ponteggi di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="fac5f-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="fac5f-111">Per migliorare la visualizzazione, si aggiungerà un helper HTML personalizzato che tronca le descrizioni lunghe.</span><span class="sxs-lookup"><span data-stu-id="fac5f-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="fac5f-112">Successivamente, si aggiungeranno le visualizzazioni modifica e crea che consentono di modificare gli album nel database, con l'ausilio di elementi del modulo come elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="fac5f-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="fac5f-113">Infine, si consentirà agli utenti di eliminare un album e anche di impedire l'immissione di dati errati convalidando l'input.</span><span class="sxs-lookup"><span data-stu-id="fac5f-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="fac5f-114">Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="fac5f-115">Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC nozioni di base** sui Lab pratici.</span><span class="sxs-lookup"><span data-stu-id="fac5f-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="fac5f-116">In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="fac5f-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="fac5f-117">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="fac5f-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="fac5f-118">Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Helper, moduli e convalida](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="fac5f-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="fac5f-119">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="fac5f-119">Objectives</span></span>

<span data-ttu-id="fac5f-120">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="fac5f-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="fac5f-121">Creare un controller per supportare le operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="fac5f-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="fac5f-122">Generare una visualizzazione index per visualizzare le proprietà delle entità in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="fac5f-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="fac5f-123">Aggiungere un helper HTML personalizzato</span><span class="sxs-lookup"><span data-stu-id="fac5f-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="fac5f-124">Creare e personalizzare una visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="fac5f-125">Distinguere tra i metodi di azione che reagiscono alle chiamate HTTP-GET o HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="fac5f-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="fac5f-126">Aggiungere e personalizzare una vista di creazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-126">Add and customize a Create View</span></span>
- <span data-ttu-id="fac5f-127">Gestire l'eliminazione di un'entità</span><span class="sxs-lookup"><span data-stu-id="fac5f-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="fac5f-128">Convalidare l'input utente</span><span class="sxs-lookup"><span data-stu-id="fac5f-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="fac5f-129">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fac5f-129">Prerequisites</span></span>

<span data-ttu-id="fac5f-130">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fac5f-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="fac5f-131">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="fac5f-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="fac5f-132">Configurazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-132">Setup</span></span>

<span data-ttu-id="fac5f-133">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="fac5f-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="fac5f-134">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fac5f-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="fac5f-135">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="fac5f-136">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice B: uso dei frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="fac5f-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="fac5f-137">Esercizi</span><span class="sxs-lookup"><span data-stu-id="fac5f-137">Exercises</span></span>

<span data-ttu-id="fac5f-138">Gli esercizi seguenti costituiscono questa esercitazione pratica:</span><span class="sxs-lookup"><span data-stu-id="fac5f-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="fac5f-139">Creazione del controller di Store Manager e della relativa visualizzazione index</span><span class="sxs-lookup"><span data-stu-id="fac5f-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="fac5f-140">Aggiunta di un helper HTML</span><span class="sxs-lookup"><span data-stu-id="fac5f-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="fac5f-141">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="fac5f-142">Aggiunta di una visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="fac5f-143">Gestione dell'eliminazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="fac5f-144">Aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="fac5f-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="fac5f-145">Uso di jQuery non intrusivo sul lato client</span><span class="sxs-lookup"><span data-stu-id="fac5f-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="fac5f-146">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="fac5f-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="fac5f-147">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="fac5f-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="fac5f-148">Tempo stimato per il completamento del Lab: **60 minuti**</span><span class="sxs-lookup"><span data-stu-id="fac5f-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="fac5f-149">Esercizio 1: creazione del controller di Store Manager e della relativa visualizzazione index</span><span class="sxs-lookup"><span data-stu-id="fac5f-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="fac5f-150">In questo esercizio verrà illustrato come creare un nuovo controller per supportare le operazioni CRUD, personalizzare il metodo di azione dell'indice per restituire un elenco di album dal database e infine generare un modello di visualizzazione degli indici che sfrutta i vantaggi dell'impalcatura di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="fac5f-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="fac5f-151">Attività 1: creazione del StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="fac5f-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="fac5f-152">In questa attività verrà creato un nuovo controller denominato **StoreManagerController** per supportare le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="fac5f-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="fac5f-153">Aprire la soluzione **Begin** disponibile nella cartella **source/EX1-CreatingTheStoreManagerController/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="fac5f-154">Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-155">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-156">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-157">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-158">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-159">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-160">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-161">Aggiungere un nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="fac5f-161">Add a new controller.</span></span> <span data-ttu-id="fac5f-162">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Controllers** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi il comando **controller** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="fac5f-163">Modificare il **nome** del controller in **StoreManagerController** e assicurarsi che sia selezionata l'opzione **controller MVC con azioni di lettura/scrittura vuote** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="fac5f-164">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-164">Click **Add**.</span></span>

    <span data-ttu-id="fac5f-165">![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Finestra di dialogo Aggiungi controller")</span><span class="sxs-lookup"><span data-stu-id="fac5f-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="fac5f-166">*Finestra di dialogo Aggiungi controller*</span><span class="sxs-lookup"><span data-stu-id="fac5f-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="fac5f-167">Viene generata una nuova classe controller.</span><span class="sxs-lookup"><span data-stu-id="fac5f-167">A new Controller class is generated.</span></span> <span data-ttu-id="fac5f-168">Poiché è stato indicato di aggiungere azioni per la lettura/scrittura, i metodi stub per tali azioni, le azioni CRUD comuni vengono create con commenti TODO compilati e viene richiesto di includere la logica specifica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="fac5f-169">Attività 2: personalizzazione dell'indice StoreManager</span><span class="sxs-lookup"><span data-stu-id="fac5f-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="fac5f-170">In questa attività verrà personalizzato il metodo di azione dell'indice StoreManager per restituire una visualizzazione con l'elenco di album del database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="fac5f-171">Nella classe StoreManagerController aggiungere le direttive *using* seguenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="fac5f-172">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX1 using MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="fac5f-173">Aggiungere un campo a **StoreManagerController** per mantenere un'istanza di **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="fac5f-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="fac5f-174">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="fac5f-175">Implementare l'azione StoreManagerController index per restituire una visualizzazione con l'elenco di album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="fac5f-176">La logica dell'azione del controller sarà molto simile all'azione dell'indice di StoreController scritta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fac5f-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="fac5f-177">Usare LINQ per recuperare tutti gli album, incluse le informazioni sul genere e sull'artista per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="fac5f-178">(Frammento di codice: *ASP.NET MVC 4 Helper e Forms and Validation-EX1 StoreManagerController index*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="fac5f-179">Attività 3-creazione della vista index</span><span class="sxs-lookup"><span data-stu-id="fac5f-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="fac5f-180">In questa attività verrà creato il modello di visualizzazione indice per visualizzare l'elenco di album restituiti dal controller **StoreManager** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="fac5f-181">Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che la **finestra di dialogo Aggiungi visualizzazione** conosca la classe **album** da usare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="fac5f-182">Seleziona **compilazione | Compilare MvcMusicStore** per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="fac5f-183">Fare clic con il pulsante destro del mouse all'interno del metodo di azione **index** e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="fac5f-184">Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="fac5f-185">![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="fac5f-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="fac5f-186">*Aggiunta di una vista dall'interno del metodo index*</span><span class="sxs-lookup"><span data-stu-id="fac5f-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="fac5f-187">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **index**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="fac5f-188">Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="fac5f-189">Selezionare **elenco dall'elenco** a discesa **modello di impalcatura** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="fac5f-190">Lasciare il **motore di visualizzazione** a **Razor** e gli altri campi con il relativo valore predefinito, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="fac5f-191">![Aggiunta di una visualizzazione indice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Aggiunta di una visualizzazione indice")</span><span class="sxs-lookup"><span data-stu-id="fac5f-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="fac5f-192">*Aggiunta di una visualizzazione indice*</span><span class="sxs-lookup"><span data-stu-id="fac5f-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="fac5f-193">Attività 4: personalizzazione del patibolo della vista index</span><span class="sxs-lookup"><span data-stu-id="fac5f-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="fac5f-194">In questa attività si modificherà il modello di visualizzazione semplice creato con la funzionalità di ponteggi MVC ASP.NET per visualizzare i campi desiderati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="fac5f-195">Il supporto dell' **impalcatura** all'interno di ASP.NET MVC genera un modello di visualizzazione semplice che elenca tutti i campi nel modello di album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="fac5f-196">L' **impalcatura** fornisce un modo rapido per iniziare a usare una visualizzazione fortemente tipizzata: anziché dover scrivere manualmente il modello di visualizzazione, l'impalcatura genera rapidamente un modello predefinito, quindi è possibile modificare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="fac5f-197">Esaminare il codice creato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-197">Review the code created.</span></span> <span data-ttu-id="fac5f-198">L'elenco generato dei campi sarà parte della tabella HTML seguente utilizzata dalle **impalcature** per la visualizzazione dei dati tabulari.</span><span class="sxs-lookup"><span data-stu-id="fac5f-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="fac5f-199">Sostituire la **tabella&lt;&gt;** codice con il codice seguente per visualizzare solo i campi **genere**, **artista**, **Titolo album**e **Prezzo** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="fac5f-200">Verranno eliminate le colonne URL **AlbumId** e **album Art** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="fac5f-201">Modifica anche le colonne GenreId e ArtistId per visualizzare le proprietà della classe collegata di **Artist.Name** e **genre.Name**e rimuove il collegamento **Dettagli** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="fac5f-202">Modificare le descrizioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="fac5f-203">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="fac5f-204">In questa attività si verificherà che il modello **StoreManager** **index** View visualizzi un elenco di album in base alla progettazione dei passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="fac5f-205">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-206">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-206">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-207">Modificare l'URL in **/StoreManager** per verificare che venga visualizzato un elenco di album, mostrando il **titolo**, l' **artista** e il **genere**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="fac5f-208">![Esplorazione dell'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Esplorazione dell'elenco di album")</span><span class="sxs-lookup"><span data-stu-id="fac5f-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="fac5f-209">*Esplorazione dell'elenco di album*</span><span class="sxs-lookup"><span data-stu-id="fac5f-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="fac5f-210">Esercizio 2: aggiunta di un helper HTML</span><span class="sxs-lookup"><span data-stu-id="fac5f-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="fac5f-211">La pagina di indice StoreManager presenta un potenziale problema: le proprietà title e nome artista possono avere una lunghezza sufficiente per eliminare la formattazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="fac5f-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="fac5f-212">In questo esercizio verrà illustrato come aggiungere un helper HTML personalizzato per troncare il testo.</span><span class="sxs-lookup"><span data-stu-id="fac5f-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="fac5f-213">Nella figura seguente è possibile vedere come viene modificato il formato a causa della lunghezza del testo quando si usa una piccola dimensione del browser.</span><span class="sxs-lookup"><span data-stu-id="fac5f-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="fac5f-214">![Esplorazione dell'elenco di album con testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Esplorazione dell'elenco di album con testo non troncato")</span><span class="sxs-lookup"><span data-stu-id="fac5f-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="fac5f-215">*Esplorazione dell'elenco di album con testo non troncato*</span><span class="sxs-lookup"><span data-stu-id="fac5f-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="fac5f-216">Attività 1: estensione dell'helper HTML</span><span class="sxs-lookup"><span data-stu-id="fac5f-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="fac5f-217">In questa attività verrà aggiunto un nuovo metodo **troncato** all'oggetto **HTML** esposto all'interno delle visualizzazioni MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fac5f-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="fac5f-218">A tale scopo, è necessario implementare un **metodo di estensione** per la classe **System. Web. Mvc. HtmlHelper** incorporata fornita da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fac5f-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="fac5f-219">Per altre informazioni sui **metodi di estensione**, vedere questo articolo di MSDN.</span><span class="sxs-lookup"><span data-stu-id="fac5f-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="fac5f-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="fac5f-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="fac5f-221">Aprire la soluzione **Begin** disponibile nella cartella **source/EX2-AddingAnHTMLHelper/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="fac5f-222">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-223">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-224">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-225">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-226">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-227">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-228">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-229">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-230">Aprire la visualizzazione dell'indice di StoreManager.</span><span class="sxs-lookup"><span data-stu-id="fac5f-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="fac5f-231">A tale scopo, nella Esplora soluzioni espandere la cartella **views** , quindi **StoreManager** e aprire il file **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="fac5f-232">Aggiungere il codice seguente sotto la direttiva <strong>@model</strong> per definire il metodo helper <strong>Truncate</strong> .</span><span class="sxs-lookup"><span data-stu-id="fac5f-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="fac5f-233">Attività 2: troncamento del testo nella pagina</span><span class="sxs-lookup"><span data-stu-id="fac5f-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="fac5f-234">In questa attività si userà il metodo **Truncate** per troncare il testo nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="fac5f-235">Aprire la visualizzazione dell'indice di StoreManager.</span><span class="sxs-lookup"><span data-stu-id="fac5f-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="fac5f-236">A tale scopo, nella Esplora soluzioni espandere la cartella **views** , quindi **StoreManager** e aprire il file **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="fac5f-237">Sostituire le righe che mostrano il **nome dell'artista** e il **titolo**dell'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="fac5f-238">A tale scopo, sostituire le righe seguenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="fac5f-239">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="fac5f-240">In questa attività si verificherà che il modello di visualizzazione dell' **Indice** **StoreManager** tronca il titolo e il nome dell'autore dell'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="fac5f-241">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-242">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-242">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-243">Modificare l'URL in **/StoreManager** per verificare che i testi lunghi nella colonna **titolo** e **artista** siano troncati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="fac5f-244">![Nomi di titoli e artisti troncati](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nomi di titoli e artisti troncati")</span><span class="sxs-lookup"><span data-stu-id="fac5f-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="fac5f-245">*Nomi di titoli e artisti troncati*</span><span class="sxs-lookup"><span data-stu-id="fac5f-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="fac5f-246">Esercizio 3: creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="fac5f-247">In questo esercizio verrà illustrato come creare un modulo per consentire ai gestori di negozi di modificare un album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="fac5f-248">Esplorano l'URL di **/StoreManager/Edit/ID** (**ID** che corrisponde all'ID univoco dell'album da modificare), effettuando così una chiamata HTTP-GET al server.</span><span class="sxs-lookup"><span data-stu-id="fac5f-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="fac5f-249">Il metodo di azione modifica controller consente di recuperare l'album appropriato dal database, creare un oggetto **StoreManagerViewModel** per incapsularlo (insieme a un elenco di artisti e generi) e quindi passarlo a un modello di visualizzazione per eseguire il rendering della pagina HTML all'utente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="fac5f-250">Questa pagina conterrà un elemento **&lt;form&gt;** con caselle di testo ed elenchi a discesa per la modifica delle proprietà dell'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="fac5f-251">Una volta che l'utente ha aggiornato i valori dei moduli di album e fa clic sul pulsante **Salva** , le modifiche vengono inviate tramite una chiamata http-post a **/StoreManager/Edit/ID**. Anche se l'URL rimane lo stesso dell'ultima chiamata, ASP.NET MVC identifica che questa volta è un POST HTTP e pertanto esegue un metodo di azione di modifica diverso (uno decorato con **[HttpPost]** ).</span><span class="sxs-lookup"><span data-stu-id="fac5f-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="fac5f-252">Attività 1: implementazione del metodo di azione HTTP-GET Edit</span><span class="sxs-lookup"><span data-stu-id="fac5f-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="fac5f-253">In questa attività verrà implementata la versione HTTP-GET del metodo di azione Edit per recuperare l'album appropriato dal database, oltre a un elenco di tutti i generi e artisti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="fac5f-254">I dati verranno inclusi nel pacchetto nell'oggetto **StoreManagerViewModel** definito nell'ultimo passaggio, che verrà quindi passato a un modello di vista per il rendering della risposta.</span><span class="sxs-lookup"><span data-stu-id="fac5f-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="fac5f-255">Aprire la soluzione **Begin** disponibile nella cartella **source/EX3-CreatingTheEditView/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="fac5f-256">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-257">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-258">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-259">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-260">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-261">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-262">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-263">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-264">Aprire la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="fac5f-265">A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="fac5f-266">Sostituire il metodo di azione **http-Get Edit** con il codice seguente per recuperare l' **album** appropriato, nonché gli elenchi **genres** e **Artists** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="fac5f-267">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX3 STOREMANAGERCONTROLLER http-Get Edit action*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-268">Si usa l'elenco di **selezione** **System. Web. Mvc** per gli artisti e i generi anziché l'elenco **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="fac5f-269">**Select** è un modo più pulito per popolare elenchi a discesa HTML e gestire elementi come la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="fac5f-270">La creazione di un'istanza e la successiva configurazione di questi oggetti ViewModel nell'azione del controller renderà più semplice lo scenario di modifica del modulo.</span><span class="sxs-lookup"><span data-stu-id="fac5f-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="fac5f-271">Attività 2: creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="fac5f-272">In questa attività verrà creato un modello di visualizzazione di modifica in cui verranno visualizzate le proprietà degli album in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="fac5f-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="fac5f-273">Creare la visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="fac5f-273">Create the Edit View.</span></span> <span data-ttu-id="fac5f-274">A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo **modifica** azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="fac5f-275">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **modifica**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="fac5f-276">Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa della **classe di dati View** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="fac5f-277">Selezionare **modifica** dall'elenco a discesa **modello di impalcatura** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="fac5f-278">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="fac5f-279">![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Aggiunta di una visualizzazione di modifica")</span><span class="sxs-lookup"><span data-stu-id="fac5f-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="fac5f-280">*Aggiunta di una visualizzazione di modifica*</span><span class="sxs-lookup"><span data-stu-id="fac5f-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="fac5f-281">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="fac5f-282">In questa attività si verificherà che nella pagina **StoreManager** **Edit** View (visualizzazione di modifica) vengono visualizzati i valori delle proprietà per l'album passato come parametro.</span><span class="sxs-lookup"><span data-stu-id="fac5f-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="fac5f-283">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-284">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-284">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-285">Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà per l'album passato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="fac5f-286">![Visualizzazione di modifica dell'album di esplorazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Visualizzazione di modifica dell'album di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="fac5f-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="fac5f-287">*Visualizzazione di modifica dell'album di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="fac5f-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="fac5f-288">Attività 4: implementazione degli elenchi a discesa nel modello di editor di album</span><span class="sxs-lookup"><span data-stu-id="fac5f-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="fac5f-289">In questa attività verranno aggiunti gli elenchi a discesa del modello di visualizzazione creato nell'ultima attività, in modo che l'utente possa effettuare una selezione da un elenco di artisti e generi.</span><span class="sxs-lookup"><span data-stu-id="fac5f-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="fac5f-290">Sostituire tutto il codice dei campi **album** con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac5f-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-291">È stato aggiunto un helper **HTML. DropDownList** per eseguire il rendering degli elenchi a discesa per la scelta degli artisti e dei generi.</span><span class="sxs-lookup"><span data-stu-id="fac5f-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="fac5f-292">I parametri passati a **HTML. DropDownList** sono:</span><span class="sxs-lookup"><span data-stu-id="fac5f-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="fac5f-293">Nome del campo del form ( **&quot;ArtistId&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="fac5f-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="fac5f-294">Elenco di valori **selezionati** per l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="fac5f-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="fac5f-295">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="fac5f-296">In questa attività si verificherà che nella pagina **StoreManager** **Edit** View (visualizzazione di modifica) vengono visualizzati gli elenchi a discesa anziché i campi di testo ID autore e genere.</span><span class="sxs-lookup"><span data-stu-id="fac5f-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="fac5f-297">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-298">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-298">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-299">Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati gli elenchi a discesa anziché i campi di testo ID autore e genere.</span><span class="sxs-lookup"><span data-stu-id="fac5f-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="fac5f-300">![Esplorazione della visualizzazione di modifica degli album con elenchi a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Esplorazione della visualizzazione di modifica degli album con elenchi a discesa")</span><span class="sxs-lookup"><span data-stu-id="fac5f-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="fac5f-301">*Visualizzazione di modifica dell'album, questa volta con elenchi a discesa*</span><span class="sxs-lookup"><span data-stu-id="fac5f-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="fac5f-302">Attività 6: implementazione del metodo di azione HTTP-POST-modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="fac5f-303">Ora che la visualizzazione di modifica viene visualizzata come previsto, è necessario implementare il metodo di azione HTTP-POST Edit per salvare le modifiche apportate all'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="fac5f-304">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fac5f-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fac5f-305">Aprire **StoreManagerController** dalla cartella **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="fac5f-306">Sostituire il codice del metodo di azione **http-post-modifica** con il codice seguente. si noti che il metodo che deve essere sostituito è una versione di overload che riceve due parametri:</span><span class="sxs-lookup"><span data-stu-id="fac5f-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="fac5f-307">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX3 STOREMANAGERCONTROLLER http-post Edit action*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-308">Questo metodo verrà eseguito quando l'utente fa clic sul pulsante **Salva** della visualizzazione ed esegue un post http dei valori del modulo sul server per renderli permanente nel database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="fac5f-309">L'elemento Decorator **[HttpPost]** indica che il metodo deve essere usato per gli scenari HTTP-post.</span><span class="sxs-lookup"><span data-stu-id="fac5f-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="fac5f-310">Il metodo accetta un oggetto **album** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-310">The method takes an **Album** object.</span></span> <span data-ttu-id="fac5f-311">ASP.NET MVC creerà automaticamente l'oggetto album dal modulo di &lt;inviato&gt; valori.</span><span class="sxs-lookup"><span data-stu-id="fac5f-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="fac5f-312">Il metodo eseguirà i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fac5f-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="fac5f-313">Se il modello è valido:</span><span class="sxs-lookup"><span data-stu-id="fac5f-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="fac5f-314">Aggiornare la voce di album nel contesto per contrassegnarla come oggetto modificato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="fac5f-315">Salvare le modifiche e reindirizzarle alla vista index.</span><span class="sxs-lookup"><span data-stu-id="fac5f-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="fac5f-316">Se il modello non è valido, il ViewBag verrà popolato con **GenreId** e **ArtistId**, quindi verrà restituita la visualizzazione con l'oggetto album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento necessario.</span><span class="sxs-lookup"><span data-stu-id="fac5f-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="fac5f-317">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="fac5f-318">In questa attività si verificherà che la pagina **StoreManager Edit** View salva effettivamente i dati degli album aggiornati nel database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="fac5f-319">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-320">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-320">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-321">Modificare l'URL in **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="fac5f-322">Modificare il titolo dell'album da **caricare** e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="fac5f-323">Verificare che il titolo dell'album sia stato effettivamente modificato nell'elenco di album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="fac5f-324">![Aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aggiornamento di un album")</span><span class="sxs-lookup"><span data-stu-id="fac5f-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="fac5f-325">*Aggiornamento di un album*</span><span class="sxs-lookup"><span data-stu-id="fac5f-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="fac5f-326">Esercizio 4: aggiunta di una visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="fac5f-327">Ora che **StoreManagerController** supporta la funzionalità di **modifica** , in questo esercizio verrà illustrato come aggiungere un modello di visualizzazione create per consentire ai responsabili dell'archiviazione di aggiungere nuovi album all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="fac5f-328">Come è stato fatto con la funzionalità di modifica, si implementerà lo scenario di creazione usando due metodi distinti all'interno della classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="fac5f-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="fac5f-329">Un metodo di azione visualizzerà un modulo vuoto quando i gestori dell'archivio visitano prima di tutto l'URL **/StoreManager/create** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="fac5f-330">Un secondo metodo di azione gestirà lo scenario in cui il gestore dell'archivio fa clic sul pulsante **Salva** nel form e li invia nuovamente all'URL **/StoreManager/create** come http-post.</span><span class="sxs-lookup"><span data-stu-id="fac5f-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="fac5f-331">Attività 1: implementazione del metodo di azione HTTP-GET create</span><span class="sxs-lookup"><span data-stu-id="fac5f-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="fac5f-332">In questa attività verrà implementata la versione HTTP-GET del metodo di azione create per recuperare un elenco di tutti i generi e artisti, creare un pacchetto di questi dati in un oggetto **StoreManagerViewModel** , che verrà quindi passato a un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="fac5f-333">Aprire la soluzione **Begin** disponibile nella cartella **source/EX4-AddingACreateView/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="fac5f-334">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-335">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-336">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-337">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-338">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-339">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-340">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-341">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-342">Aprire la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="fac5f-343">A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="fac5f-344">Sostituire il codice del metodo **create** Action con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac5f-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="fac5f-345">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX4 STOREMANAGERCONTROLLER http-Get create Action*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="fac5f-346">Attività 2: aggiunta della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="fac5f-347">In questa attività verrà aggiunto il modello crea vista che visualizzerà un nuovo modulo di album (vuoto).</span><span class="sxs-lookup"><span data-stu-id="fac5f-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="fac5f-348">Fare clic con il pulsante destro del mouse all'interno del metodo **Crea** azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="fac5f-349">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="fac5f-350">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **create**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="fac5f-351">Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** e **creare** dall'elenco a discesa **modello di impalcatura** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="fac5f-352">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="fac5f-353">![Aggiunta di una visualizzazione di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")</span><span class="sxs-lookup"><span data-stu-id="fac5f-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="fac5f-354">*Aggiunta della visualizzazione di creazione*</span><span class="sxs-lookup"><span data-stu-id="fac5f-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="fac5f-355">Aggiornare i campi **GenreId** e **ArtistId** in modo da usare un elenco a discesa, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="fac5f-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="fac5f-356">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="fac5f-357">In questa attività si verificherà che nella pagina **StoreManager** **create** View venga visualizzato un modulo album vuoto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="fac5f-358">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-359">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-359">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-360">Modificare l'URL in **/StoreManager/create**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="fac5f-361">Verificare che venga visualizzato un modulo vuoto per riempire le proprietà del nuovo album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="fac5f-362">![Crea vista con un modulo vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Crea vista con un modulo vuoto")</span><span class="sxs-lookup"><span data-stu-id="fac5f-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="fac5f-363">*Crea vista con un modulo vuoto*</span><span class="sxs-lookup"><span data-stu-id="fac5f-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="fac5f-364">Attività 4: implementazione del metodo di azione HTTP-POST-creazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="fac5f-365">In questa attività verrà implementata la versione HTTP-POST del metodo di azione create che verrà richiamata quando un utente fa clic sul pulsante **Salva** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="fac5f-366">Il metodo deve salvare il nuovo album nel database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="fac5f-367">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fac5f-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fac5f-368">Aprire la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="fac5f-369">A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="fac5f-370">Sostituire il codice del metodo di azione **http-post create** con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac5f-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="fac5f-371">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-EX4 STOREMANAGERCONTROLLER http-post create Action*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-372">L'azione di creazione è piuttosto simile al metodo di azione di modifica precedente, ma anziché impostare l'oggetto come modificato, viene aggiunto al contesto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="fac5f-373">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="fac5f-374">In questa attività si verificherà che la pagina di creazione della visualizzazione **StoreManager** consente di creare un nuovo album e quindi di eseguire il reindirizzamento alla visualizzazione dell'indice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="fac5f-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="fac5f-375">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-376">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-376">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-377">Modificare l'URL in **/StoreManager/create**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="fac5f-378">Compilare tutti i campi del modulo con i dati per un nuovo album, come quello illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="fac5f-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="fac5f-379">![Creazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creazione di un album")</span><span class="sxs-lookup"><span data-stu-id="fac5f-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="fac5f-380">*Creazione di un album*</span><span class="sxs-lookup"><span data-stu-id="fac5f-380">*Creating an Album*</span></span>
3. <span data-ttu-id="fac5f-381">Verificare di essere reindirizzati alla vista StoreManager index che include il nuovo album appena creato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="fac5f-382">![Nuovo album creato](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo album creato")</span><span class="sxs-lookup"><span data-stu-id="fac5f-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="fac5f-383">*Nuovo album creato*</span><span class="sxs-lookup"><span data-stu-id="fac5f-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="fac5f-384">Esercizio 5: gestione dell'eliminazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="fac5f-385">La possibilità di eliminare album non è ancora implementata.</span><span class="sxs-lookup"><span data-stu-id="fac5f-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="fac5f-386">Si tratta di questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="fac5f-386">This is what this exercise will be about.</span></span> <span data-ttu-id="fac5f-387">Come prima, si implementerà lo scenario Delete usando due metodi distinti all'interno della classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="fac5f-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="fac5f-388">Un metodo di azione visualizzerà un modulo di conferma</span><span class="sxs-lookup"><span data-stu-id="fac5f-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="fac5f-389">Un secondo metodo di azione gestirà l'invio del form</span><span class="sxs-lookup"><span data-stu-id="fac5f-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="fac5f-390">Attività 1: implementazione del metodo di azione HTTP-GET Delete</span><span class="sxs-lookup"><span data-stu-id="fac5f-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="fac5f-391">In questa attività verrà implementata la versione HTTP-GET del metodo di azione Delete per recuperare le informazioni sull'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="fac5f-392">Aprire la soluzione **Begin** disponibile nella cartella **source/eX5-HandlingDeletion/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="fac5f-393">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-394">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-395">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-396">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-397">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-398">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-399">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-400">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-401">Aprire la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="fac5f-402">A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="fac5f-403">L'azione di eliminazione del controller è esattamente identica all'azione del controller dei dettagli dell'archivio precedente: esegue una query sull'oggetto **album** dal database usando l' **ID** specificato nell'URL e restituisce la **visualizzazione**appropriata.</span><span class="sxs-lookup"><span data-stu-id="fac5f-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="fac5f-404">A tale scopo, sostituire il codice del metodo di azione HTTP-GET **Delete** con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac5f-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="fac5f-405">(Frammento di codice- *ASP.NET MVC 4 Helper e Forms and Validation-eX5 gestione dell'eliminazione http-Get Delete azione*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="fac5f-406">Fare clic con il pulsante destro del mouse all'interno del metodo di azione **Delete** e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="fac5f-407">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="fac5f-408">Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Delete**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="fac5f-409">Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** e selezionare **album (MvcMusicStore. Models)** dall'elenco a discesa **classe modello** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="fac5f-410">Selezionare **Elimina** dall'elenco a discesa **modello di impalcatura** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="fac5f-411">Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="fac5f-412">![Aggiunta di una visualizzazione Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Aggiunta di una visualizzazione Delete")</span><span class="sxs-lookup"><span data-stu-id="fac5f-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="fac5f-413">*Aggiunta di una visualizzazione Delete*</span><span class="sxs-lookup"><span data-stu-id="fac5f-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="fac5f-414">Il modello Delete Mostra tutti i campi del modello.</span><span class="sxs-lookup"><span data-stu-id="fac5f-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="fac5f-415">Viene visualizzato solo il titolo dell'album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-415">You will show only the album's title.</span></span> <span data-ttu-id="fac5f-416">A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fac5f-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="fac5f-417">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="fac5f-418">In questa attività si verificherà che nella pagina **StoreManager** **Delete** View venga visualizzato un modulo di eliminazione della conferma.</span><span class="sxs-lookup"><span data-stu-id="fac5f-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="fac5f-419">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-420">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-420">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-421">Modificare l'URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="fac5f-422">Selezionare un album da eliminare facendo clic su **Elimina** e verificare che la nuova vista sia stata caricata.</span><span class="sxs-lookup"><span data-stu-id="fac5f-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="fac5f-423">![Eliminazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Eliminazione di un album")</span><span class="sxs-lookup"><span data-stu-id="fac5f-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="fac5f-424">*Eliminazione di un album*</span><span class="sxs-lookup"><span data-stu-id="fac5f-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="fac5f-425">Attività 3: implementazione del metodo di azione HTTP-POST Delete</span><span class="sxs-lookup"><span data-stu-id="fac5f-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="fac5f-426">In questa attività verrà implementata la versione HTTP-POST del metodo di azione DELETE che verrà richiamato quando un utente fa clic sul pulsante **Elimina** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="fac5f-427">Il metodo deve eliminare l'album nel database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="fac5f-428">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fac5f-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fac5f-429">Aprire la classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="fac5f-430">A tale scopo, espandere la cartella **Controllers** e fare doppio clic su **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="fac5f-431">Sostituire il codice del metodo di azione **http-post Delete** con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac5f-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="fac5f-432">(Frammento di codice- *ASP.NET MVC 4 Helper e form e convalida-eX5 gestione dell'eliminazione http-post Delete*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="fac5f-433">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="fac5f-434">In questa attività si verificherà che la pagina di visualizzazione **eliminazione di StoreManager** consente di eliminare un album e quindi di eseguire il reindirizzamento alla visualizzazione dell'indice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="fac5f-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="fac5f-435">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-436">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-436">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-437">Modificare l'URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="fac5f-438">Selezionare un album da eliminare facendo clic su **Elimina.**</span><span class="sxs-lookup"><span data-stu-id="fac5f-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="fac5f-439">Confermare l'eliminazione facendo clic sul pulsante **Elimina** :</span><span class="sxs-lookup"><span data-stu-id="fac5f-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="fac5f-440">![Eliminazione di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Eliminazione di un album")</span><span class="sxs-lookup"><span data-stu-id="fac5f-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="fac5f-441">*Eliminazione di un album*</span><span class="sxs-lookup"><span data-stu-id="fac5f-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="fac5f-442">Verificare che l'album sia stato eliminato perché non è presente nella pagina di **Indice** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="fac5f-443">Esercizio 6: aggiunta della convalida</span><span class="sxs-lookup"><span data-stu-id="fac5f-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="fac5f-444">Attualmente, i moduli di creazione e modifica non eseguono alcun tipo di convalida.</span><span class="sxs-lookup"><span data-stu-id="fac5f-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="fac5f-445">Se l'utente lascia un campo obbligatorio o digita lettere nel campo Price, il primo errore che si otterrà sarà dal database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="fac5f-446">Per aggiungere la convalida all'applicazione, è possibile aggiungere annotazioni dei dati alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="fac5f-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="fac5f-447">Le annotazioni dei dati consentono di descrivere le regole che si desidera applicare alle proprietà del modello. ASP.NET MVC si occuperà di applicare e visualizzare il messaggio appropriato agli utenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="fac5f-448">Attività 1-aggiunta di annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="fac5f-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="fac5f-449">In questa attività verranno aggiunte le annotazioni dei dati al modello di album che renderà la pagina di creazione e modifica in cui verranno visualizzati i messaggi di convalida quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="fac5f-450">Per una classe di modello semplice, l'aggiunta di un'annotazione dei dati viene gestita semplicemente aggiungendo un'istruzione **using** per **System. ComponentModel. DataAnnotation**, quindi inserendo un attributo **[Required]** sulle proprietà appropriate.</span><span class="sxs-lookup"><span data-stu-id="fac5f-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="fac5f-451">Nell'esempio seguente la proprietà **Name** è un campo obbligatorio nella vista.</span><span class="sxs-lookup"><span data-stu-id="fac5f-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="fac5f-452">Si tratta di un po' più complesso in casi come questa applicazione in cui viene generata la Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="fac5f-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="fac5f-453">Se le annotazioni dei dati sono state aggiunte direttamente alle classi del modello, verranno sovrascritte se si aggiorna il modello dal database.</span><span class="sxs-lookup"><span data-stu-id="fac5f-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="fac5f-454">In alternativa, è possibile utilizzare le classi parziali dei metadati che saranno disponibili per conservare le annotazioni e associate alle classi del modello utilizzando l'attributo **[MetadataType]** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="fac5f-455">Aprire la soluzione **Begin** disponibile nella cartella **source/Ex6-AddingValidation/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="fac5f-456">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-457">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-458">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-459">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-460">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-461">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-462">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-463">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-464">Aprire **album.cs** dalla cartella **Models** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="fac5f-465">Sostituire il contenuto di **album.cs** con il codice evidenziato, in modo che abbia un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fac5f-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="fac5f-466">La riga **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica che le stringhe vuote del modello non verranno convertite in null quando il campo dati viene aggiornato nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="fac5f-467">Questa impostazione consente di evitare un'eccezione quando il Entity Framework assegna valori null al modello prima che i campi vengano convalidati dall'annotazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="fac5f-468">(Frammento di codice- *ASP.NET MVC 4 Helper e form e convalida-classe parziale dei metadati degli album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="fac5f-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-469">Questa classe parziale dell' **album** ha un attributo **MetadataType** che punta alla classe **AlbumMetaData** per le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="fac5f-470">Di seguito sono indicati alcuni degli attributi di annotazione dei dati utilizzati per aggiungere annotazioni al modello di album:</span><span class="sxs-lookup"><span data-stu-id="fac5f-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="fac5f-471">Required: indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="fac5f-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="fac5f-472">DisplayName-definisce il testo da usare nei campi del form e nei messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="fac5f-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="fac5f-473">DisplayFormat: specifica la modalità di visualizzazione e formattazione dei campi dati.</span><span class="sxs-lookup"><span data-stu-id="fac5f-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="fac5f-474">StringLength-definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="fac5f-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="fac5f-475">Range: fornisce un valore massimo e minimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="fac5f-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="fac5f-476">ScaffoldColumn-consente di nascondere i campi dai form dell'editor</span><span class="sxs-lookup"><span data-stu-id="fac5f-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="fac5f-477">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="fac5f-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="fac5f-478">In questa attività si verificherà che le pagine Crea e modifica convalideranno i campi, usando i nomi visualizzati scelti nell'ultima attività.</span><span class="sxs-lookup"><span data-stu-id="fac5f-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="fac5f-479">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fac5f-480">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-480">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-481">Modificare l'URL in **/StoreManager/create**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="fac5f-482">Verificare che i nomi visualizzati corrispondano a quelli della classe parziale, ad esempio l' **URL dell'immagine dell'album** anziché **AlbumArtUrl**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="fac5f-483">Fare clic su **Crea**senza compilare il modulo.</span><span class="sxs-lookup"><span data-stu-id="fac5f-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="fac5f-484">Verificare di ottenere i messaggi di convalida corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="fac5f-485">![Campi convalidati nella pagina Crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campi convalidati nella pagina Crea")</span><span class="sxs-lookup"><span data-stu-id="fac5f-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="fac5f-486">*Campi convalidati nella pagina Crea*</span><span class="sxs-lookup"><span data-stu-id="fac5f-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="fac5f-487">È possibile verificare che lo stesso avvenga con la pagina di **modifica** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="fac5f-488">Modificare l'URL in **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli della classe parziale (ad esempio, l' **URL dell'immagine dell'album** anziché **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="fac5f-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="fac5f-489">Svuotare i campi **title** e **Price** , quindi fare clic su **Save**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="fac5f-490">Verificare di ottenere i messaggi di convalida corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-490">Verify that you get the corresponding validation messages.</span></span>

    ![Campi convalidati nella pagina modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="fac5f-492">*Campi convalidati nella pagina modifica*</span><span class="sxs-lookup"><span data-stu-id="fac5f-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="fac5f-493">Esercizio 7: uso di jQuery non intrusivo sul lato client</span><span class="sxs-lookup"><span data-stu-id="fac5f-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="fac5f-494">In questo esercizio verrà illustrato come abilitare MVC 4 la convalida jQuery non intrusiva sul lato client.</span><span class="sxs-lookup"><span data-stu-id="fac5f-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="fac5f-495">Il jQuery non intrusivo usa il prefisso JavaScript di data-Ajax per richiamare i metodi di azione sul server piuttosto che per la creazione intrusiva di script client inline.</span><span class="sxs-lookup"><span data-stu-id="fac5f-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="fac5f-496">Attività 1: esecuzione dell'applicazione prima di abilitare jQuery non intrusivo</span><span class="sxs-lookup"><span data-stu-id="fac5f-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="fac5f-497">In questa attività, l'applicazione verrà eseguita prima dell'inclusione di jQuery per confrontare entrambi i modelli di convalida.</span><span class="sxs-lookup"><span data-stu-id="fac5f-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="fac5f-498">Aprire la soluzione **Begin** disponibile nella cartella **source/EX7-UnobtrusivejQueryValidation/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="fac5f-499">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="fac5f-500">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fac5f-501">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fac5f-502">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fac5f-503">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fac5f-504">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fac5f-505">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fac5f-506">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="fac5f-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="fac5f-507">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="fac5f-508">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-508">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-509">Sfogliare **/StoreManager/create** e fare clic su **Crea** senza compilare il modulo per verificare che vengano visualizzati i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="fac5f-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="fac5f-510">![Convalida client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Convalida client disabilitata")</span><span class="sxs-lookup"><span data-stu-id="fac5f-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="fac5f-511">*Convalida client disabilitata*</span><span class="sxs-lookup"><span data-stu-id="fac5f-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="fac5f-512">Nel browser aprire il codice sorgente HTML:</span><span class="sxs-lookup"><span data-stu-id="fac5f-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="fac5f-513">Attività 2: abilitazione della convalida client non intrusiva</span><span class="sxs-lookup"><span data-stu-id="fac5f-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="fac5f-514">In questa attività verrà abilitata la **convalida del client jQuery unobtrusive** dal file **Web. config** , che per impostazione predefinita è impostata su false in tutti i nuovi progetti ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fac5f-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="fac5f-515">Inoltre, si aggiungeranno i riferimenti agli script necessari per rendere il lavoro di convalida del client non intrusivo di jQuery.</span><span class="sxs-lookup"><span data-stu-id="fac5f-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="fac5f-516">Aprire il file **Web. config** nella radice del progetto e assicurarsi che i valori delle chiavi **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** siano impostati su **true**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-517">È anche possibile abilitare la convalida client dal codice in Global.asax.cs per ottenere gli stessi risultati:</span><span class="sxs-lookup"><span data-stu-id="fac5f-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="fac5f-518">**HtmlHelper. ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="fac5f-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="fac5f-519">Inoltre, è possibile assegnare l'attributo ClientValidationEnabled a qualsiasi controller per avere un comportamento personalizzato.</span><span class="sxs-lookup"><span data-stu-id="fac5f-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="fac5f-520">Aprire **create. cshtml** in **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="fac5f-521">Assicurarsi che i file di script seguenti, **jQuery. Validate** e **jQuery. Validate. unintrusivo**, siano presenti nella vista come riferimento tramite il bundle &quot; **~/Bundles/jqueryval**&quot;.</span><span class="sxs-lookup"><span data-stu-id="fac5f-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="fac5f-522">Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fac5f-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="fac5f-523">È possibile trovare altre librerie nella cartella **/Scripts** del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac5f-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="fac5f-524">Per consentire il funzionamento delle librerie di convalida, è necessario aggiungere un riferimento alla libreria del framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="fac5f-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="fac5f-525">Poiché questo riferimento è già stato aggiunto nel file **\_layout. cshtml** , non è necessario aggiungerlo in questa visualizzazione particolare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="fac5f-526">Attività 3: esecuzione dell'applicazione con la convalida jQuery non intrusiva</span><span class="sxs-lookup"><span data-stu-id="fac5f-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="fac5f-527">In questa attività si verificherà che il modello **StoreManager** Create View esegua la convalida lato client usando le librerie jQuery quando l'utente crea un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="fac5f-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="fac5f-528">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="fac5f-529">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="fac5f-529">The project starts in the Home page.</span></span> <span data-ttu-id="fac5f-530">Sfogliare **/StoreManager/create** e fare clic su **Crea** senza compilare il modulo per verificare che vengano visualizzati i messaggi di convalida:</span><span class="sxs-lookup"><span data-stu-id="fac5f-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="fac5f-531">![Convalida client con jQuery abilitato](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Convalida client con jQuery abilitato")</span><span class="sxs-lookup"><span data-stu-id="fac5f-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="fac5f-532">*Convalida client con jQuery abilitato*</span><span class="sxs-lookup"><span data-stu-id="fac5f-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="fac5f-533">Nel browser aprire il codice sorgente per Crea vista:</span><span class="sxs-lookup"><span data-stu-id="fac5f-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="fac5f-534">Per ogni regola di convalida del client, jQuery non intrusivo aggiunge un attributo con data-Val-*rulename*=&quot;&quot;*messaggio* .</span><span class="sxs-lookup"><span data-stu-id="fac5f-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="fac5f-535">Di seguito è riportato un elenco di tag che vengono inseriti da jQuery non intrusivo nel campo di input HTML per eseguire la convalida del client:</span><span class="sxs-lookup"><span data-stu-id="fac5f-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="fac5f-536">Dati-Val</span><span class="sxs-lookup"><span data-stu-id="fac5f-536">Data-val</span></span>
   > - <span data-ttu-id="fac5f-537">Data-Val-Number</span><span class="sxs-lookup"><span data-stu-id="fac5f-537">Data-val-number</span></span>
   > - <span data-ttu-id="fac5f-538">Data-intervallo Val</span><span class="sxs-lookup"><span data-stu-id="fac5f-538">Data-val-range</span></span>
   > - <span data-ttu-id="fac5f-539">Data-Val-range-min/Data-Val-Range-max</span><span class="sxs-lookup"><span data-stu-id="fac5f-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="fac5f-540">Dati-Val-obbligatorio</span><span class="sxs-lookup"><span data-stu-id="fac5f-540">Data-val-required</span></span>
   > - <span data-ttu-id="fac5f-541">Data-lunghezza di Val</span><span class="sxs-lookup"><span data-stu-id="fac5f-541">Data-val-length</span></span>
   > - <span data-ttu-id="fac5f-542">Data-Val-Length-Max/data-Val-length-min</span><span class="sxs-lookup"><span data-stu-id="fac5f-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="fac5f-543">Tutti i valori dei dati vengono riempiti con l' **annotazione dei dati**del modello.</span><span class="sxs-lookup"><span data-stu-id="fac5f-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="fac5f-544">Quindi, tutta la logica che funziona sul lato server può essere eseguita sul lato client.</span><span class="sxs-lookup"><span data-stu-id="fac5f-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="fac5f-545">Nell'attributo Price, ad esempio, è presente l'annotazione dei dati seguente nel modello:</span><span class="sxs-lookup"><span data-stu-id="fac5f-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="fac5f-546">Dopo l'uso di jQuery non intrusivo, il codice generato è:</span><span class="sxs-lookup"><span data-stu-id="fac5f-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="fac5f-547">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="fac5f-547">Summary</span></span>

<span data-ttu-id="fac5f-548">Completando questa esercitazione pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="fac5f-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="fac5f-549">Azioni del controller come index, create, Edit, Delete</span><span class="sxs-lookup"><span data-stu-id="fac5f-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="fac5f-550">Funzionalità di ponteggi di ASP.NET MVC per la visualizzazione delle proprietà in una tabella HTML</span><span class="sxs-lookup"><span data-stu-id="fac5f-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="fac5f-551">Helper HTML personalizzati per migliorare l'esperienza utente</span><span class="sxs-lookup"><span data-stu-id="fac5f-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="fac5f-552">Metodi di azione che reagiscono a chiamate HTTP-GET o HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="fac5f-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="fac5f-553">Modello di editor condiviso per modelli di visualizzazione simili, ad esempio creazione e modifica</span><span class="sxs-lookup"><span data-stu-id="fac5f-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="fac5f-554">Elementi del form come gli elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="fac5f-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="fac5f-555">Annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="fac5f-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="fac5f-556">Convalida lato client tramite la libreria non intrusiva jQuery</span><span class="sxs-lookup"><span data-stu-id="fac5f-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="fac5f-557">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="fac5f-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="fac5f-558">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="fac5f-559">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="fac5f-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="fac5f-560">Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="fac5f-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="fac5f-561">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="fac5f-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="fac5f-562">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-562">Click on **Install Now**.</span></span> <span data-ttu-id="fac5f-563">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="fac5f-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="fac5f-564">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="fac5f-565">![Installa Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="fac5f-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="fac5f-566">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="fac5f-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="fac5f-567">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="fac5f-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="fac5f-569">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="fac5f-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="fac5f-570">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="fac5f-570">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="fac5f-572">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="fac5f-572">*Installation progress*</span></span>
6. <span data-ttu-id="fac5f-573">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-573">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="fac5f-575">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="fac5f-575">*Installation completed*</span></span>
7. <span data-ttu-id="fac5f-576">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="fac5f-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="fac5f-577">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="fac5f-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="fac5f-579">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="fac5f-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="fac5f-580">Appendice B: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="fac5f-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="fac5f-581">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="fac5f-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="fac5f-582">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="fac5f-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="fac5f-583">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="fac5f-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="fac5f-584">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="fac5f-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="fac5f-585">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="fac5f-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="fac5f-586">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="fac5f-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="fac5f-587">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="fac5f-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="fac5f-588">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="fac5f-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="fac5f-589">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="fac5f-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="fac5f-590">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="fac5f-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="fac5f-591">![Inizia a digitare il nome del frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="fac5f-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="fac5f-592">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="fac5f-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="fac5f-593">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="fac5f-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="fac5f-594">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="fac5f-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="fac5f-595">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="fac5f-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="fac5f-596">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="fac5f-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="fac5f-597">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="fac5f-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="fac5f-598">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="fac5f-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="fac5f-599">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="fac5f-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="fac5f-600">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="fac5f-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="fac5f-601">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="fac5f-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="fac5f-602">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="fac5f-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="fac5f-603">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="fac5f-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="fac5f-604">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="fac5f-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
