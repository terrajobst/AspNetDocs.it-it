---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557540"
---
# <a name="add-models-and-controllers"></a><span data-ttu-id="fcfb2-102">Aggiungere modelli e controller</span><span class="sxs-lookup"><span data-stu-id="fcfb2-102">Add Models and Controllers</span></span>

<span data-ttu-id="fcfb2-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fcfb2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="fcfb2-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="fcfb2-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="fcfb2-105">In questa sezione vengono aggiunte le classi del modello che definiscono le entità di database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="fcfb2-106">Verranno quindi aggiunti controller API Web che eseguono operazioni CRUD su tali entità.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="fcfb2-107">Aggiungi classi modello</span><span class="sxs-lookup"><span data-stu-id="fcfb2-107">Add Model Classes</span></span>

<span data-ttu-id="fcfb2-108">In questa esercitazione verrà creato il database usando l'approccio "Code First" per Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="fcfb2-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="fcfb2-109">Con Code First, è possibile C# scrivere classi che corrispondono a tabelle di database e EF crea il database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="fcfb2-110">Per ulteriori informazioni, vedere [Entity Framework approcci di sviluppo](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="fcfb2-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="fcfb2-111">Si inizia definendo gli oggetti di dominio come oggetti POCO (Plain-Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="fcfb2-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="fcfb2-112">Si creeranno le seguenti operazioni POCO:</span><span class="sxs-lookup"><span data-stu-id="fcfb2-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="fcfb2-113">Autore</span><span class="sxs-lookup"><span data-stu-id="fcfb2-113">Author</span></span>
- <span data-ttu-id="fcfb2-114">Book</span><span class="sxs-lookup"><span data-stu-id="fcfb2-114">Book</span></span>

<span data-ttu-id="fcfb2-115">In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="fcfb2-116">Selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="fcfb2-117">Assegnare alla classe il nome `Author`.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="fcfb2-118">Sostituire tutto il codice standard in Author.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="fcfb2-119">Aggiungere un'altra classe denominata `Book`con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="fcfb2-120">Entity Framework utilizzeranno questi modelli per creare tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="fcfb2-121">Per ogni modello, la proprietà `Id` diventerà la colonna chiave primaria della tabella di database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="fcfb2-122">Nella classe Book, il `AuthorId` definisce una chiave esterna nella tabella `Author`.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="fcfb2-123">Per semplicità, si presuppone che ogni libro abbia un singolo autore. La classe Book contiene anche una proprietà di navigazione per la `Author`correlata.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="fcfb2-124">È possibile utilizzare la proprietà di navigazione per accedere al `Author` correlato nel codice.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="fcfb2-125">Si aggiungono altre informazioni sulle proprietà di navigazione nella parte 4, [gestione delle relazioni tra entità](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="fcfb2-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="fcfb2-126">Aggiungere controller API Web</span><span class="sxs-lookup"><span data-stu-id="fcfb2-126">Add Web API Controllers</span></span>

<span data-ttu-id="fcfb2-127">In questa sezione verranno aggiunti controller API Web che supportano operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione).</span><span class="sxs-lookup"><span data-stu-id="fcfb2-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="fcfb2-128">I controller utilizzeranno Entity Framework per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="fcfb2-129">In primo luogo, è possibile eliminare i file Controllers/ValuesController. cs.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="fcfb2-130">Questo file contiene un esempio di controller API Web, ma non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="fcfb2-131">Successivamente, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-131">Next, build the project.</span></span> <span data-ttu-id="fcfb2-132">L'impalcatura dell'API Web usa la reflection per trovare le classi del modello, quindi richiede l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="fcfb2-133">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="fcfb2-134">Selezionare **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="fcfb2-135">Nella finestra di dialogo **Aggiungi impalcatura** selezionare "Web API 2 controller with actions, using Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="fcfb2-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="fcfb2-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="fcfb2-137">Nella finestra di dialogo **Aggiungi controller** eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fcfb2-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="fcfb2-138">Nell'elenco a discesa **classe modello** selezionare la classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="fcfb2-139">Se non viene visualizzato nell'elenco a discesa, assicurarsi di aver compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="fcfb2-140">Selezionare "use Async controller actions".</span><span class="sxs-lookup"><span data-stu-id="fcfb2-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="fcfb2-141">Lasciare il nome del controller come &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="fcfb2-142">Fare clic sul pulsante con il segno più (+) accanto alla **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="fcfb2-143">Nella finestra di dialogo **nuovo contesto dati** lasciare il nome predefinito e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="fcfb2-144">Fare clic su **Aggiungi** per completare la finestra di dialogo **Aggiungi controller** .</span><span class="sxs-lookup"><span data-stu-id="fcfb2-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="fcfb2-145">La finestra di dialogo aggiunge due classi al progetto:</span><span class="sxs-lookup"><span data-stu-id="fcfb2-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="fcfb2-146">`AuthorsController` definisce un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="fcfb2-147">Il controller implementa l'API REST usata dai client per eseguire operazioni CRUD nell'elenco di autori.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="fcfb2-148">`BookServiceContext` gestisce gli oggetti entità in fase di esecuzione, che include il popolamento di oggetti con dati da un database, il rilevamento delle modifiche e il salvataggio permanente dei dati nel database.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="fcfb2-149">Eredita da `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="fcfb2-150">A questo punto, compilare nuovamente il progetto.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-150">At this point, build the project again.</span></span> <span data-ttu-id="fcfb2-151">Eseguire ora la stessa procedura per aggiungere un controller API per le entità `Book`.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="fcfb2-152">Questa volta selezionare `Book` per la classe modello e selezionare la classe `BookServiceContext` esistente per la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="fcfb2-153">Non creare un nuovo contesto dati. Fare clic su **Aggiungi** per aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="fcfb2-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="fcfb2-154">[Precedente](part-1.md)
> [Successivo](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="fcfb2-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
