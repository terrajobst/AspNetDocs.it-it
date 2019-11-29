---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633179"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="495ec-104">Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="495ec-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="495ec-105">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="495ec-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="495ec-106">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="495ec-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="495ec-107">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="495ec-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="495ec-108">Il modello di associazione di modelli funziona con qualsiasi tecnologia di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="495ec-109">In questa esercitazione si utilizzerà Entity Framework, ma è possibile utilizzare la tecnologia di accesso ai dati più familiare.</span><span class="sxs-lookup"><span data-stu-id="495ec-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="495ec-110">Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView o FormView, è possibile specificare i nomi dei metodi da utilizzare per la selezione, l'aggiornamento, l'eliminazione e la creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="495ec-111">In questa esercitazione verrà specificato un valore per SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="495ec-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="495ec-112">All'interno di questo metodo, viene fornita la logica per il recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="495ec-113">Nell'esercitazione successiva si imposteranno i valori per UpdateMethod, DeleteMethod e InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="495ec-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="495ec-114">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="495ec-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="495ec-115">Il codice scaricabile funziona con Visual Studio 2012 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="495ec-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="495ec-116">Usa il modello di Visual Studio 2012, leggermente diverso rispetto al modello di Visual Studio 2017 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="495ec-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="495ec-117">Nell'esercitazione viene eseguita l'applicazione in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="495ec-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="495ec-118">È inoltre possibile distribuire l'applicazione a un provider di hosting e renderla disponibile tramite Internet.</span><span class="sxs-lookup"><span data-stu-id="495ec-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="495ec-119">Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un</span><span class="sxs-lookup"><span data-stu-id="495ec-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="495ec-120">[account di valutazione gratuito di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="495ec-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="495ec-121">Per informazioni su come distribuire un progetto Web di Visual Studio per app Azure app Web del servizio, vedere la pagina relativa alla [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series.</span><span class="sxs-lookup"><span data-stu-id="495ec-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="495ec-122">Questa esercitazione illustra anche come usare Migrazioni Code First di Entity Framework per distribuire il database SQL Server nel database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="495ec-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="495ec-123">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="495ec-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="495ec-124">Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="495ec-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="495ec-125">Questa esercitazione funziona anche con Visual Studio 2012 e Visual Studio 2013, ma esistono alcune differenze nell'interfaccia utente e nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="495ec-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="495ec-126">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="495ec-126">What you'll build</span></span>

<span data-ttu-id="495ec-127">In questa esercitazione verranno illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="495ec-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="495ec-128">Creare oggetti dati che riflettono un'università con studenti iscritti a corsi</span><span class="sxs-lookup"><span data-stu-id="495ec-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="495ec-129">Compilare le tabelle di database dagli oggetti</span><span class="sxs-lookup"><span data-stu-id="495ec-129">Build database tables from the objects</span></span>
* <span data-ttu-id="495ec-130">Popola il database con i dati di test</span><span class="sxs-lookup"><span data-stu-id="495ec-130">Populate the database with test data</span></span>
* <span data-ttu-id="495ec-131">Visualizzare i dati in un Web Form</span><span class="sxs-lookup"><span data-stu-id="495ec-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="495ec-132">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="495ec-132">Create the project</span></span>

1. <span data-ttu-id="495ec-133">In Visual Studio 2017 creare un progetto di **applicazione Web ASP.NET (.NET Framework)** denominato **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="495ec-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Crea progetto](retrieving-data/_static/image19.png)

2. <span data-ttu-id="495ec-135">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="495ec-135">Select **OK**.</span></span> <span data-ttu-id="495ec-136">Verrà visualizzata la finestra di dialogo per la selezione di un modello.</span><span class="sxs-lookup"><span data-stu-id="495ec-136">The dialog box to select a template appears.</span></span>

   ![seleziona Web Form](retrieving-data/_static/image3.png)

3. <span data-ttu-id="495ec-138">Selezionare il modello **Web Form** .</span><span class="sxs-lookup"><span data-stu-id="495ec-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="495ec-139">Se necessario, modificare l'autenticazione per **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="495ec-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="495ec-140">Selezionare **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="495ec-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="495ec-141">Modificare l'aspetto del sito</span><span class="sxs-lookup"><span data-stu-id="495ec-141">Modify site appearance</span></span>

   <span data-ttu-id="495ec-142">Apportare alcune modifiche per personalizzare l'aspetto del sito.</span><span class="sxs-lookup"><span data-stu-id="495ec-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="495ec-143">Aprire il file site. master.</span><span class="sxs-lookup"><span data-stu-id="495ec-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="495ec-144">Modificare il titolo per visualizzare **Contoso University** e non l' **applicazione ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="495ec-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="495ec-145">Modificare il testo dell'intestazione da **nome applicazione** a **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="495ec-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="495ec-146">Modificare l'intestazione di navigazione collegamenti a quelli appropriati per il sito.</span><span class="sxs-lookup"><span data-stu-id="495ec-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="495ec-147">Rimuovere i collegamenti per **informazioni su** e **contatto** e, al contrario, collegarsi a una pagina **students** , che sarà creata.</span><span class="sxs-lookup"><span data-stu-id="495ec-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="495ec-148">Salvare site. master.</span><span class="sxs-lookup"><span data-stu-id="495ec-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="495ec-149">Aggiungere un Web Form per visualizzare i dati degli studenti</span><span class="sxs-lookup"><span data-stu-id="495ec-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="495ec-150">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi** , quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="495ec-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="495ec-151">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il **Web Form con il modello di pagina master** e denominarlo **students. aspx**.</span><span class="sxs-lookup"><span data-stu-id="495ec-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Crea pagina](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="495ec-153">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="495ec-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="495ec-154">Per la pagina master del Web Form selezionare **site. master**.</span><span class="sxs-lookup"><span data-stu-id="495ec-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="495ec-155">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="495ec-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="495ec-156">Aggiungere il modello di dati</span><span class="sxs-lookup"><span data-stu-id="495ec-156">Add the data model</span></span>

<span data-ttu-id="495ec-157">Nella cartella **Models** aggiungere una classe denominata **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="495ec-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="495ec-158">Fare clic con il pulsante destro del mouse su **modelli**, scegliere **Aggiungi**, quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="495ec-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="495ec-159">Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="495ec-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="495ec-160">Nel menu di spostamento a sinistra selezionare **codice**, quindi **classe**.</span><span class="sxs-lookup"><span data-stu-id="495ec-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Crea classe modello](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="495ec-162">Assegnare alla classe il nome **UniversityModels.cs** e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="495ec-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="495ec-163">In questo file definire le classi `SchoolContext`, `Student`, `Enrollment`e `Course` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="495ec-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="495ec-164">La classe `SchoolContext` deriva da `DbContext`, che gestisce la connessione al database e le modifiche apportate ai dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="495ec-165">Nella classe `Student` si notino gli attributi applicati alle proprietà `FirstName`, `LastName`e `Year`.</span><span class="sxs-lookup"><span data-stu-id="495ec-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="495ec-166">Questa esercitazione usa questi attributi per la convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="495ec-167">Per semplificare il codice, solo queste proprietà sono contrassegnate con gli attributi di convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="495ec-168">In un progetto reale, si applicano gli attributi di convalida a tutte le proprietà che richiedono la convalida.</span><span class="sxs-lookup"><span data-stu-id="495ec-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="495ec-169">Salvare UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="495ec-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="495ec-170">Configurare il database in base alle classi</span><span class="sxs-lookup"><span data-stu-id="495ec-170">Set up the database based on classes</span></span>

<span data-ttu-id="495ec-171">Questa esercitazione USA [migrazioni Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) per creare oggetti e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="495ec-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="495ec-172">Queste tabelle archiviano informazioni sugli studenti e sui relativi corsi.</span><span class="sxs-lookup"><span data-stu-id="495ec-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="495ec-173">Selezionare **strumenti** > **gestione pacchetti NuGet** > **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="495ec-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="495ec-174">Nella **console di gestione pacchetti**eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="495ec-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="495ec-175">Se il comando viene completato correttamente, viene visualizzato un messaggio che indica che sono state abilitate le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="495ec-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Abilita migrazioni](retrieving-data/_static/image8.png)

      <span data-ttu-id="495ec-177">Si noti che è stato creato un file denominato *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="495ec-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="495ec-178">La classe `Configuration` dispone di un metodo `Seed`, che può prepopolare le tabelle di database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="495ec-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="495ec-179">Popolamento preliminare del database</span><span class="sxs-lookup"><span data-stu-id="495ec-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="495ec-180">Aprire Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="495ec-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="495ec-181">Aggiungere al metodo `Seed` il seguente codice.</span><span class="sxs-lookup"><span data-stu-id="495ec-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="495ec-182">Aggiungere inoltre un'istruzione `using` per lo spazio dei nomi `ContosoUniversityModelBinding. Models`.</span><span class="sxs-lookup"><span data-stu-id="495ec-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="495ec-183">Salvare Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="495ec-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="495ec-184">Nella console di gestione pacchetti eseguire il comando **Add-Migration Initial**.</span><span class="sxs-lookup"><span data-stu-id="495ec-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="495ec-185">Eseguire il comando **Update-database**.</span><span class="sxs-lookup"><span data-stu-id="495ec-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="495ec-186">Se si riceve un'eccezione durante l'esecuzione di questo comando, i valori `StudentID` e `CourseID` potrebbero essere diversi dai valori del metodo di `Seed`.</span><span class="sxs-lookup"><span data-stu-id="495ec-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="495ec-187">Aprire le tabelle di database e individuare i valori esistenti per `StudentID` e `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="495ec-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="495ec-188">Aggiungere tali valori al codice per il seeding della tabella `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="495ec-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="495ec-189">Aggiungere un controllo GridView</span><span class="sxs-lookup"><span data-stu-id="495ec-189">Add a GridView control</span></span>

<span data-ttu-id="495ec-190">Con i dati del database popolato, ora è possibile recuperare i dati e visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="495ec-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="495ec-191">Aprire students. aspx.</span><span class="sxs-lookup"><span data-stu-id="495ec-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="495ec-192">Individuare il segnaposto `MainContent`.</span><span class="sxs-lookup"><span data-stu-id="495ec-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="495ec-193">All'interno di tale segnaposto, aggiungere un controllo **GridView** che includa questo codice.</span><span class="sxs-lookup"><span data-stu-id="495ec-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="495ec-194">Aspetti da considerare:</span><span class="sxs-lookup"><span data-stu-id="495ec-194">Things to note:</span></span>
   * <span data-ttu-id="495ec-195">Si noti il valore impostato per la proprietà `SelectMethod` nell'elemento GridView.</span><span class="sxs-lookup"><span data-stu-id="495ec-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="495ec-196">Questo valore specifica il metodo utilizzato per recuperare i dati GridView, che vengono creati nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="495ec-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="495ec-197">La proprietà `ItemType` è impostata sulla classe `Student` creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="495ec-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="495ec-198">Questa impostazione consente di fare riferimento alle proprietà della classe nel markup.</span><span class="sxs-lookup"><span data-stu-id="495ec-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="495ec-199">Ad esempio, la classe `Student` dispone di una raccolta denominata `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="495ec-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="495ec-200">È possibile usare `Item.Enrollments` per recuperare la raccolta e quindi usare la [sintassi LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) per recuperare la somma dei crediti registrati di ogni studente.</span><span class="sxs-lookup"><span data-stu-id="495ec-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="495ec-201">Salvare students. aspx.</span><span class="sxs-lookup"><span data-stu-id="495ec-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="495ec-202">Aggiungere codice per recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="495ec-202">Add code to retrieve data</span></span>

   <span data-ttu-id="495ec-203">Nel file code-behind students. aspx aggiungere il metodo specificato per il valore `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="495ec-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="495ec-204">Aprire Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="495ec-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="495ec-205">Aggiungere istruzioni `using` per gli spazi dei nomi `ContosoUniversityModelBinding. Models` e `System.Data.Entity`.</span><span class="sxs-lookup"><span data-stu-id="495ec-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="495ec-206">Aggiungere il metodo specificato per `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="495ec-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="495ec-207">La clausola `Include` migliora le prestazioni delle query, ma non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="495ec-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="495ec-208">Senza la clausola `Include`, i dati vengono recuperati utilizzando il [*caricamento lazy*](https://en.wikipedia.org/wiki/Lazy_loading), che prevede l'invio di una query separata al database ogni volta che i dati correlati vengono recuperati.</span><span class="sxs-lookup"><span data-stu-id="495ec-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="495ec-209">Con la clausola `Include`, i dati vengono recuperati utilizzando il *caricamento eager*, il che significa che una singola query di database recupera tutti i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="495ec-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="495ec-210">Se non si usano dati correlati, il caricamento eager è meno efficiente perché vengono recuperati più dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="495ec-211">Tuttavia, in questo caso, il caricamento eager offre le migliori prestazioni perché i dati correlati vengono visualizzati per ogni record.</span><span class="sxs-lookup"><span data-stu-id="495ec-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="495ec-212">Per ulteriori informazioni sulle considerazioni relative alle prestazioni per il caricamento di dati correlati, vedere la sezione **Lazy, eager e caricamento esplicito di dati correlati** nell'articolo [relativo alla lettura dei dati correlati con l'Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="495ec-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="495ec-213">Per impostazione predefinita, i dati vengono ordinati in base ai valori della proprietà contrassegnata come chiave.</span><span class="sxs-lookup"><span data-stu-id="495ec-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="495ec-214">È possibile aggiungere una clausola `OrderBy` per specificare un valore di ordinamento diverso.</span><span class="sxs-lookup"><span data-stu-id="495ec-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="495ec-215">In questo esempio, la proprietà `StudentID` predefinita viene utilizzata per l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="495ec-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="495ec-216">Nell'articolo [ordinamento, paging e filtro dei dati](sorting-paging-and-filtering-data.md) l'utente è abilitato per selezionare una colonna per l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="495ec-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="495ec-217">Salvare Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="495ec-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="495ec-218">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="495ec-218">Run your application</span></span> 

<span data-ttu-id="495ec-219">Eseguire l'applicazione Web (**F5**) e passare alla pagina **students (studenti** ), che visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="495ec-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Mostra dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="495ec-221">Generazione automatica di metodi di associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="495ec-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="495ec-222">Quando si usa questa serie di esercitazioni, è possibile copiare semplicemente il codice dall'esercitazione al progetto.</span><span class="sxs-lookup"><span data-stu-id="495ec-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="495ec-223">Tuttavia, uno svantaggio di questo approccio è che è possibile che non si sia a conoscenza della funzionalità fornita da Visual Studio per generare automaticamente il codice per i metodi di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="495ec-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="495ec-224">Quando si lavora su progetti personalizzati, la generazione automatica del codice consente di risparmiare tempo e di ottenere informazioni su come implementare un'operazione.</span><span class="sxs-lookup"><span data-stu-id="495ec-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="495ec-225">In questa sezione viene descritta la funzionalità di generazione automatica del codice.</span><span class="sxs-lookup"><span data-stu-id="495ec-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="495ec-226">Questa sezione è solo informativa e non contiene codice che è necessario implementare nel progetto.</span><span class="sxs-lookup"><span data-stu-id="495ec-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="495ec-227">Quando si imposta un valore per le proprietà `SelectMethod`, `UpdateMethod`, `InsertMethod`o `DeleteMethod` nel codice di markup, è possibile selezionare l'opzione **Crea nuovo metodo** .</span><span class="sxs-lookup"><span data-stu-id="495ec-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![creazione di un metodo](retrieving-data/_static/image18.png)

<span data-ttu-id="495ec-229">Visual Studio non crea solo un metodo nel code-behind con la firma corretta, ma genera anche il codice di implementazione per eseguire l'operazione.</span><span class="sxs-lookup"><span data-stu-id="495ec-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="495ec-230">Se si imposta prima la proprietà `ItemType` prima di utilizzare la funzionalità di generazione automatica del codice, il codice generato utilizzerà tale tipo per le operazioni.</span><span class="sxs-lookup"><span data-stu-id="495ec-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="495ec-231">Ad esempio, quando si imposta la proprietà `UpdateMethod`, viene generato automaticamente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="495ec-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="495ec-232">Anche in questo caso non è necessario aggiungere il codice al progetto.</span><span class="sxs-lookup"><span data-stu-id="495ec-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="495ec-233">Nell'esercitazione successiva verranno implementati i metodi per l'aggiornamento, l'eliminazione e l'aggiunta di nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="495ec-234">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="495ec-234">Summary</span></span>

<span data-ttu-id="495ec-235">In questa esercitazione sono state create classi di modelli di dati e un database è stato generato da tali classi.</span><span class="sxs-lookup"><span data-stu-id="495ec-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="495ec-236">Sono state compilate le tabelle di database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="495ec-236">You filled the database tables with test data.</span></span> <span data-ttu-id="495ec-237">È stata utilizzata l'associazione di modelli per recuperare i dati dal database e quindi sono stati visualizzati i dati in GridView.</span><span class="sxs-lookup"><span data-stu-id="495ec-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="495ec-238">Nell' [esercitazione](updating-deleting-and-creating-data.md) successiva di questa serie sarà possibile abilitare l'aggiornamento, l'eliminazione e la creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="495ec-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="495ec-239">avanti</span><span class="sxs-lookup"><span data-stu-id="495ec-239">Next</span></span>](updating-deleting-and-creating-data.md)
