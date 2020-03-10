---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework ponteggi e migrazioni | Microsoft Docs
author: rick-anderson
description: Se si ha familiarità con i metodi del controller ASP.NET MVC 4 o sono stati completati i &quot;helper, i moduli e la convalida&quot; Lab pratico, è necessario tenere presente...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598903"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="870c3-103">Scaffolding e migrazioni di ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="870c3-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="870c3-104">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="870c3-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="870c3-105">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="870c3-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="870c3-106">Se si ha familiarità con i metodi del controller ASP.NET MVC 4 o sono stati completati i &quot;helper, i moduli e la convalida&quot; Lab pratico, è necessario tenere presente che molti della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="870c3-107">Se il modello dispone di diverse classi da modificare, sarà probabilmente necessario dedicare molto tempo alla scrittura dei metodi di azione POST e GET per ogni operazione di entità, nonché di ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="870c3-108">In questo laboratorio si apprenderà come usare l'impalcatura ASP.NET MVC 4 per generare automaticamente la linea di base dell'applicazione CRUD (creazione, lettura, aggiornamento ed eliminazione).</span><span class="sxs-lookup"><span data-stu-id="870c3-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="870c3-109">Partendo da una semplice classe di modelli, e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, oltre a tutte le visualizzazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="870c3-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="870c3-110">Dopo aver compilato ed eseguito la soluzione semplice, il database dell'applicazione sarà generato insieme alla logica e alle visualizzazioni MVC per la manipolazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="870c3-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="870c3-111">Inoltre, si apprenderà come è facile usare Entity Framework migrazioni per eseguire gli aggiornamenti dei modelli nell'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="870c3-112">Entity Framework migrazioni consente di modificare il database dopo che il modello è stato modificato con semplici passaggi.</span><span class="sxs-lookup"><span data-stu-id="870c3-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="870c3-113">Tenendo conto di queste considerazioni, sarà possibile creare e gestire applicazioni Web in modo più efficiente, sfruttando le funzionalità più recenti di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="870c3-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="870c3-114">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camp, disponibile in alle [versioni di Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="870c3-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="870c3-115">Il progetto specifico di questo Lab è disponibile in [ASP.NET MVC 4 Entity Framework impalcature e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="870c3-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="870c3-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="870c3-116">Objectives</span></span>

<span data-ttu-id="870c3-117">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="870c3-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="870c3-118">Usare l'impalcatura ASP.NET per le operazioni CRUD nei controller.</span><span class="sxs-lookup"><span data-stu-id="870c3-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="870c3-119">Modificare il modello di database utilizzando migrazioni Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="870c3-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="870c3-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="870c3-120">Prerequisites</span></span>

<span data-ttu-id="870c3-121">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="870c3-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="870c3-122">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="870c3-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="870c3-123">Configurazione</span><span class="sxs-lookup"><span data-stu-id="870c3-123">Setup</span></span>

<span data-ttu-id="870c3-124">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="870c3-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="870c3-125">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="870c3-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="870c3-126">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="870c3-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="870c3-127">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice B: uso dei frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="870c3-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="870c3-128">Esercizi</span><span class="sxs-lookup"><span data-stu-id="870c3-128">Exercises</span></span>

<span data-ttu-id="870c3-129">Il seguente esercizio costituisce il laboratorio pratico:</span><span class="sxs-lookup"><span data-stu-id="870c3-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="870c3-130">Uso di ponteggi ASP.NET MVC 4 con migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="870c3-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="870c3-131">Questo esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="870c3-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="870c3-132">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="870c3-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="870c3-133">Tempo stimato per il completamento del Lab: **30 minuti**</span><span class="sxs-lookup"><span data-stu-id="870c3-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="870c3-134">Esercizio 1: uso di ponteggi ASP.NET MVC 4 con migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="870c3-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="870c3-135">L'impalcatura MVC ASP.NET offre un modo rapido per generare le operazioni CRUD in modo standardizzato, creando la logica necessaria che consente all'applicazione di interagire con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="870c3-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="870c3-136">In questo esercizio si apprenderà come usare l'impalcatura ASP.NET MVC 4 con Code First per creare i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="870c3-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="870c3-137">Si apprenderà quindi come aggiornare il modello applicando le modifiche nel database usando Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="870c3-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="870c3-138">Attività 1: creazione di un nuovo progetto ASP.NET MVC 4 mediante l'impalcatura</span><span class="sxs-lookup"><span data-stu-id="870c3-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="870c3-139">Se non è già aperto, avviare **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="870c3-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="870c3-140">Selezionare **file | Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="870c3-140">Select **File | New Project**.</span></span> <span data-ttu-id="870c3-141">Nella finestra di dialogo nuovo progetto, sotto l'oggetto **visivo C# | Sezione Web** selezionare **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="870c3-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="870c3-142">Assegnare al progetto il nome **MVC4andEFMigrations** e impostare il percorso sulla cartella **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** di questo Lab.</span><span class="sxs-lookup"><span data-stu-id="870c3-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="870c3-143">Impostare il **nome della soluzione** su **Begin** e verificare che l'opzione **Crea directory per soluzione** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="870c3-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="870c3-144">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="870c3-144">Click **OK**.</span></span>

    <span data-ttu-id="870c3-145">![Finestra di dialogo nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Finestra di dialogo nuovo progetto MVC 4 ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="870c3-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="870c3-146">*Finestra di dialogo nuovo progetto MVC 4 ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="870c3-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="870c3-147">Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello **applicazione Internet** e assicurarsi che **Razor** sia il **motore di visualizzazione**selezionato.</span><span class="sxs-lookup"><span data-stu-id="870c3-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="870c3-148">Fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="870c3-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="870c3-149">![Nuova applicazione Internet MVC 4 ASP.NET](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nuova applicazione Internet MVC 4 ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="870c3-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="870c3-150">*Nuova applicazione Internet MVC 4 ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="870c3-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="870c3-151">Nella Esplora soluzioni fare clic con il pulsante destro del mouse su **modelli** e scegliere **Aggiungi | Classe** per creare una semplice classe Person (poco).</span><span class="sxs-lookup"><span data-stu-id="870c3-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="870c3-152">Assegnare un nome all' **utente** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="870c3-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="870c3-153">Aprire la classe Person e inserire le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="870c3-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="870c3-154">(Frammento di codice: *ASP.NET MVC 4 e Entity Framework migrazioni-proprietà della persona EX1*)</span><span class="sxs-lookup"><span data-stu-id="870c3-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="870c3-155">Fare clic su **Compila | Compilare la soluzione** per salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="870c3-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="870c3-156">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="870c3-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="870c3-157">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="870c3-157">*Building the Application*</span></span>
7. <span data-ttu-id="870c3-158">Nella Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi | Controller**.</span><span class="sxs-lookup"><span data-stu-id="870c3-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="870c3-159">Denominare il controller *PersonController* e completare le **Opzioni di ponteggi** con i valori seguenti.</span><span class="sxs-lookup"><span data-stu-id="870c3-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="870c3-160">Nell'elenco a discesa **modello** selezionare il **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="870c3-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="870c3-161">Nell'elenco a discesa **classe modello** selezionare la classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="870c3-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="870c3-162">Nell'elenco **classe contesto dati** selezionare **&lt;nuovo contesto dati...&gt;** .</span><span class="sxs-lookup"><span data-stu-id="870c3-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="870c3-163">Scegliere un nome qualsiasi e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="870c3-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="870c3-164">Nell'elenco a discesa **views** verificare che **Razor** sia selezionato.</span><span class="sxs-lookup"><span data-stu-id="870c3-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="870c3-165">![Aggiunta del controller Person con impalcature](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Aggiunta del controller Person con impalcature")</span><span class="sxs-lookup"><span data-stu-id="870c3-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="870c3-166">*Aggiunta del controller Person con impalcature*</span><span class="sxs-lookup"><span data-stu-id="870c3-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="870c3-167">Fare clic su **Aggiungi** per creare il nuovo controller per la persona con ponteggi.</span><span class="sxs-lookup"><span data-stu-id="870c3-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="870c3-168">A questo punto sono state generate le azioni del controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="870c3-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="870c3-169">![Dopo aver creato il controller Person con impalcature](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Dopo aver creato il controller Person con impalcature")</span><span class="sxs-lookup"><span data-stu-id="870c3-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="870c3-170">*Dopo aver creato il controller Person con impalcature*</span><span class="sxs-lookup"><span data-stu-id="870c3-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="870c3-171">Aprire la classe **PersonController** .</span><span class="sxs-lookup"><span data-stu-id="870c3-171">Open **PersonController** class.</span></span> <span data-ttu-id="870c3-172">Si noti che i metodi di azione CRUD completi sono stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="870c3-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="870c3-173">![All'interno del controller person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "All'interno del controller person")</span><span class="sxs-lookup"><span data-stu-id="870c3-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="870c3-174">*All'interno del controller person*</span><span class="sxs-lookup"><span data-stu-id="870c3-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="870c3-175">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="870c3-175">Task 2- Running the application</span></span>

<span data-ttu-id="870c3-176">A questo punto, il database non è ancora stato creato.</span><span class="sxs-lookup"><span data-stu-id="870c3-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="870c3-177">In questa attività si eseguirà l'applicazione per la prima volta e si verificheranno le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="870c3-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="870c3-178">Il database verrà creato in tempo reale con Code First.</span><span class="sxs-lookup"><span data-stu-id="870c3-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="870c3-179">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="870c3-180">Nel browser aggiungere **/persona** all'URL per aprire la pagina person.</span><span class="sxs-lookup"><span data-stu-id="870c3-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="870c3-181">![Prima esecuzione applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Prima esecuzione applicazione")</span><span class="sxs-lookup"><span data-stu-id="870c3-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="870c3-182">*Applicazione: prima esecuzione*</span><span class="sxs-lookup"><span data-stu-id="870c3-182">*Application: first run*</span></span>
3. <span data-ttu-id="870c3-183">Verranno ora Esplorate le pagine Person e verranno testate le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="870c3-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="870c3-184">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="870c3-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="870c3-185">Immettere il nome e il cognome e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="870c3-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="870c3-186">![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Aggiunta di una nuova persona")</span><span class="sxs-lookup"><span data-stu-id="870c3-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="870c3-187">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="870c3-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="870c3-188">Nell'elenco della persona è possibile eliminare, modificare o aggiungere elementi.</span><span class="sxs-lookup"><span data-stu-id="870c3-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="870c3-189">![elenco utenti](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco utenti")</span><span class="sxs-lookup"><span data-stu-id="870c3-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="870c3-190">*Elenco utenti*</span><span class="sxs-lookup"><span data-stu-id="870c3-190">*Person list*</span></span>
    3. <span data-ttu-id="870c3-191">Fare clic su **Dettagli** per aprire i dettagli della persona.</span><span class="sxs-lookup"><span data-stu-id="870c3-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="870c3-192">![Dettagli persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Dettagli persona")</span><span class="sxs-lookup"><span data-stu-id="870c3-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="870c3-193">*Dettagli persona*</span><span class="sxs-lookup"><span data-stu-id="870c3-193">*Person's details*</span></span>
4. <span data-ttu-id="870c3-194">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="870c3-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="870c3-195">Si noti che è stata creata l'intera CRUD per l'entità Person all'interno dell'applicazione, dal modello alle visualizzazioni, senza dover scrivere una sola riga di codice.</span><span class="sxs-lookup"><span data-stu-id="870c3-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="870c3-196">Attività 3: aggiornamento del database tramite migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="870c3-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="870c3-197">In questa attività verrà aggiornato il database utilizzando Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="870c3-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="870c3-198">Si scoprirà quanto sia facile modificare il modello e riflettere le modifiche nei database usando la funzionalità di migrazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="870c3-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="870c3-199">Aprire la console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="870c3-199">Open the Package Manager Console.</span></span> <span data-ttu-id="870c3-200">Fare clic su **Strumenti** > **Gestione Pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="870c3-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="870c3-201">Nella Console di Gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="870c3-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="870c3-202">PMC</span><span class="sxs-lookup"><span data-stu-id="870c3-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="870c3-203">![Abilitazione delle migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Abilitare le migrazioni")</span><span class="sxs-lookup"><span data-stu-id="870c3-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="870c3-204">*Abilitazione delle migrazioni*</span><span class="sxs-lookup"><span data-stu-id="870c3-204">*Enabling migrations*</span></span>

    <span data-ttu-id="870c3-205">Il comando Enable-Migration crea la cartella **Migrations** , che contiene uno script per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="870c3-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="870c3-206">![Cartella migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Cartella migrazioni")</span><span class="sxs-lookup"><span data-stu-id="870c3-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="870c3-207">*Cartella migrazioni*</span><span class="sxs-lookup"><span data-stu-id="870c3-207">*Migrations folder*</span></span>
3. <span data-ttu-id="870c3-208">Aprire il file **Configuration.cs** nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="870c3-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="870c3-209">Individuare il costruttore della classe e modificare il valore **AutomaticMigrationsEnabled** in *true*.</span><span class="sxs-lookup"><span data-stu-id="870c3-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="870c3-210">Aprire la classe Person e aggiungere un attributo per il secondo nome della persona.</span><span class="sxs-lookup"><span data-stu-id="870c3-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="870c3-211">Con questo nuovo attributo, il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="870c3-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="870c3-212">Seleziona **compilazione | Compilare la soluzione** nel menu per compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="870c3-213">![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="870c3-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="870c3-214">*Compilazione dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="870c3-214">*Building the application*</span></span>
6. <span data-ttu-id="870c3-215">Nella Console di Gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="870c3-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="870c3-216">PMC</span><span class="sxs-lookup"><span data-stu-id="870c3-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="870c3-217">Questo comando cercherà le modifiche apportate agli oggetti dati, quindi aggiungerà i comandi necessari per modificare il database di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="870c3-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="870c3-218">![Aggiunta di un secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Aggiunta di un secondo nome")</span><span class="sxs-lookup"><span data-stu-id="870c3-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="870c3-219">*Aggiunta di un secondo nome*</span><span class="sxs-lookup"><span data-stu-id="870c3-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="870c3-220">Opzionale È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento differenziale.</span><span class="sxs-lookup"><span data-stu-id="870c3-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="870c3-221">In questo modo sarà possibile aggiornare manualmente il database, in questo caso non è necessario, oppure applicare le modifiche in altri database:</span><span class="sxs-lookup"><span data-stu-id="870c3-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="870c3-222">PMC</span><span class="sxs-lookup"><span data-stu-id="870c3-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="870c3-223">![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generazione di uno script SQL")</span><span class="sxs-lookup"><span data-stu-id="870c3-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="870c3-224">*Generazione di uno script SQL*</span><span class="sxs-lookup"><span data-stu-id="870c3-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="870c3-225">![Aggiornamento script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Aggiornamento script SQL")</span><span class="sxs-lookup"><span data-stu-id="870c3-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="870c3-226">*Aggiornamento script SQL*</span><span class="sxs-lookup"><span data-stu-id="870c3-226">*SQL Script update*</span></span>
8. <span data-ttu-id="870c3-227">Nella console di gestione pacchetti immettere il comando seguente per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="870c3-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="870c3-228">PMC</span><span class="sxs-lookup"><span data-stu-id="870c3-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="870c3-229">![Aggiornamento del database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aggiornamento del database")</span><span class="sxs-lookup"><span data-stu-id="870c3-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="870c3-230">*Aggiornamento del database*</span><span class="sxs-lookup"><span data-stu-id="870c3-230">*Updating the Database*</span></span>

    <span data-ttu-id="870c3-231">Verrà aggiunta la colonna **MiddleName** nella tabella **people** in modo che corrisponda alla definizione corrente della classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="870c3-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="870c3-232">Una volta aggiornato il database, fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi | Controller** per aggiungere di nuovo il controller di persona (completa con gli stessi valori).</span><span class="sxs-lookup"><span data-stu-id="870c3-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="870c3-233">I metodi e le visualizzazioni esistenti vengono aggiornati con l'aggiunta del nuovo attributo.</span><span class="sxs-lookup"><span data-stu-id="870c3-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="870c3-234">![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Aggiunta di un aggiornamento del controller")</span><span class="sxs-lookup"><span data-stu-id="870c3-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="870c3-235">*Aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="870c3-235">*Updating the controller*</span></span>
10. <span data-ttu-id="870c3-236">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="870c3-236">Click **Add**.</span></span> <span data-ttu-id="870c3-237">Selezionare quindi i valori **Sovrascrivi PersonController.cs** e **Sovrascrivi visualizzazioni associate** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="870c3-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Aggiunta di una sovrascrittura del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="870c3-239">*Aggiornamento del controller*</span><span class="sxs-lookup"><span data-stu-id="870c3-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="870c3-240">Task4-esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="870c3-240">Task4- Running the application</span></span>

1. <span data-ttu-id="870c3-241">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="870c3-242">Aprire **/persona**.</span><span class="sxs-lookup"><span data-stu-id="870c3-242">Open **/Person**.</span></span> <span data-ttu-id="870c3-243">Si noti che i dati sono stati conservati, mentre è stata aggiunta la colonna Middle Name.</span><span class="sxs-lookup"><span data-stu-id="870c3-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="870c3-244">![Secondo nome aggiunto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Secondo nome aggiunto")</span><span class="sxs-lookup"><span data-stu-id="870c3-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="870c3-245">*Secondo nome aggiunto*</span><span class="sxs-lookup"><span data-stu-id="870c3-245">*Middle Name added*</span></span>
3. <span data-ttu-id="870c3-246">Se si fa clic su **modifica**, sarà possibile aggiungere un nome intermedio alla persona corrente.</span><span class="sxs-lookup"><span data-stu-id="870c3-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="870c3-247">![Edizione in secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edizione in secondo nome")</span><span class="sxs-lookup"><span data-stu-id="870c3-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="870c3-248">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="870c3-248">Summary</span></span>

<span data-ttu-id="870c3-249">In questo laboratorio pratico sono state apprese semplici procedure per creare operazioni CRUD con l'impalcatura ASP.NET MVC 4 usando qualsiasi classe di modello.</span><span class="sxs-lookup"><span data-stu-id="870c3-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="870c3-250">Si è quindi appreso come eseguire un aggiornamento end-to-end nell'applicazione, dal database alle viste, usando Entity Framework migrazioni.</span><span class="sxs-lookup"><span data-stu-id="870c3-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="870c3-251">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="870c3-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="870c3-252">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="870c3-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="870c3-253">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="870c3-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="870c3-254">Passare a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="870c3-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="870c3-255">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="870c3-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="870c3-256">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="870c3-256">Click on **Install Now**.</span></span> <span data-ttu-id="870c3-257">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="870c3-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="870c3-258">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="870c3-259">![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="870c3-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="870c3-260">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="870c3-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="870c3-261">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="870c3-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="870c3-263">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="870c3-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="870c3-264">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="870c3-264">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="870c3-266">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="870c3-266">*Installation progress*</span></span>
6. <span data-ttu-id="870c3-267">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="870c3-267">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="870c3-269">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="870c3-269">*Installation completed*</span></span>
7. <span data-ttu-id="870c3-270">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="870c3-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="870c3-271">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="870c3-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="870c3-273">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="870c3-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="870c3-274">Appendice B: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="870c3-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="870c3-275">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="870c3-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="870c3-276">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="870c3-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="870c3-277">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="870c3-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="870c3-278">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="870c3-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="870c3-279">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="870c3-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="870c3-280">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="870c3-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="870c3-281">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="870c3-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="870c3-282">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="870c3-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="870c3-283">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="870c3-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="870c3-284">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="870c3-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="870c3-285">![Inizia a digitare il nome del frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="870c3-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="870c3-286">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="870c3-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="870c3-287">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="870c3-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="870c3-288">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="870c3-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="870c3-289">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="870c3-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="870c3-290">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="870c3-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="870c3-291">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="870c3-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="870c3-292">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="870c3-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="870c3-293">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="870c3-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="870c3-294">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="870c3-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="870c3-295">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="870c3-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="870c3-296">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="870c3-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="870c3-297">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="870c3-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="870c3-298">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="870c3-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
