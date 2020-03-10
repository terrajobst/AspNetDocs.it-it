---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modelli e accesso ai dati di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Nota: in questa esercitazione pratica si presuppone che l'utente abbia una conoscenza di base di ASP.NET MVC. Se ASP.NET MVC non è stato usato in precedenza, si consiglia di passare a ASP.NET MVC 4..."
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560200"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="73410-104">Modelli e accesso ai dati di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="73410-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="73410-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="73410-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="73410-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="73410-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="73410-107">Questa esercitazione pratica presuppone la conoscenza di base di **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="73410-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="73410-108">Se **ASP.NET MVC** non è stato usato in precedenza, si consiglia di passare a **ASP.NET MVC 4 Nozioni di base** sul Lab pratico.</span><span class="sxs-lookup"><span data-stu-id="73410-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="73410-109">In questa esercitazione vengono illustrati i miglioramenti e le nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="73410-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-110">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="73410-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="73410-111">Il progetto specifico di questo Lab è disponibile nei [modelli ASP.NET MVC 4 e nell'accesso ai dati](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="73410-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="73410-112">In **ASP.NET MVC nozioni fondamentali** sul Lab è stato passato il passaggio dei dati hardcoded dai controller ai modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="73410-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="73410-113">Tuttavia, per creare un'applicazione Web reale, potrebbe essere necessario utilizzare un database reale.</span><span class="sxs-lookup"><span data-stu-id="73410-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="73410-114">In questo laboratorio pratico verrà illustrato come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione Music Store.</span><span class="sxs-lookup"><span data-stu-id="73410-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="73410-115">A tale scopo, si inizierà con un database esistente e si creerà il Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="73410-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="73410-116">In questo laboratorio si soddisferà l'approccio **database First** e l'approccio **Code First** .</span><span class="sxs-lookup"><span data-stu-id="73410-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="73410-117">Tuttavia, è possibile utilizzare anche l'approccio **Model First** , creare lo stesso modello utilizzando gli strumenti di e quindi generare il database.</span><span class="sxs-lookup"><span data-stu-id="73410-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="73410-118">![Confronto tra Database First e Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Confronto tra Database First e Model First")</span><span class="sxs-lookup"><span data-stu-id="73410-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="73410-119">*Confronto tra Database First e Model First*</span><span class="sxs-lookup"><span data-stu-id="73410-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="73410-120">Dopo la generazione del modello, si apporteranno le modifiche appropriate in StoreController per fornire le visualizzazioni di archivio con i dati ricavati dal database, invece di usare dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="73410-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="73410-121">Non è necessario apportare alcuna modifica ai modelli di visualizzazione perché StoreController restituirà gli stessi ViewModel ai modelli di visualizzazione, anche se questa volta i dati proverranno dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="73410-122">**Approccio Code First**</span><span class="sxs-lookup"><span data-stu-id="73410-122">**The Code First Approach**</span></span>

<span data-ttu-id="73410-123">Il Code First approccio consente di definire il modello dal codice senza generare classi normalmente associate al Framework.</span><span class="sxs-lookup"><span data-stu-id="73410-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="73410-124">In Code First gli oggetti modello sono definiti con POCO, &quot;oggetti CLR obsoleti&quot;.</span><span class="sxs-lookup"><span data-stu-id="73410-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="73410-125">POCO sono semplici classi semplici senza ereditarietà e che non implementano interfacce.</span><span class="sxs-lookup"><span data-stu-id="73410-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="73410-126">È possibile generare automaticamente il database da essi oppure è possibile usare un database esistente e generare il mapping della classe dal codice.</span><span class="sxs-lookup"><span data-stu-id="73410-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="73410-127">I vantaggi derivanti dall'utilizzo di questo approccio sono che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), perché le classi POCO non sono associate al Framework di mapping.</span><span class="sxs-lookup"><span data-stu-id="73410-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-128">Questa esercitazione è basata su ASP.NET MVC 4 e una versione dell'applicazione di esempio Music Store personalizzata e ridotta a icona per adattarsi solo alle funzionalità illustrate in questo laboratorio pratico.</span><span class="sxs-lookup"><span data-stu-id="73410-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="73410-129">Per esplorare l'intera applicazione di esercitazione di **Music Store** , è possibile trovarla in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="73410-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="73410-130">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="73410-130">Prerequisites</span></span>

<span data-ttu-id="73410-131">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73410-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="73410-132">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o Superior (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="73410-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="73410-133">Configurazione</span><span class="sxs-lookup"><span data-stu-id="73410-133">Setup</span></span>

<span data-ttu-id="73410-134">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="73410-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="73410-135">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73410-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="73410-136">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="73410-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="73410-137">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="73410-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="73410-138">Esercizi</span><span class="sxs-lookup"><span data-stu-id="73410-138">Exercises</span></span>

<span data-ttu-id="73410-139">Questo laboratorio pratico è costituito dagli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73410-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="73410-140">Esercizio 1: aggiunta di un database</span><span class="sxs-lookup"><span data-stu-id="73410-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="73410-141">Esercizio 2: creazione di un database con Code First</span><span class="sxs-lookup"><span data-stu-id="73410-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="73410-142">Esercizio 3: esecuzione di query sul database con parametri</span><span class="sxs-lookup"><span data-stu-id="73410-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="73410-143">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="73410-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="73410-144">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="73410-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="73410-145">Tempo stimato per il completamento del Lab: **35 minuti**.</span><span class="sxs-lookup"><span data-stu-id="73410-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="73410-146">Esercizio 1: aggiunta di un database</span><span class="sxs-lookup"><span data-stu-id="73410-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="73410-147">In questo esercizio verrà illustrato come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per poter utilizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="73410-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="73410-148">Una volta che il database è stato generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire al modello di vista i dati ricavati dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="73410-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="73410-149">Attività 1-aggiunta di un database</span><span class="sxs-lookup"><span data-stu-id="73410-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="73410-150">In questa attività verrà aggiunto un database già creato con le tabelle principali dell'applicazione MusicStore alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="73410-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="73410-151">Aprire la soluzione **Begin** disponibile nella cartella **source/EX1-AddingADatabaseDBFirst/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="73410-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="73410-152">Prima di continuare, sarà necessario scaricare alcuni pacchetti NuGet mancanti.</span><span class="sxs-lookup"><span data-stu-id="73410-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="73410-153">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="73410-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="73410-154">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="73410-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="73410-155">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="73410-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="73410-156">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="73410-157">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="73410-158">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="73410-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="73410-159">Aggiungere un file di database **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="73410-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="73410-160">In questo laboratorio pratico si userà un database già creato denominato **MvcMusicStore. MDF**.</span><span class="sxs-lookup"><span data-stu-id="73410-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="73410-161">A tale scopo, fare clic con il pulsante destro del mouse su **App\_dati** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="73410-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="73410-162">Passare a **\Source\Assets** e selezionare il file **MvcMusicStore. MDF** .</span><span class="sxs-lookup"><span data-stu-id="73410-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="73410-163">![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Aggiunta di un elemento esistente")</span><span class="sxs-lookup"><span data-stu-id="73410-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="73410-164">*Aggiunta di un elemento esistente*</span><span class="sxs-lookup"><span data-stu-id="73410-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="73410-165">![File di database MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "File di database MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="73410-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="73410-166">*File di database MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="73410-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="73410-167">Il database è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-167">The database has been added to the project.</span></span> <span data-ttu-id="73410-168">Anche quando il database si trova all'interno della soluzione, è possibile eseguire una query e aggiornarlo perché era ospitato in un server di database diverso.</span><span class="sxs-lookup"><span data-stu-id="73410-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="73410-169">![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "Database MvcMusicStore in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="73410-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="73410-170">*Database MvcMusicStore in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="73410-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="73410-171">Verificare la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="73410-171">Verify the connection to the database.</span></span> <span data-ttu-id="73410-172">A tale scopo, fare doppio clic su **MvcMusicStore. MDF** per stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="73410-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="73410-173">![Connessione a MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connessione a MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="73410-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="73410-174">*Connessione a MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="73410-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="73410-175">Attività 2: creazione di un modello di dati</span><span class="sxs-lookup"><span data-stu-id="73410-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="73410-176">In questa attività verrà creato un modello di dati per interagire con il database aggiunto nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="73410-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="73410-177">Creare un modello di dati che rappresenterà il database.</span><span class="sxs-lookup"><span data-stu-id="73410-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="73410-178">A tale scopo, nella Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **modelli** , scegliere **Aggiungi** , quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="73410-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="73410-179">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello di **dati** e quindi l'elemento **ADO.NET Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="73410-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="73410-180">Modificare il nome del modello di dati in **StoreDB. edmx** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="73410-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="73410-181">![Aggiunta del Entity Data Model StoreDB ADO.NET](aspnet-mvc-4-models-and-data-access/_static/image6.png "Aggiunta del Entity Data Model StoreDB ADO.NET")</span><span class="sxs-lookup"><span data-stu-id="73410-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="73410-182">*Aggiunta del Entity Data Model StoreDB ADO.NET*</span><span class="sxs-lookup"><span data-stu-id="73410-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="73410-183">Verrà visualizzata la **procedura guidata Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="73410-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="73410-184">Questa procedura guidata consente di creare il livello del modello.</span><span class="sxs-lookup"><span data-stu-id="73410-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="73410-185">Poiché il modello deve essere creato in base al database esistente aggiunto di recente, selezionare **genera da database** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="73410-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="73410-186">![Scelta del contenuto del modello](aspnet-mvc-4-models-and-data-access/_static/image7.png "Scelta del contenuto del modello")</span><span class="sxs-lookup"><span data-stu-id="73410-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="73410-187">*Scelta del contenuto del modello*</span><span class="sxs-lookup"><span data-stu-id="73410-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="73410-188">Poiché si sta generando un modello da un database, sarà necessario specificare la connessione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="73410-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="73410-189">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="73410-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="73410-190">Selezionare **Microsoft SQL Server file di database** e fare clic su **continua**.</span><span class="sxs-lookup"><span data-stu-id="73410-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="73410-191">![Scegli origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "Scegli origine dati")</span><span class="sxs-lookup"><span data-stu-id="73410-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="73410-192">*Finestra di dialogo Scegli origine dati*</span><span class="sxs-lookup"><span data-stu-id="73410-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="73410-193">Fare clic su **Sfoglia** e selezionare il database **MvcMusicStore. MDF** che si trova nella cartella **app\_data** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="73410-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="73410-194">![Proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "Proprietà di connessione")</span><span class="sxs-lookup"><span data-stu-id="73410-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="73410-195">*Proprietà di connessione*</span><span class="sxs-lookup"><span data-stu-id="73410-195">*Connection properties*</span></span>
6. <span data-ttu-id="73410-196">La classe generata deve avere lo stesso nome della stringa di connessione dell'entità, quindi modificare il nome in **MusicStoreEntities** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="73410-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="73410-197">![Scelta della connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "Scelta della connessione dati")</span><span class="sxs-lookup"><span data-stu-id="73410-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="73410-198">*Scelta della connessione dati*</span><span class="sxs-lookup"><span data-stu-id="73410-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="73410-199">Consente di scegliere gli oggetti di database da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="73410-199">Choose the database objects to use.</span></span> <span data-ttu-id="73410-200">Poiché il modello di entità utilizzerà solo le tabelle del database, selezionare l'opzione **Tables** e verificare che siano selezionate anche le opzioni **Includi colonne di chiavi esterne nel modello** e **plurali o singolari** .</span><span class="sxs-lookup"><span data-stu-id="73410-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="73410-201">Modificare lo spazio dei nomi del modello in **MvcMusicStore. Model** e fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="73410-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="73410-202">![Scelta degli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "Scelta degli oggetti di database")</span><span class="sxs-lookup"><span data-stu-id="73410-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="73410-203">*Scelta degli oggetti di database*</span><span class="sxs-lookup"><span data-stu-id="73410-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-204">Se viene visualizzata una finestra di dialogo di avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per le entità del modello.</span><span class="sxs-lookup"><span data-stu-id="73410-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="73410-205">Verrà visualizzato un diagramma entità per il database, mentre una classe separata che esegue il mapping di ogni tabella al database verrà creata.</span><span class="sxs-lookup"><span data-stu-id="73410-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="73410-206">Ad esempio, la tabella **albums** sarà rappresentata da una classe **album** , in cui ogni colonna della tabella eseguirà il mapping a una proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="73410-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="73410-207">In questo modo sarà possibile eseguire query e utilizzare oggetti che rappresentano righe nel database.</span><span class="sxs-lookup"><span data-stu-id="73410-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="73410-208">![Diagramma entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagramma dell'entità")</span><span class="sxs-lookup"><span data-stu-id="73410-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="73410-209">*Diagramma entità*</span><span class="sxs-lookup"><span data-stu-id="73410-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-210">I modelli T4 (. TT) eseguono codice per generare le classi di entità e sovrascrivono le classi esistenti con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="73410-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="73410-211">In questo esempio le classi &quot;album&quot;, &quot;genre&quot; e &quot;Artist&quot; sono state sovrascritte con il codice generato.</span><span class="sxs-lookup"><span data-stu-id="73410-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="73410-212">Attività 3: creazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="73410-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="73410-213">In questa attività si verificherà che, sebbene la generazione del modello abbia rimosso le classi del modello **album**, **genre** e **Artist** , il progetto verrà compilato correttamente usando le nuove classi del modello di dati.</span><span class="sxs-lookup"><span data-stu-id="73410-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="73410-214">Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="73410-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="73410-215">![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilazione del progetto")</span><span class="sxs-lookup"><span data-stu-id="73410-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="73410-216">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="73410-216">*Building the project*</span></span>
2. <span data-ttu-id="73410-217">Il progetto è stato compilato correttamente.</span><span class="sxs-lookup"><span data-stu-id="73410-217">The project builds successfully.</span></span> <span data-ttu-id="73410-218">Perché funziona ancora?</span><span class="sxs-lookup"><span data-stu-id="73410-218">Why does it still work?</span></span> <span data-ttu-id="73410-219">Il funzionamento è dovuto al fatto che le tabelle di database includono campi che includono le proprietà utilizzate nelle classi rimosse **album** e **genere**.</span><span class="sxs-lookup"><span data-stu-id="73410-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="73410-220">![Compilazioni riuscite](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilazioni riuscite")</span><span class="sxs-lookup"><span data-stu-id="73410-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="73410-221">*Compilazioni riuscite*</span><span class="sxs-lookup"><span data-stu-id="73410-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="73410-222">Mentre la finestra di progettazione Visualizza le entità in un formato di diagramma, C# sono effettivamente classi.</span><span class="sxs-lookup"><span data-stu-id="73410-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="73410-223">Espandere il nodo **StoreDB. edmx** nella Esplora soluzioni e quindi **StoreDB.TT**, vengono visualizzate le nuove entità generate.</span><span class="sxs-lookup"><span data-stu-id="73410-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="73410-224">![File generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "File generati")</span><span class="sxs-lookup"><span data-stu-id="73410-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="73410-225">*File generati*</span><span class="sxs-lookup"><span data-stu-id="73410-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="73410-226">Attività 4: esecuzione di query sul database</span><span class="sxs-lookup"><span data-stu-id="73410-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="73410-227">In questa attività verrà aggiornata la classe StoreController in modo che, invece di utilizzare dati hardcoded, effettuerà una query sul database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="73410-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="73410-228">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per mantenere un'istanza della classe **MusicStoreEntities** , denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="73410-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="73410-229">(Frammento di codice: *modelli e accesso ai dati-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="73410-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="73410-230">La classe **MusicStoreEntities** espone una proprietà di raccolta per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="73410-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="73410-231">Aggiorna il metodo di azione **Browse** per recuperare un genere con tutti gli **album**.</span><span class="sxs-lookup"><span data-stu-id="73410-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="73410-232">(Frammento di codice: *modelli e accesso ai dati-Sfoglia Archivio EX1*)</span><span class="sxs-lookup"><span data-stu-id="73410-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="73410-233">Si usa una funzionalità di .NET denominata **LINQ** (Language-Integrated Query) per scrivere espressioni di query fortemente tipizzate in queste raccolte, che eseguiranno il codice sul database e restituiranno oggetti con cui è possibile programmare.</span><span class="sxs-lookup"><span data-stu-id="73410-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="73410-234">Per ulteriori informazioni su LINQ, visitare il [sito MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="73410-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="73410-235">Metodo di azione Update **index** per recuperare tutti i generi.</span><span class="sxs-lookup"><span data-stu-id="73410-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="73410-236">(Frammento di codice- *modelli e accesso ai dati-Indice archivio EX1*)</span><span class="sxs-lookup"><span data-stu-id="73410-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="73410-237">Metodo di azione Update **index** per recuperare tutti i generi e trasformare la raccolta in un elenco.</span><span class="sxs-lookup"><span data-stu-id="73410-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="73410-238">(Frammento di codice- *modelli e accesso ai dati-GenreMenu archivio EX1*)</span><span class="sxs-lookup"><span data-stu-id="73410-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="73410-239">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="73410-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="73410-240">In questa attività si verificherà che nella pagina store index verranno ora visualizzati i generi archiviati nel database anziché quelli hardcoded.</span><span class="sxs-lookup"><span data-stu-id="73410-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="73410-241">Non è necessario modificare il modello di visualizzazione perché **StoreController** restituisce le stesse entità di prima, anche se questa volta i dati proverranno dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="73410-242">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73410-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="73410-243">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="73410-243">The project starts in the Home page.</span></span> <span data-ttu-id="73410-244">Verificare che il menu dei **generi** non sia più un elenco hardcoded e che i dati vengano recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="73410-246">*Esplorazione dei generi dal database*</span><span class="sxs-lookup"><span data-stu-id="73410-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="73410-247">A questo punto, passare a qualsiasi genere e verificare che gli album siano popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="73410-248">![Esplorazione di album dal database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Esplorazione di album dal database")</span><span class="sxs-lookup"><span data-stu-id="73410-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="73410-249">*Esplorazione di album dal database*</span><span class="sxs-lookup"><span data-stu-id="73410-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="73410-250">Esercizio 2: creazione di un database con Code First</span><span class="sxs-lookup"><span data-stu-id="73410-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="73410-251">In questo esercizio verrà illustrato come utilizzare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai relativi dati.</span><span class="sxs-lookup"><span data-stu-id="73410-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="73410-252">Una volta generato il modello, si modificherà il StoreController in modo da fornire il modello di visualizzazione con i dati ricavati dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="73410-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-253">Se è stato completato l'esercizio 1 e si è già lavorato con l'approccio Database First, verrà ora illustrato come ottenere gli stessi risultati con un processo diverso.</span><span class="sxs-lookup"><span data-stu-id="73410-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="73410-254">Le attività in comune con l'esercizio 1 sono state contrassegnate per semplificare la lettura.</span><span class="sxs-lookup"><span data-stu-id="73410-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="73410-255">Se l'esercizio 1 non è stato completato ma si desidera apprendere l'approccio Code First, è possibile iniziare da questo esercizio e ottenere una copertura completa dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="73410-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="73410-256">Attività 1: popolamento dei dati di esempio</span><span class="sxs-lookup"><span data-stu-id="73410-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="73410-257">In questa attività, il database verrà popolato con dati di esempio quando viene creato inizialmente usando Code-First.</span><span class="sxs-lookup"><span data-stu-id="73410-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="73410-258">Aprire la soluzione **Begin** disponibile nella cartella **source/EX2-CreatingADatabaseCodeFirst/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="73410-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="73410-259">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="73410-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="73410-260">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="73410-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="73410-261">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="73410-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="73410-262">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="73410-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="73410-263">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="73410-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="73410-264">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="73410-265">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="73410-266">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="73410-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="73410-267">Aggiungere il file **SampleData.cs** alla cartella **Models** .</span><span class="sxs-lookup"><span data-stu-id="73410-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="73410-268">A tale scopo, fare clic con il pulsante destro del mouse su **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="73410-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="73410-269">Passare a **\Source\Assets** e selezionare il file **SampleData.cs** .</span><span class="sxs-lookup"><span data-stu-id="73410-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="73410-270">![Dati di esempio popolare il codice](aspnet-mvc-4-models-and-data-access/_static/image18.png "Dati di esempio popolare il codice")</span><span class="sxs-lookup"><span data-stu-id="73410-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="73410-271">*Dati di esempio popolare il codice*</span><span class="sxs-lookup"><span data-stu-id="73410-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="73410-272">Aprire il file **Global.asax.cs** e aggiungere le istruzioni *using* seguenti.</span><span class="sxs-lookup"><span data-stu-id="73410-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="73410-273">(Frammento di codice: *modelli e accesso ai dati-EX2 Global asax using*)</span><span class="sxs-lookup"><span data-stu-id="73410-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="73410-274">Nel metodo **Application\_Start ()** aggiungere la riga seguente per impostare l'inizializzatore di database.</span><span class="sxs-lookup"><span data-stu-id="73410-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="73410-275">(Frammento di codice- *modelli e accesso ai dati-EX2 Global asax Seinitializer*)</span><span class="sxs-lookup"><span data-stu-id="73410-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="73410-276">Attività 2: configurazione della connessione al database</span><span class="sxs-lookup"><span data-stu-id="73410-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="73410-277">Ora che è già stato aggiunto un database al progetto, il file **Web. config** verrà scritto nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="73410-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="73410-278">Aggiungere una stringa di connessione in **Web. config**. A tale scopo, aprire il **file Web. config** nella radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con questa riga nella sezione **&lt;connectionStrings&gt;** :</span><span class="sxs-lookup"><span data-stu-id="73410-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="73410-279">![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Percorso del file Web. config")</span><span class="sxs-lookup"><span data-stu-id="73410-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="73410-280">*Percorso del file Web. config*</span><span class="sxs-lookup"><span data-stu-id="73410-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="73410-281">Attività 3-uso del modello</span><span class="sxs-lookup"><span data-stu-id="73410-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="73410-282">Ora che è già stata configurata la connessione al database, il modello verrà collegato con le tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="73410-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="73410-283">In questa attività verrà creata una classe che verrà collegata al database con Code First.</span><span class="sxs-lookup"><span data-stu-id="73410-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="73410-284">Tenere presente che esiste una classe del modello POCO esistente che deve essere modificata.</span><span class="sxs-lookup"><span data-stu-id="73410-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-285">Se è stato completato l'esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="73410-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="73410-286">In Code First, si creeranno manualmente classi che verranno collegate alle entità di dati.</span><span class="sxs-lookup"><span data-stu-id="73410-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="73410-287">Aprire il **genere** di classe del modello poco dalla cartella del progetto **Models** e includere un ID.</span><span class="sxs-lookup"><span data-stu-id="73410-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="73410-288">Usare una proprietà int con il nome **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="73410-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="73410-289">(Frammento di codice- *modelli e accesso ai dati-Ex2 Code First genere*)</span><span class="sxs-lookup"><span data-stu-id="73410-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="73410-290">Per lavorare con le convenzioni di Code First, il genere della classe deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="73410-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="73410-291">Per altre informazioni sulle convenzioni di Code First, vedere questo [articolo di MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="73410-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="73410-292">Aprire ora l' **album** della classe del modello poco dalla cartella del progetto **Models** e includere le chiavi esterne, creare le proprietà con i nomi **GenreId** e **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="73410-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="73410-293">Questa classe dispone già di **GenreId** per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="73410-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="73410-294">(Frammento di codice- *modelli e accesso ai dati-Ex2 Code First Album*)</span><span class="sxs-lookup"><span data-stu-id="73410-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="73410-295">Aprire l' **autore** della classe del modello poco e includere la proprietà **ArtistId** .</span><span class="sxs-lookup"><span data-stu-id="73410-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="73410-296">(Frammento di codice- *modelli e accesso ai dati-Ex2 Code First Artist*)</span><span class="sxs-lookup"><span data-stu-id="73410-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="73410-297">Fare clic con il pulsante destro del mouse sulla cartella del progetto **Models** e scegliere **Aggiungi | Classe**.</span><span class="sxs-lookup"><span data-stu-id="73410-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="73410-298">Denominare il file **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="73410-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="73410-299">Quindi, fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="73410-299">Then, click **Add.**</span></span>

    <span data-ttu-id="73410-300">![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="73410-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="73410-301">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="73410-301">*Adding a new item*</span></span>

    <span data-ttu-id="73410-302">![Aggiunta di un Class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Aggiunta di un Class2")</span><span class="sxs-lookup"><span data-stu-id="73410-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="73410-303">*Aggiunta di una classe*</span><span class="sxs-lookup"><span data-stu-id="73410-303">*Adding a class*</span></span>
5. <span data-ttu-id="73410-304">Aprire la classe appena creata, **MusicStoreEntities.cs**, e includere gli spazi dei nomi **System. Data. Entity** e **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="73410-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="73410-305">Sostituire la dichiarazione di classe per estendere la classe **DbContext** : dichiarare un **DBSet** pubblico ed eseguire l'override del metodo **OnModelCreating** .</span><span class="sxs-lookup"><span data-stu-id="73410-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="73410-306">Dopo questo passaggio si otterrà una classe di dominio che collegherà il modello con la Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="73410-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="73410-307">A tale scopo, sostituire il codice della classe con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="73410-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="73410-308">(Frammento di codice- *modelli e accesso ai dati-Ex2 Code First MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="73410-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="73410-309">Con Entity Framework **DbContext** e **DBSet** sarà possibile eseguire una query sul genere della classe poco.</span><span class="sxs-lookup"><span data-stu-id="73410-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="73410-310">Estendendo il metodo **OnModelCreating** , viene specificato nel **codice** come verrà eseguito il mapping del genere a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="73410-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="73410-311">Altre informazioni su DBContext e DBSet sono disponibili in questo articolo di MSDN: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="73410-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="73410-312">Attività 4: esecuzione di query sul database</span><span class="sxs-lookup"><span data-stu-id="73410-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="73410-313">In questa attività verrà aggiornata la classe StoreController in modo che, invece di usare i dati hardcoded, la recupererà dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-314">Questa attività è in comune con l'esercizio 1.</span><span class="sxs-lookup"><span data-stu-id="73410-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="73410-315">Se è stato completato l'esercizio 1, si noterà che questi passaggi sono gli stessi in entrambi gli approcci (database First o Code First).</span><span class="sxs-lookup"><span data-stu-id="73410-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="73410-316">Sono diversi nella modalità di collegamento dei dati con il modello, ma l'accesso alle entità di dati è ancora trasparente dal controller.</span><span class="sxs-lookup"><span data-stu-id="73410-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="73410-317">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per mantenere un'istanza della classe **MusicStoreEntities** , denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="73410-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="73410-318">(Frammento di codice: *modelli e accesso ai dati-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="73410-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="73410-319">La classe **MusicStoreEntities** espone una proprietà di raccolta per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="73410-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="73410-320">Aggiorna il metodo di azione **Browse** per recuperare un genere con tutti gli **album**.</span><span class="sxs-lookup"><span data-stu-id="73410-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="73410-321">(Frammento di codice- *modelli e accesso ai dati-EX2 store browse*)</span><span class="sxs-lookup"><span data-stu-id="73410-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="73410-322">Si usa una funzionalità di .NET denominata **LINQ** (Language-Integrated Query) per scrivere espressioni di query fortemente tipizzate in queste raccolte, che eseguiranno il codice sul database e restituiranno oggetti con cui è possibile programmare.</span><span class="sxs-lookup"><span data-stu-id="73410-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="73410-323">Per ulteriori informazioni su LINQ, visitare il [sito MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="73410-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="73410-324">Metodo di azione Update **index** per recuperare tutti i generi.</span><span class="sxs-lookup"><span data-stu-id="73410-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="73410-325">(Frammento di codice- *modelli e accesso ai dati-Indice archivio EX2*)</span><span class="sxs-lookup"><span data-stu-id="73410-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="73410-326">Metodo di azione Update **index** per recuperare tutti i generi e trasformare la raccolta in un elenco.</span><span class="sxs-lookup"><span data-stu-id="73410-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="73410-327">(Frammento di codice: *modelli e accesso ai dati-archivio EX2 GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="73410-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="73410-328">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="73410-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="73410-329">In questa attività si verificherà che nella pagina store index verranno ora visualizzati i generi archiviati nel database anziché quelli hardcoded.</span><span class="sxs-lookup"><span data-stu-id="73410-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="73410-330">Non è necessario modificare il modello di visualizzazione perché **StoreController** restituisce lo stesso **StoreIndexViewModel** precedente, ma questa volta i dati proverranno dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="73410-331">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73410-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="73410-332">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="73410-332">The project starts in the Home page.</span></span> <span data-ttu-id="73410-333">Verificare che il menu dei **generi** non sia più un elenco hardcoded e che i dati vengano recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="73410-335">*Esplorazione dei generi dal database*</span><span class="sxs-lookup"><span data-stu-id="73410-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="73410-336">A questo punto, passare a qualsiasi genere e verificare che gli album siano popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="73410-337">![Esplorazione di album dal database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Esplorazione di album dal database")</span><span class="sxs-lookup"><span data-stu-id="73410-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="73410-338">*Esplorazione di album dal database*</span><span class="sxs-lookup"><span data-stu-id="73410-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="73410-339">Esercizio 3: esecuzione di query sul database con parametri</span><span class="sxs-lookup"><span data-stu-id="73410-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="73410-340">In questo esercizio verrà illustrato come eseguire una query sul database utilizzando i parametri e come utilizzare il data shaping per i risultati della query, una funzionalità che consente di ridurre il numero di accessi al database in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="73410-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="73410-341">Per ulteriori informazioni sulla modellazione dei risultati delle query, vedere l' [articolo MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)seguente.</span><span class="sxs-lookup"><span data-stu-id="73410-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="73410-342">Attività 1: modifica di StoreController per recuperare gli album dal database</span><span class="sxs-lookup"><span data-stu-id="73410-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="73410-343">In questa attività verrà modificata la classe **StoreController** per accedere al database per recuperare gli album da un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="73410-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="73410-344">Se si vuole usare un approccio basato sul database, aprire la soluzione **Begin** disponibile nella cartella **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** se si vuole usare l'approccio Code-First o la cartella **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** .</span><span class="sxs-lookup"><span data-stu-id="73410-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="73410-345">In caso contrario, è possibile continuare a usare la soluzione **finale** ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="73410-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="73410-346">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="73410-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="73410-347">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="73410-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="73410-348">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="73410-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="73410-349">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="73410-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="73410-350">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="73410-351">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="73410-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="73410-352">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="73410-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="73410-353">Aprire la classe **StoreController** per modificare il metodo di azione **Browse** .</span><span class="sxs-lookup"><span data-stu-id="73410-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="73410-354">A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="73410-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="73410-355">Modificare il metodo di azione **Browse** per recuperare gli album per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="73410-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="73410-356">A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="73410-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="73410-357">(Frammento di codice- *modelli e accesso ai dati-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="73410-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="73410-358">Per popolare una raccolta di entità, è necessario usare il metodo **include** per specificare che si desidera recuperare anche gli album.</span><span class="sxs-lookup"><span data-stu-id="73410-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="73410-359">È possibile utilizzare. Estensione **Single ()** in LINQ perché in questo caso è previsto un solo genere per un album.</span><span class="sxs-lookup"><span data-stu-id="73410-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="73410-360">Il metodo **Single ()** accetta un'espressione lambda come parametro, che in questo caso specifica un singolo oggetto Genre, in modo che il nome corrisponda al valore definito.</span><span class="sxs-lookup"><span data-stu-id="73410-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="73410-361">Si utilizzerà una funzionalità che consente di indicare anche altre entità correlate che si desidera caricare quando viene recuperato l'oggetto Genre.</span><span class="sxs-lookup"><span data-stu-id="73410-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="73410-362">Questa funzionalità è denominata **shaping dei risultati della query**e consente di ridurre il numero di volte in cui è necessario accedere al database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="73410-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="73410-363">In questo scenario, è preferibile eseguire il recupero preliminare degli album per il genere recuperato.</span><span class="sxs-lookup"><span data-stu-id="73410-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="73410-364">La query include i **generi. includere (&quot;album&quot;)** per indicare che si desiderano anche gli album correlati.</span><span class="sxs-lookup"><span data-stu-id="73410-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="73410-365">Questa operazione comporterà un'applicazione più efficiente, perché recupererà i dati di genere e di album in una singola richiesta di database.</span><span class="sxs-lookup"><span data-stu-id="73410-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="73410-366">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="73410-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="73410-367">In questa attività verrà eseguita l'applicazione e verranno recuperati gli album di un genere specifico dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="73410-368">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73410-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="73410-369">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="73410-369">The project starts in the Home page.</span></span> <span data-ttu-id="73410-370">Modificare l'URL in **/Store/browse? genre = pop** per verificare che i risultati siano stati recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="73410-371">![Esplorazione per genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "Esplorazione per genere")</span><span class="sxs-lookup"><span data-stu-id="73410-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="73410-372">*Esplorazione di/Store/Browse? genre = pop*</span><span class="sxs-lookup"><span data-stu-id="73410-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="73410-373">Attività 3: accesso agli album in base all'ID</span><span class="sxs-lookup"><span data-stu-id="73410-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="73410-374">In questa attività verrà ripetuta la procedura precedente per ottenere gli album in base al relativo ID.</span><span class="sxs-lookup"><span data-stu-id="73410-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="73410-375">Se necessario, chiudere il browser per tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73410-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="73410-376">Aprire la classe **StoreController** per modificare il metodo di azione **Dettagli** .</span><span class="sxs-lookup"><span data-stu-id="73410-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="73410-377">A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="73410-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="73410-378">Modificare il metodo di azione **Dettagli** per recuperare i dettagli degli album in base al relativo **ID**. A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="73410-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="73410-379">(Frammento di codice- *modelli e accesso ai dati-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="73410-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="73410-380">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="73410-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="73410-381">In questa attività verrà eseguita l'applicazione in un Web browser per ottenere i dettagli degli album in base al relativo ID.</span><span class="sxs-lookup"><span data-stu-id="73410-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="73410-382">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73410-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="73410-383">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="73410-383">The project starts in the Home page.</span></span> <span data-ttu-id="73410-384">Modificare l'URL in **/Store/Details/51** o sfogliare i generi e selezionare un album per verificare che i risultati siano stati recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="73410-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="73410-385">![Dettagli esplorazione](aspnet-mvc-4-models-and-data-access/_static/image25.png "Dettagli esplorazione")</span><span class="sxs-lookup"><span data-stu-id="73410-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="73410-386">*Esplorazione di/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="73410-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="73410-387">Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="73410-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="73410-388">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="73410-388">Summary</span></span>

<span data-ttu-id="73410-389">Completando questo laboratorio pratico, sono state apprese le nozioni di base dei modelli e dell'accesso ai dati di ASP.NET MVC, usando il **database First** approccio e l'approccio **Code First** :</span><span class="sxs-lookup"><span data-stu-id="73410-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="73410-390">Come aggiungere un database alla soluzione per utilizzarne i dati</span><span class="sxs-lookup"><span data-stu-id="73410-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="73410-391">Come aggiornare i controller per fornire modelli di visualizzazione con i dati ricavati dal database invece di quelli hardcoded</span><span class="sxs-lookup"><span data-stu-id="73410-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="73410-392">Come eseguire una query sul database usando i parametri</span><span class="sxs-lookup"><span data-stu-id="73410-392">How to query the database using parameters</span></span>
- <span data-ttu-id="73410-393">Come usare la definizione dei risultati della query, una funzionalità che riduce il numero di accessi al database, il recupero dei dati in modo più efficiente</span><span class="sxs-lookup"><span data-stu-id="73410-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="73410-394">Come usare sia Database First che Code First approcci in Microsoft Entity Framework per collegare il database al modello</span><span class="sxs-lookup"><span data-stu-id="73410-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="73410-395">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="73410-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="73410-396">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="73410-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="73410-397">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="73410-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="73410-398">Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="73410-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="73410-399">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="73410-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="73410-400">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="73410-400">Click on **Install Now**.</span></span> <span data-ttu-id="73410-401">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="73410-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="73410-402">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="73410-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="73410-403">![Installa Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="73410-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="73410-404">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="73410-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="73410-405">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="73410-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="73410-407">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="73410-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="73410-408">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="73410-408">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="73410-410">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="73410-410">*Installation progress*</span></span>
6. <span data-ttu-id="73410-411">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="73410-411">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="73410-413">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="73410-413">*Installation completed*</span></span>
7. <span data-ttu-id="73410-414">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="73410-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="73410-415">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="73410-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="73410-417">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="73410-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="73410-418">Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="73410-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="73410-419">In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="73410-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="73410-420">Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure</span><span class="sxs-lookup"><span data-stu-id="73410-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="73410-421">Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="73410-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-422">Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico.</span><span class="sxs-lookup"><span data-stu-id="73410-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="73410-423">È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="73410-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="73410-424">![Accedere a Windows portale di Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Accedere a Windows portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="73410-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="73410-425">*Accedere a portale di gestione di Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="73410-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="73410-426">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="73410-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="73410-427">![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creazione di un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="73410-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="73410-428">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="73410-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="73410-429">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="73410-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="73410-430">Quindi selezionare opzione **creazione rapida** .</span><span class="sxs-lookup"><span data-stu-id="73410-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="73410-431">Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="73410-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-432">Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="73410-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="73410-433">L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="73410-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="73410-434">Non sono inclusi i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="73410-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="73410-435">![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creazione di un nuovo sito Web mediante creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="73410-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="73410-436">*Creazione di un nuovo sito Web mediante creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="73410-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="73410-437">Attendere la creazione del nuovo **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="73410-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="73410-438">Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** .</span><span class="sxs-lookup"><span data-stu-id="73410-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="73410-439">Verificare che il nuovo sito Web sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="73410-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="73410-440">![Esplorazione del nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Esplorazione del nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="73410-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="73410-441">*Esplorazione del nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="73410-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="73410-442">![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "Sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="73410-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="73410-443">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="73410-443">*Web site running*</span></span>
6. <span data-ttu-id="73410-444">Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="73410-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="73410-445">![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Apertura delle pagine di gestione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="73410-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="73410-446">*Apertura delle pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="73410-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="73410-447">Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .</span><span class="sxs-lookup"><span data-stu-id="73410-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-448">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="73410-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="73410-449">Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="73410-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="73410-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="73410-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="73410-451">![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Download del profilo di pubblicazione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="73410-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="73410-452">*Download del profilo di pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="73410-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="73410-453">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="73410-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="73410-454">Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73410-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="73410-455">![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "Salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="73410-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="73410-456">*Salvataggio del file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="73410-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="73410-457">Attività 2: configurazione del server di database</span><span class="sxs-lookup"><span data-stu-id="73410-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="73410-458">Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="73410-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="73410-459">Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="73410-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="73410-460">Per archiviare il database dell'applicazione, sarà necessario un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="73410-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="73410-461">È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**.</span><span class="sxs-lookup"><span data-stu-id="73410-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="73410-462">Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="73410-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="73410-463">Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive.</span><span class="sxs-lookup"><span data-stu-id="73410-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="73410-464">Non creare ancora il database, perché verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="73410-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="73410-465">![Dashboard del server di database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard del server di database SQL")</span><span class="sxs-lookup"><span data-stu-id="73410-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="73410-466">*Dashboard del server di database SQL*</span><span class="sxs-lookup"><span data-stu-id="73410-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="73410-467">Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server.</span><span class="sxs-lookup"><span data-stu-id="73410-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="73410-468">A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-models-and-data-access/_static/image40.png).</span><span class="sxs-lookup"><span data-stu-id="73410-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="73410-470">*Aggiunta dell'indirizzo IP del client*</span><span class="sxs-lookup"><span data-stu-id="73410-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="73410-471">Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="73410-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Conferma modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="73410-473">*Conferma modifiche*</span><span class="sxs-lookup"><span data-stu-id="73410-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="73410-474">Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="73410-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="73410-475">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="73410-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="73410-476">Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="73410-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="73410-477">![Pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "Pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="73410-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="73410-478">*Pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="73410-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="73410-479">Importare il profilo di pubblicazione salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="73410-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="73410-480">![Importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="73410-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="73410-481">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="73410-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="73410-482">Fare clic su **convalida connessione**.</span><span class="sxs-lookup"><span data-stu-id="73410-482">Click **Validate Connection**.</span></span> <span data-ttu-id="73410-483">Al termine della convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="73410-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="73410-484">La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="73410-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="73410-485">![Convalida della connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "Convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="73410-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="73410-486">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="73410-486">*Validating connection*</span></span>
4. <span data-ttu-id="73410-487">Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.</span><span class="sxs-lookup"><span data-stu-id="73410-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="73410-488">![Configurazione distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configurazione distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="73410-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="73410-489">*Configurazione distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="73410-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="73410-490">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="73410-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="73410-491">In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="73410-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="73410-492">In **nome utente** Digitare il nome di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="73410-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="73410-493">In **password** Digitare la password di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="73410-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="73410-494">Digitare un nuovo nome di database.</span><span class="sxs-lookup"><span data-stu-id="73410-494">Type a new database name.</span></span>

     <span data-ttu-id="73410-495">![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurazione della stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="73410-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="73410-496">*Configurazione della stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="73410-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="73410-497">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="73410-497">Then click **OK**.</span></span> <span data-ttu-id="73410-498">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="73410-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="73410-499">![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="73410-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="73410-500">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="73410-500">*Creating the database*</span></span>
7. <span data-ttu-id="73410-501">La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita).</span><span class="sxs-lookup"><span data-stu-id="73410-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="73410-502">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="73410-502">Then click **Next**.</span></span>

    <span data-ttu-id="73410-503">![Stringa di connessione che punta al database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "Stringa di connessione che punta al database SQL")</span><span class="sxs-lookup"><span data-stu-id="73410-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="73410-504">*Stringa di connessione che punta al database SQL*</span><span class="sxs-lookup"><span data-stu-id="73410-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="73410-505">Nella pagina **Anteprima** fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="73410-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="73410-506">![Pubblicazione dell'applicazione Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Pubblicazione dell'applicazione Web")</span><span class="sxs-lookup"><span data-stu-id="73410-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="73410-507">*Pubblicazione dell'applicazione Web*</span><span class="sxs-lookup"><span data-stu-id="73410-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="73410-508">Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="73410-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="73410-509">Appendice C: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="73410-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="73410-510">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="73410-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="73410-511">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="73410-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="73410-512">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="73410-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="73410-513">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="73410-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="73410-514">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="73410-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="73410-515">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="73410-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="73410-516">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="73410-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="73410-517">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="73410-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="73410-518">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="73410-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="73410-519">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="73410-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="73410-520">![Inizia a digitare il nome del frammento](aspnet-mvc-4-models-and-data-access/_static/image52.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="73410-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="73410-521">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="73410-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="73410-522">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-models-and-data-access/_static/image53.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="73410-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="73410-523">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="73410-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="73410-524">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image54.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="73410-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="73410-525">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="73410-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="73410-526">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="73410-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="73410-527">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="73410-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="73410-528">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="73410-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="73410-529">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="73410-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="73410-530">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-models-and-data-access/_static/image55.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="73410-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="73410-531">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="73410-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="73410-532">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="73410-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="73410-533">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="73410-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
