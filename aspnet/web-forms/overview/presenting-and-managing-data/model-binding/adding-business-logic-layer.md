---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Aggiunta del livello di logica di business a un progetto che utilizza l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634834"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="4768e-104">Aggiunta del livello di logica di business a un progetto che utilizza l'associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="4768e-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="4768e-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4768e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4768e-106">Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4768e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4768e-107">L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="4768e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4768e-108">Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="4768e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4768e-109">Questa esercitazione illustra come usare l'associazione di modelli con un livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="4768e-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="4768e-110">Si imposterà il membro OnCallingDataMethods per specificare che per chiamare i metodi dati verrà usato un oggetto diverso dalla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="4768e-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="4768e-111">Questa esercitazione si basa sul progetto creato nelle parti [precedenti](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="4768e-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4768e-112">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB.</span><span class="sxs-lookup"><span data-stu-id="4768e-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4768e-113">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4768e-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4768e-114">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="4768e-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="4768e-115">Elementi da compilare</span><span class="sxs-lookup"><span data-stu-id="4768e-115">What you'll build</span></span>

<span data-ttu-id="4768e-116">L'associazione di modelli consente di inserire il codice di interazione dei dati nel file code-behind per una pagina Web o in una classe della logica di business separata.</span><span class="sxs-lookup"><span data-stu-id="4768e-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="4768e-117">Nelle esercitazioni precedenti è stato illustrato come usare i file code-behind per il codice di interazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="4768e-118">Questo approccio funziona per i siti di piccole dimensioni, ma può comportare la ripetizione del codice e una maggiore difficoltà quando si gestisce un sito di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="4768e-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="4768e-119">Può anche essere molto difficile testare a livello di codice il codice che risiede nei file code-behind perché non esiste un livello di astrazione.</span><span class="sxs-lookup"><span data-stu-id="4768e-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="4768e-120">Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contenga tutta la logica per interagire con i dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="4768e-121">Il livello della logica di business viene quindi chiamato dalle pagine Web.</span><span class="sxs-lookup"><span data-stu-id="4768e-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="4768e-122">In questa esercitazione viene illustrato come spostare tutto il codice scritto nelle esercitazioni precedenti in un livello di logica di business e quindi utilizzare tale codice dalle pagine.</span><span class="sxs-lookup"><span data-stu-id="4768e-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="4768e-123">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="4768e-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4768e-124">Spostare il codice dai file code-behind a un livello della logica di business</span><span class="sxs-lookup"><span data-stu-id="4768e-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="4768e-125">Modificare i controlli associati a dati per chiamare i metodi nel livello della logica di business</span><span class="sxs-lookup"><span data-stu-id="4768e-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="4768e-126">Crea livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="4768e-126">Create business logic layer</span></span>

<span data-ttu-id="4768e-127">A questo punto, si creerà la classe che viene chiamata dalle pagine Web.</span><span class="sxs-lookup"><span data-stu-id="4768e-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="4768e-128">I metodi di questa classe hanno un aspetto simile ai metodi usati nelle esercitazioni precedenti e includono gli attributi del provider di valori.</span><span class="sxs-lookup"><span data-stu-id="4768e-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="4768e-129">In primo luogo, aggiungere una nuova cartella denominata **BLL**.</span><span class="sxs-lookup"><span data-stu-id="4768e-129">First, add a new folder called **BLL**.</span></span>

![Aggiungi cartella](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="4768e-131">Nella cartella BLL creare una nuova classe denominata **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="4768e-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="4768e-132">Conterrà tutte le operazioni sui dati che in origine risiedevano nei file code-behind.</span><span class="sxs-lookup"><span data-stu-id="4768e-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="4768e-133">I metodi sono quasi identici a quelli del file code-behind, ma includeranno alcune modifiche.</span><span class="sxs-lookup"><span data-stu-id="4768e-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="4768e-134">La modifica più importante da notare è che non è più in esecuzione il codice dall'interno di un'istanza della classe **Page** .</span><span class="sxs-lookup"><span data-stu-id="4768e-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="4768e-135">La classe Page contiene il metodo **TryUpdateModel** e la proprietà **ModelState** .</span><span class="sxs-lookup"><span data-stu-id="4768e-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="4768e-136">Quando questo codice viene spostato in un livello di logica di business, non è più disponibile un'istanza della classe di pagina per chiamare questi membri.</span><span class="sxs-lookup"><span data-stu-id="4768e-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="4768e-137">Per aggirare questo problema, è necessario aggiungere un parametro **ModelMethodContext** a qualsiasi metodo che accede a TryUpdateModel o ModelState.</span><span class="sxs-lookup"><span data-stu-id="4768e-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="4768e-138">Usare questo parametro ModelMethodContext per chiamare TryUpdateModel o recuperare ModelState.</span><span class="sxs-lookup"><span data-stu-id="4768e-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="4768e-139">Non è necessario modificare alcun elemento nella pagina Web per tenere conto di questo nuovo parametro.</span><span class="sxs-lookup"><span data-stu-id="4768e-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="4768e-140">Sostituire il codice in SchoolBL.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="4768e-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="4768e-141">Rivedere le pagine esistenti per recuperare i dati dal livello della logica di business</span><span class="sxs-lookup"><span data-stu-id="4768e-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="4768e-142">Infine, si convertiranno le pagine students. aspx, AddStudent. aspx e courses. aspx dall'utilizzo di query nel file code-behind per l'utilizzo del livello della logica di business.</span><span class="sxs-lookup"><span data-stu-id="4768e-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="4768e-143">Nei file code-behind per Students, AddStudent e Courses, eliminare o impostare come commento i metodi di query seguenti:</span><span class="sxs-lookup"><span data-stu-id="4768e-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="4768e-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4768e-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="4768e-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="4768e-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="4768e-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="4768e-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="4768e-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="4768e-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="4768e-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="4768e-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="4768e-149">Nel file code-behind non dovrebbe essere presente codice che riguarda le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="4768e-150">Il gestore dell'evento **OnCallingDataMethods** consente di specificare un oggetto da utilizzare per i metodi di dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="4768e-151">In students. aspx aggiungere un valore per il gestore eventi e modificare i nomi dei metodi dei dati con i nomi dei metodi nella classe della logica di business.</span><span class="sxs-lookup"><span data-stu-id="4768e-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="4768e-152">Nel file code-behind per Students. aspx definire il gestore eventi per l'evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="4768e-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="4768e-153">In questo gestore eventi specificare la classe della logica di business per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="4768e-154">In AddStudent. aspx apportare modifiche simili.</span><span class="sxs-lookup"><span data-stu-id="4768e-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="4768e-155">In courses. aspx apportare modifiche simili.</span><span class="sxs-lookup"><span data-stu-id="4768e-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="4768e-156">Eseguire l'applicazione e notare che tutte le pagine funzionano come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4768e-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="4768e-157">La logica di convalida funziona anche correttamente.</span><span class="sxs-lookup"><span data-stu-id="4768e-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4768e-158">Conclusione</span><span class="sxs-lookup"><span data-stu-id="4768e-158">Conclusion</span></span>

<span data-ttu-id="4768e-159">In questa esercitazione è stata ristrutturata l'applicazione in modo da usare un livello di accesso ai dati e un livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="4768e-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="4768e-160">È stato specificato che i controlli dati utilizzano un oggetto che non è la pagina corrente per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="4768e-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4768e-161">Precedente</span><span class="sxs-lookup"><span data-stu-id="4768e-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
