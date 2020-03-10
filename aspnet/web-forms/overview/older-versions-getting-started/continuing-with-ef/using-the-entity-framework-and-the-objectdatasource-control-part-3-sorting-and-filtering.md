---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Uso della Entity Framework 4,0 e del controllo ObjectDataSource, parte 3: ordinamento e filtro | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631670"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="a88c7-104">Uso della Entity Framework 4,0 e del controllo ObjectDataSource, parte 3: ordinamento e filtro</span><span class="sxs-lookup"><span data-stu-id="a88c7-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="a88c7-105">di [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a88c7-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a88c7-106">Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni Entity Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="a88c7-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="a88c7-107">Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata.</span><span class="sxs-lookup"><span data-stu-id="a88c7-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="a88c7-108">È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete.</span><span class="sxs-lookup"><span data-stu-id="a88c7-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="a88c7-109">In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="a88c7-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="a88c7-110">Nell'esercitazione precedente è stato implementato il modello di repository in un'applicazione Web a più livelli che usa il Entity Framework e il controllo `ObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="a88c7-111">In questa esercitazione viene illustrato come eseguire operazioni di ordinamento e filtro e gestire scenari Master-Detail.</span><span class="sxs-lookup"><span data-stu-id="a88c7-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="a88c7-112">Verranno aggiunti i miglioramenti seguenti alla pagina *Departments. aspx* :</span><span class="sxs-lookup"><span data-stu-id="a88c7-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="a88c7-113">Casella di testo che consente agli utenti di selezionare i reparti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="a88c7-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="a88c7-114">Elenco di corsi per ogni reparto visualizzato nella griglia.</span><span class="sxs-lookup"><span data-stu-id="a88c7-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="a88c7-115">Possibilità di ordinare facendo clic sulle intestazioni di colonna.</span><span class="sxs-lookup"><span data-stu-id="a88c7-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="a88c7-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a88c7-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="a88c7-117">Aggiunta della possibilità di ordinare le colonne GridView</span><span class="sxs-lookup"><span data-stu-id="a88c7-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="a88c7-118">Aprire la pagina *Departments. aspx* e aggiungere un attributo `SortParameterName="sortExpression"` al controllo `ObjectDataSource` denominato `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="a88c7-119">In un secondo momento verrà creato un `GetDepartments` metodo che accetta un parametro denominato `sortExpression`. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="a88c7-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="a88c7-120">Aggiungere l'attributo `AllowSorting="true"` al tag di apertura del controllo `GridView`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="a88c7-121">Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="a88c7-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="a88c7-122">In *Departments.aspx.cs*impostare l'ordinamento predefinito chiamando il metodo di `Sort` del controllo `GridView` dal metodo `Page_Load`:</span><span class="sxs-lookup"><span data-stu-id="a88c7-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="a88c7-123">È possibile aggiungere codice che ordina o filtra nella classe della logica di business o nella classe del repository.</span><span class="sxs-lookup"><span data-stu-id="a88c7-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="a88c7-124">Se si esegue questa operazione nella classe della logica di business, le operazioni di ordinamento o filtro verranno eseguite dopo che i dati vengono recuperati dal database, perché la classe della logica di business sta utilizzando un oggetto `IEnumerable` restituito dal repository.</span><span class="sxs-lookup"><span data-stu-id="a88c7-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="a88c7-125">Se si aggiunge codice di ordinamento e filtro nella classe del repository e si esegue questa operazione prima che un'espressione LINQ o una query di oggetto venga convertita in un oggetto `IEnumerable`, i comandi verranno passati al database per l'elaborazione, che in genere è più efficiente.</span><span class="sxs-lookup"><span data-stu-id="a88c7-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="a88c7-126">In questa esercitazione verrà implementato l'ordinamento e il filtro in modo che l'elaborazione venga eseguita dal database, ovvero nel repository.</span><span class="sxs-lookup"><span data-stu-id="a88c7-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="a88c7-127">Per aggiungere la funzionalità di ordinamento, è necessario aggiungere un nuovo metodo all'interfaccia del repository e alle classi del repository, nonché alla classe della logica di business.</span><span class="sxs-lookup"><span data-stu-id="a88c7-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="a88c7-128">Nel file *ISchoolRepository.cs* aggiungere un nuovo `GetDepartments` metodo che accetta un parametro di `sortExpression` che verrà usato per ordinare l'elenco dei reparti restituiti:</span><span class="sxs-lookup"><span data-stu-id="a88c7-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="a88c7-129">Il parametro `sortExpression` specifica la colonna in cui eseguire l'ordinamento e la direzione di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="a88c7-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="a88c7-130">Aggiungere il codice per il nuovo metodo al file *SchoolRepository.cs* :</span><span class="sxs-lookup"><span data-stu-id="a88c7-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="a88c7-131">Modificare il metodo `GetDepartments` senza parametri esistente per chiamare il nuovo metodo:</span><span class="sxs-lookup"><span data-stu-id="a88c7-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="a88c7-132">Nel progetto di test aggiungere il nuovo metodo seguente a *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="a88c7-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="a88c7-133">Se si intende creare unit test che dipendono da questo metodo che restituisce un elenco ordinato, è necessario ordinare l'elenco prima di restituirlo.</span><span class="sxs-lookup"><span data-stu-id="a88c7-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="a88c7-134">In questa esercitazione non verranno creati test, quindi il metodo può solo restituire l'elenco non ordinato dei reparti.</span><span class="sxs-lookup"><span data-stu-id="a88c7-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="a88c7-135">Nel file *SchoolBL.cs* aggiungere il nuovo metodo seguente alla classe della logica di business:</span><span class="sxs-lookup"><span data-stu-id="a88c7-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="a88c7-136">Questo codice passa il parametro sort al metodo del repository.</span><span class="sxs-lookup"><span data-stu-id="a88c7-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="a88c7-137">Eseguire la pagina *repartitions. aspx* .</span><span class="sxs-lookup"><span data-stu-id="a88c7-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="a88c7-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a88c7-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="a88c7-139">È ora possibile fare clic su qualsiasi intestazione di colonna per ordinare in base a tale colonna.</span><span class="sxs-lookup"><span data-stu-id="a88c7-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="a88c7-140">Se la colonna è già ordinata, facendo clic sull'intestazione viene invertita la direzione di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="a88c7-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="a88c7-141">Aggiunta di una casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="a88c7-141">Adding a Search Box</span></span>

<span data-ttu-id="a88c7-142">In questa sezione si aggiungerà una casella di testo di ricerca, lo si collegherà al controllo `ObjectDataSource` usando un parametro di controllo e si aggiungerà un metodo alla classe della logica di business per supportare l'applicazione di filtri.</span><span class="sxs-lookup"><span data-stu-id="a88c7-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="a88c7-143">Aprire la pagina *Departments. aspx* e aggiungere il markup seguente tra l'intestazione e il primo controllo `ObjectDataSource`:</span><span class="sxs-lookup"><span data-stu-id="a88c7-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="a88c7-144">Nel controllo `ObjectDataSource` denominato `DepartmentsObjectDataSource`eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a88c7-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="a88c7-145">Aggiungere un elemento `SelectParameters` per un parametro denominato `nameSearchString` che ottiene il valore immesso nel controllo `SearchTextBox`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="a88c7-146">Modificare il valore dell'attributo `SelectMethod` in `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="a88c7-147">Questo metodo verrà creato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="a88c7-147">(You'll create this method later.)</span></span>

<span data-ttu-id="a88c7-148">Il markup per il controllo `ObjectDataSource` ora è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a88c7-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="a88c7-149">In *ISchoolRepository.cs*aggiungere un `GetDepartmentsByName` metodo che accetta i parametri `sortExpression` e `nameSearchString`:</span><span class="sxs-lookup"><span data-stu-id="a88c7-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="a88c7-150">In *SchoolRepository.cs*aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="a88c7-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="a88c7-151">Questo codice usa un metodo di `Where` per selezionare gli elementi che contengono la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="a88c7-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="a88c7-152">Se la stringa di ricerca è vuota, verranno selezionati tutti i record.</span><span class="sxs-lookup"><span data-stu-id="a88c7-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="a88c7-153">Si noti che quando si specificano le chiamate al metodo in un'unica istruzione come questa (`Include`, quindi `OrderBy`, quindi `Where`), il metodo `Where` deve essere sempre l'ultimo.</span><span class="sxs-lookup"><span data-stu-id="a88c7-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="a88c7-154">Modificare il metodo `GetDepartments` esistente che accetta un parametro di `sortExpression` per chiamare il nuovo metodo:</span><span class="sxs-lookup"><span data-stu-id="a88c7-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="a88c7-155">In *MockSchoolRepository.cs* nel progetto di test aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="a88c7-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="a88c7-156">In *SchoolBL.cs*aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="a88c7-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="a88c7-157">Eseguire la pagina *repartitions. aspx* e immettere una stringa di ricerca per assicurarsi che la logica di selezione funzioni.</span><span class="sxs-lookup"><span data-stu-id="a88c7-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="a88c7-158">Lasciare vuota la casella di testo e provare a eseguire una ricerca per assicurarsi che vengano restituiti tutti i record.</span><span class="sxs-lookup"><span data-stu-id="a88c7-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="a88c7-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a88c7-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="a88c7-160">Aggiunta di una colonna dettagli per ogni riga della griglia</span><span class="sxs-lookup"><span data-stu-id="a88c7-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="a88c7-161">Successivamente, si desidera visualizzare tutti i corsi per ogni reparto visualizzato nella cella a destra della griglia.</span><span class="sxs-lookup"><span data-stu-id="a88c7-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="a88c7-162">A tale scopo, si utilizzerà un controllo `GridView` annidato e lo si aggiungerà ai dati della proprietà di navigazione `Courses` dell'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="a88c7-163">Aprire *repartitions. aspx* e nel markup per il controllo `GridView` specificare un gestore per l'evento `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="a88c7-164">Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="a88c7-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="a88c7-165">Aggiungere un nuovo elemento `TemplateField` dopo il campo modello `Administrator`:</span><span class="sxs-lookup"><span data-stu-id="a88c7-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="a88c7-166">Questo markup crea un controllo `GridView` annidato che mostra il numero di corso e il titolo di un elenco di corsi.</span><span class="sxs-lookup"><span data-stu-id="a88c7-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="a88c7-167">Non specifica un'origine dati perché verrà eseguita la relativa associazione nel codice del gestore `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="a88c7-168">Aprire *Departments.aspx.cs* e aggiungere il gestore seguente per l'evento `RowDataBound`:</span><span class="sxs-lookup"><span data-stu-id="a88c7-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="a88c7-169">Questo codice ottiene l'entità `Department` dagli argomenti dell'evento, converte la proprietà di navigazione `Courses` in una raccolta di `List` e DataBinding il `GridView` annidato alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="a88c7-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="a88c7-170">Aprire il file *SchoolRepository.cs* e specificare il caricamento eager per la proprietà di navigazione `Courses` chiamando il metodo `Include` nella query di oggetto creata nel metodo `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="a88c7-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="a88c7-171">L'istruzione `return` nel metodo `GetDepartmentsByName` ora è simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="a88c7-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="a88c7-172">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="a88c7-172">Run the page.</span></span> <span data-ttu-id="a88c7-173">Oltre alla funzionalità di ordinamento e filtro aggiunta in precedenza, il controllo GridView ora Mostra i dettagli dei corsi annidati per ogni reparto.</span><span class="sxs-lookup"><span data-stu-id="a88c7-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="a88c7-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a88c7-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="a88c7-175">Questa operazione completa l'introduzione a scenari di ordinamento, filtro e dettagli master.</span><span class="sxs-lookup"><span data-stu-id="a88c7-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="a88c7-176">Nell'esercitazione successiva si vedrà come gestire la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="a88c7-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a88c7-177">[Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="a88c7-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
