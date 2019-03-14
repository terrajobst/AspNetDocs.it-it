---
title: 'Esercitazione: Aggiungere funzionalità di ordinamento, filtro e suddivisione in pagine - ASP.NET MVC con EF Core'
description: In questa esercitazione si aggiungeranno le funzionalità di ordinamento, filtro e suddivisione in pagine alla pagina Student Index (Indice degli studenti). Verrà anche creata una pagina che esegue il raggruppamento semplice.
author: rick-anderson
ms.author: tdykstra
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 51b6b08d2410652f93427371aec299eb4c8789f1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044628"
---
# <a name="tutorial-add-sorting-filtering-and-paging---aspnet-mvc-with-ef-core"></a><span data-ttu-id="53f28-104">Esercitazione: Aggiungere funzionalità di ordinamento, filtro e suddivisione in pagine - ASP.NET MVC con EF Core</span><span class="sxs-lookup"><span data-stu-id="53f28-104">Tutorial: Add sorting, filtering, and paging - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="53f28-105">Nell'esercitazione precedente è stato implementato un set di pagine Web per operazioni CRUD di base per le entità Student.</span><span class="sxs-lookup"><span data-stu-id="53f28-105">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="53f28-106">In questa esercitazione si aggiungeranno le funzionalità di ordinamento, filtro e suddivisione in pagine alla pagina Student Index (Indice degli studenti).</span><span class="sxs-lookup"><span data-stu-id="53f28-106">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="53f28-107">Verrà anche creata una pagina che esegue il raggruppamento semplice.</span><span class="sxs-lookup"><span data-stu-id="53f28-107">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="53f28-108">La figura seguente illustra l'aspetto della pagina al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="53f28-108">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="53f28-109">Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna.</span><span class="sxs-lookup"><span data-stu-id="53f28-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="53f28-110">Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.</span><span class="sxs-lookup"><span data-stu-id="53f28-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Pagina Student Index (Indice degli studenti)](sort-filter-page/_static/paging.png)

<span data-ttu-id="53f28-112">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f28-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53f28-113">Aggiungere collegamenti per l'ordinamento delle colonne</span><span class="sxs-lookup"><span data-stu-id="53f28-113">Add column sort links</span></span>
> * <span data-ttu-id="53f28-114">Aggiungere una casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="53f28-114">Add a Search box</span></span>
> * <span data-ttu-id="53f28-115">Aggiungere la suddivisione in pagine per Student Index</span><span class="sxs-lookup"><span data-stu-id="53f28-115">Add paging to Students Index</span></span>
> * <span data-ttu-id="53f28-116">Aggiungere la suddivisione in pagine al metodo Index</span><span class="sxs-lookup"><span data-stu-id="53f28-116">Add paging to Index method</span></span>
> * <span data-ttu-id="53f28-117">Aggiungere collegamenti per la suddivisione in pagine</span><span class="sxs-lookup"><span data-stu-id="53f28-117">Add paging links</span></span>
> * <span data-ttu-id="53f28-118">Creare una pagina About</span><span class="sxs-lookup"><span data-stu-id="53f28-118">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53f28-119">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="53f28-119">Prerequisites</span></span>

* [<span data-ttu-id="53f28-120">Implementare la funzionalità CRUD con EF Core in un'app Web ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="53f28-120">Implement CRUD Functionality with EF Core in an ASP.NET Core MVC web app</span></span>](crud.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="53f28-121">Aggiungere collegamenti per l'ordinamento delle colonne</span><span class="sxs-lookup"><span data-stu-id="53f28-121">Add column sort links</span></span>

<span data-ttu-id="53f28-122">Per aggiungere l'ordinamento alla pagina Student Index (Indice degli studenti), sarà necessario modificare il metodo `Index` del controller Students e aggiungere codice alla visualizzazione Student Index (Indice degli studenti).</span><span class="sxs-lookup"><span data-stu-id="53f28-122">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="53f28-123">Aggiungere la funzionalità di ordinamento al metodo Index</span><span class="sxs-lookup"><span data-stu-id="53f28-123">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="53f28-124">In *StudentsController.cs* sostituire il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53f28-124">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="53f28-125">Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL.</span><span class="sxs-lookup"><span data-stu-id="53f28-125">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="53f28-126">Il valore della stringa di query viene inviato da ASP.NET Core MVC come parametro al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="53f28-126">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="53f28-127">Il parametro sarà una stringa "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e dalla stringa "desc" per specificare l'ordine decrescente.</span><span class="sxs-lookup"><span data-stu-id="53f28-127">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="53f28-128">L'ordinamento predefinito è crescente.</span><span class="sxs-lookup"><span data-stu-id="53f28-128">The default sort order is ascending.</span></span>

<span data-ttu-id="53f28-129">La prima volta che viene richiesta la pagina di indice, non è presente alcuna stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f28-129">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="53f28-130">Gli studenti vengono visualizzati in ordine crescente in base al cognome, che è il valore predefinito determinato dal caso di fallthrough nell'istruzione `switch`.</span><span class="sxs-lookup"><span data-stu-id="53f28-130">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="53f28-131">Quando l'utente fa clic sul collegamento ipertestuale di un'intestazione di colonna, nella stringa di query viene specificato il valore `sortOrder` appropriato.</span><span class="sxs-lookup"><span data-stu-id="53f28-131">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="53f28-132">I due elementi `ViewData` (NameSortParm e DateSortParm) vengono usati dalla visualizzazione per configurare i collegamenti ipertestuali dell'intestazione di colonna con i valori della stringa di query appropriata.</span><span class="sxs-lookup"><span data-stu-id="53f28-132">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="53f28-133">Si tratta di istruzioni ternarie.</span><span class="sxs-lookup"><span data-stu-id="53f28-133">These are ternary statements.</span></span> <span data-ttu-id="53f28-134">La prima specifica che, se il parametro `sortOrder` è Null o vuoto, NameSortParm deve essere impostato su "name_desc"; in caso contrario, deve essere impostato su una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="53f28-134">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="53f28-135">Queste due istruzioni consentono alla visualizzazione di impostare i collegamenti ipertestuali dell'intestazione di colonna come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="53f28-135">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="53f28-136">Ordinamento corrente</span><span class="sxs-lookup"><span data-stu-id="53f28-136">Current sort order</span></span>  | <span data-ttu-id="53f28-137">Collegamento ipertestuale cognome</span><span class="sxs-lookup"><span data-stu-id="53f28-137">Last Name Hyperlink</span></span> | <span data-ttu-id="53f28-138">Collegamento ipertestuale data</span><span class="sxs-lookup"><span data-stu-id="53f28-138">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="53f28-139">Cognome in ordine crescente</span><span class="sxs-lookup"><span data-stu-id="53f28-139">Last Name ascending</span></span>  | <span data-ttu-id="53f28-140">descending</span><span class="sxs-lookup"><span data-stu-id="53f28-140">descending</span></span>          | <span data-ttu-id="53f28-141">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-141">ascending</span></span>      |
| <span data-ttu-id="53f28-142">Cognome in ordine decrescente</span><span class="sxs-lookup"><span data-stu-id="53f28-142">Last Name descending</span></span> | <span data-ttu-id="53f28-143">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-143">ascending</span></span>           | <span data-ttu-id="53f28-144">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-144">ascending</span></span>      |
| <span data-ttu-id="53f28-145">Data in ordine crescente</span><span class="sxs-lookup"><span data-stu-id="53f28-145">Date ascending</span></span>       | <span data-ttu-id="53f28-146">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-146">ascending</span></span>           | <span data-ttu-id="53f28-147">descending</span><span class="sxs-lookup"><span data-stu-id="53f28-147">descending</span></span>     |
| <span data-ttu-id="53f28-148">Data in ordine decrescente</span><span class="sxs-lookup"><span data-stu-id="53f28-148">Date descending</span></span>      | <span data-ttu-id="53f28-149">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-149">ascending</span></span>           | <span data-ttu-id="53f28-150">ascending</span><span class="sxs-lookup"><span data-stu-id="53f28-150">ascending</span></span>      |

<span data-ttu-id="53f28-151">Il metodo usa LINQ to Entities per specificare la colonna in base alla quale eseguire l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="53f28-151">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="53f28-152">Il codice crea una variabile `IQueryable` prima dell'istruzione switch, la modifica nell'istruzione switch e chiama il metodo `ToListAsync` dopo l'istruzione `switch`.</span><span class="sxs-lookup"><span data-stu-id="53f28-152">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="53f28-153">Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database.</span><span class="sxs-lookup"><span data-stu-id="53f28-153">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="53f28-154">La query non viene eseguita finché l'oggetto `IQueryable` non viene convertito in una raccolta chiamando un metodo, ad esempio `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="53f28-154">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="53f28-155">Questo codice genera pertanto una singola query che non viene eseguita fino all'istruzione `return View`.</span><span class="sxs-lookup"><span data-stu-id="53f28-155">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="53f28-156">Questo codice può essere reso dettagliato con un numero elevato di colonne.</span><span class="sxs-lookup"><span data-stu-id="53f28-156">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="53f28-157">Nell'[ultima esercitazione di questa serie](advanced.md#dynamic-linq) viene illustrato come scrivere codice che consente di passare il nome della colonna `OrderBy` in una variabile di stringa.</span><span class="sxs-lookup"><span data-stu-id="53f28-157">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="53f28-158">Aggiungere collegamenti ipertestuali delle intestazioni di colonna alla visualizzazione Student Index (Indice degli studenti)</span><span class="sxs-lookup"><span data-stu-id="53f28-158">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="53f28-159">Sostituire il codice in *Views/Students/Index.cshtml* con il codice seguente per aggiungere collegamenti ipertestuali delle intestazioni di colonna.</span><span class="sxs-lookup"><span data-stu-id="53f28-159">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="53f28-160">Le righe modificate sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="53f28-160">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="53f28-161">Questo codice usa le informazioni contenute nelle proprietà `ViewData` per impostare i collegamenti ipertestuali con i valori della stringa di query appropriati.</span><span class="sxs-lookup"><span data-stu-id="53f28-161">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="53f28-162">Eseguire l'app, selezionare la scheda **Students** (Studenti) e fare clic sulle intestazioni di colonna **Last Name** (Cognome) e **Enrollment Date** (Data di iscrizione) per verificare che l'ordinamento funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="53f28-162">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Pagina Student Index (Indice degli studenti) in ordine di nome](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box"></a><span data-ttu-id="53f28-164">Aggiungere una casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="53f28-164">Add a Search box</span></span>

<span data-ttu-id="53f28-165">Per aggiungere filtri alla pagina Student Index (Indice degli studenti), è necessario aggiungere alla visualizzazione una casella di testo e un pulsante di invio e apportare le modifiche corrispondenti nel metodo `Index`.</span><span class="sxs-lookup"><span data-stu-id="53f28-165">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="53f28-166">La casella di testo consente di immettere una stringa per eseguire la ricerca nei campi di nome e cognome.</span><span class="sxs-lookup"><span data-stu-id="53f28-166">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="53f28-167">Aggiungere la funzionalità di filtro al metodo Index</span><span class="sxs-lookup"><span data-stu-id="53f28-167">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="53f28-168">In *StudentsController.cs* sostituire il metodo `Index` con il codice seguente (le modifiche sono evidenziate).</span><span class="sxs-lookup"><span data-stu-id="53f28-168">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="53f28-169">È stato aggiunto un parametro `searchString` al metodo `Index`.</span><span class="sxs-lookup"><span data-stu-id="53f28-169">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="53f28-170">Il valore della stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunta alla visualizzazione Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="53f28-170">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="53f28-171">È stata anche aggiunta all'istruzione LINQ una clausola where che seleziona solo gli studenti il cui nome o cognome contiene la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="53f28-171">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="53f28-172">L'istruzione che aggiunge la clausola where viene eseguita solo se è presente un valore per la ricerca.</span><span class="sxs-lookup"><span data-stu-id="53f28-172">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="53f28-173">In questo esempio si chiama il metodo `Where` su un oggetto `IQueryable` e il filtro verrà elaborato nel server.</span><span class="sxs-lookup"><span data-stu-id="53f28-173">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="53f28-174">In alcuni scenari potrebbe essere chiamato il metodo `Where` come metodo di estensione per una raccolta in memoria.</span><span class="sxs-lookup"><span data-stu-id="53f28-174">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="53f28-175">Si supponga ad esempio di modificare il riferimento a `_context.Students` in modo che faccia riferimento a un metodo di repository che restituisce una raccolta `IEnumerable` anziché a un elemento `DbSet` EF. Il risultato sarebbe in genere lo stesso, ma in alcuni casi potrebbe essere diverso.</span><span class="sxs-lookup"><span data-stu-id="53f28-175">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="53f28-176">Ad esempio, l'implementazione di .NET Framework del metodo `Contains` esegue un confronto con la distinzione tra maiuscole e minuscole per impostazione predefinita, ma in SQL Server questo è determinato dall'impostazione delle regole di confronto dell'istanza di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="53f28-176">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="53f28-177">Questa impostazione usa come valore predefinito la non applicazione della distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="53f28-177">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="53f28-178">È possibile chiamare il metodo `ToUpper` per fare in modo che il test non applichi in modo esplicito la distinzione tra maiuscole e minuscole:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="53f28-178">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="53f28-179">Questo fa sì che i risultati rimangano invariati se si modifica il codice in un secondo momento per usare un repository che restituisce una raccolta `IEnumerable` invece di un oggetto `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="53f28-179">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="53f28-180">Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database. Con questa soluzione si verifica tuttavia una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="53f28-180">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="53f28-181">Il codice `ToUpper` dovrà inserire una funzione nella clausola WHERE dell'istruzione TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="53f28-181">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="53f28-182">In questo modo si evita che l'ottimizzazione usi un indice.</span><span class="sxs-lookup"><span data-stu-id="53f28-182">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="53f28-183">Dato che SQL viene installato per lo più con l'impostazione senza distinzione tra maiuscole e minuscole, è consigliabile evitare il codice `ToUpper` fino a quando non si esegue la migrazione a un archivio con distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="53f28-183">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="53f28-184">Aggiungere una casella di ricerca alla visualizzazione Student Index (Indice degli studenti)</span><span class="sxs-lookup"><span data-stu-id="53f28-184">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="53f28-185">In *Views/Student/Index.cshtml*, aggiungere il codice evidenziato immediatamente prima del tag tabella di apertura per creare una barra del titolo, una casella di testo e un pulsante **Search** (Ricerca).</span><span class="sxs-lookup"><span data-stu-id="53f28-185">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="53f28-186">Questo codice usa l'[helper tag](xref:mvc/views/tag-helpers/intro) `<form>` per aggiungere la casella di testo e il pulsante di ricerca.</span><span class="sxs-lookup"><span data-stu-id="53f28-186">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="53f28-187">Per impostazione predefinita, l'helper tag `<form>` invia i dati del modulo con una richiesta POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="53f28-187">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="53f28-188">Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri.</span><span class="sxs-lookup"><span data-stu-id="53f28-188">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="53f28-189">Le linee guida W3C consigliano di usare l'istruzione GET quando l'azione non risulta in un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="53f28-189">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="53f28-190">Eseguire l'app, selezionare la scheda **Students** (Studenti), immettere una stringa di ricerca e fare clic su Search (Ricerca) per verificare che il filtro funzioni.</span><span class="sxs-lookup"><span data-stu-id="53f28-190">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Pagina Student Index (Indice degli studenti) con filtro](sort-filter-page/_static/filtering.png)

<span data-ttu-id="53f28-192">Si noti che l'URL contiene la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="53f28-192">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="53f28-193">Se questa pagina viene inserita tra i segnalibri, quando si usa il segnalibro viene visualizzato l'elenco filtrato.</span><span class="sxs-lookup"><span data-stu-id="53f28-193">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="53f28-194">L'aggiunta di `method="get"` al tag `form` è l'elemento che ha determinato la generazione della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f28-194">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="53f28-195">In questa fase, se si fa clic sul collegamento di ordinamento di un'intestazione di colonna si perderà il valore del filtro immesso nella casella **Search** (Ricerca).</span><span class="sxs-lookup"><span data-stu-id="53f28-195">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="53f28-196">Nella sezione successiva verrà spiegato come correggere il problema.</span><span class="sxs-lookup"><span data-stu-id="53f28-196">You'll fix that in the next section.</span></span>

## <a name="add-paging-to-students-index"></a><span data-ttu-id="53f28-197">Aggiungere la suddivisione in pagine per Student Index</span><span class="sxs-lookup"><span data-stu-id="53f28-197">Add paging to Students Index</span></span>

<span data-ttu-id="53f28-198">Per aggiungere la suddivisione in pagine alla pagina Student Index (Indice degli studenti), creare una classe `PaginatedList` che usa le istruzioni `Skip` e `Take` per filtrare i dati sul server invece di recuperare sempre tutte le righe della tabella.</span><span class="sxs-lookup"><span data-stu-id="53f28-198">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="53f28-199">È quindi possibile apportare altre modifiche nel metodo `Index` e aggiungere i pulsanti di suddivisione in pagine alla visualizzazione `Index`.</span><span class="sxs-lookup"><span data-stu-id="53f28-199">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="53f28-200">Nella figura seguente vengono illustrati i pulsanti di suddivisione in pagine.</span><span class="sxs-lookup"><span data-stu-id="53f28-200">The following illustration shows the paging buttons.</span></span>

![Pagina Student Index (Indice degli studenti) con collegamenti di suddivisione in pagine](sort-filter-page/_static/paging.png)

<span data-ttu-id="53f28-202">Nella cartella del progetto, creare `PaginatedList.cs` e quindi sostituire il codice del modello con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="53f28-202">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="53f28-203">Il metodo `CreateAsync` in questo codice accetta le dimensioni di pagina e il numero delle pagine e applica le istruzioni `Skip` e `Take` appropriate a `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="53f28-203">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="53f28-204">Quando `ToListAsync` viene chiamato su `IQueryable`, restituisce un elenco contenente solo la pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f28-204">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="53f28-205">Le proprietà `HasPreviousPage` e `HasNextPage` possono essere usate per abilitare o disabilitare i pulsanti di suddivisione in pagine **Previous** (Indietro) e **Next** (Avanti).</span><span class="sxs-lookup"><span data-stu-id="53f28-205">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="53f28-206">Viene usato un metodo `CreateAsync` invece di un costruttore per creare l'oggetto `PaginatedList<T>` poiché i costruttori non possono eseguire codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="53f28-206">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-to-index-method"></a><span data-ttu-id="53f28-207">Aggiungere la suddivisione in pagine al metodo Index</span><span class="sxs-lookup"><span data-stu-id="53f28-207">Add paging to Index method</span></span>

<span data-ttu-id="53f28-208">In *StudentsController.cs* sostituire il metodo `Index` con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="53f28-208">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="53f28-209">Questo codice aggiunge un parametro di numeri di pagina, un parametro di ordinamento corrente e un parametro di filtro corrente alla firma del metodo.</span><span class="sxs-lookup"><span data-stu-id="53f28-209">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="53f28-210">La prima volta che viene visualizzata la pagina o se l'utente non ha selezionato un collegamento di suddivisione in pagine o di ordinamento, tutti i parametri saranno Null.</span><span class="sxs-lookup"><span data-stu-id="53f28-210">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="53f28-211">Se si seleziona un collegamento di suddivisione in pagine, la variabile di pagina conterrà il numero della pagina da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="53f28-211">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="53f28-212">L'elemento `ViewData` denominato CurrentSort offre la visualizzazione con l'ordinamento corrente, in quanto esso deve essere incluso nei collegamenti di suddivisione in pagine per mantenere l'ordinamento durante la suddivisione in pagine.</span><span class="sxs-lookup"><span data-stu-id="53f28-212">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="53f28-213">L'elemento `ViewData` denominato CurrentFilter offre la visualizzazione con la stringa di filtro corrente.</span><span class="sxs-lookup"><span data-stu-id="53f28-213">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="53f28-214">Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata.</span><span class="sxs-lookup"><span data-stu-id="53f28-214">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="53f28-215">Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi.</span><span class="sxs-lookup"><span data-stu-id="53f28-215">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="53f28-216">La stringa di ricerca viene modificata quando si immette un valore nella casella di testo e si preme il pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="53f28-216">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="53f28-217">In tal caso, il parametro `searchString` non è Null.</span><span class="sxs-lookup"><span data-stu-id="53f28-217">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="53f28-218">Alla fine del metodo `Index`, il metodo `PaginatedList.CreateAsync` converte la query degli studenti in una pagina singola di studenti in un tipo di raccolta che supporta la suddivisione in pagine.</span><span class="sxs-lookup"><span data-stu-id="53f28-218">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="53f28-219">La pagina singola di studenti viene quindi passata alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="53f28-219">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="53f28-220">Il metodo `PaginatedList.CreateAsync` accetta un numero di pagina.</span><span class="sxs-lookup"><span data-stu-id="53f28-220">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="53f28-221">I due punti interrogativi rappresentano l'operatore null-coalescing.</span><span class="sxs-lookup"><span data-stu-id="53f28-221">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="53f28-222">L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.</span><span class="sxs-lookup"><span data-stu-id="53f28-222">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links"></a><span data-ttu-id="53f28-223">Aggiungere collegamenti per la suddivisione in pagine</span><span class="sxs-lookup"><span data-stu-id="53f28-223">Add paging links</span></span>

<span data-ttu-id="53f28-224">In *Views/Students/Index.cshtml* sostituire il codice esistente con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="53f28-224">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="53f28-225">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="53f28-225">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="53f28-226">L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PaginatedList<T>` anziché un oggetto `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="53f28-226">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="53f28-227">I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:</span><span class="sxs-lookup"><span data-stu-id="53f28-227">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="53f28-228">I pulsanti di suddivisione in pagine vengono visualizzati dagli helper tag:</span><span class="sxs-lookup"><span data-stu-id="53f28-228">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="53f28-229">Eseguire l'app e passare alla pagina Students (Studenti).</span><span class="sxs-lookup"><span data-stu-id="53f28-229">Run the app and go to the Students page.</span></span>

![Pagina Student Index (Indice degli studenti) con collegamenti di suddivisione in pagine](sort-filter-page/_static/paging.png)

<span data-ttu-id="53f28-231">Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni.</span><span class="sxs-lookup"><span data-stu-id="53f28-231">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="53f28-232">Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.</span><span class="sxs-lookup"><span data-stu-id="53f28-232">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="53f28-233">Creare una pagina About</span><span class="sxs-lookup"><span data-stu-id="53f28-233">Create an About page</span></span>

<span data-ttu-id="53f28-234">Per la pagina **About** (Informazioni) del sito Web Contoso University verrà visualizzato il numero di studenti iscritti per ogni data di iscrizione.</span><span class="sxs-lookup"><span data-stu-id="53f28-234">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="53f28-235">Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi.</span><span class="sxs-lookup"><span data-stu-id="53f28-235">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="53f28-236">Per completare questa procedura, è necessario eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f28-236">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="53f28-237">Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="53f28-237">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="53f28-238">Modificare il metodo About nel controller Home.</span><span class="sxs-lookup"><span data-stu-id="53f28-238">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="53f28-239">Modificare la visualizzazione About (Informazioni).</span><span class="sxs-lookup"><span data-stu-id="53f28-239">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="53f28-240">Creare il modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="53f28-240">Create the view model</span></span>

<span data-ttu-id="53f28-241">Creare una cartella *SchoolViewModels* nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="53f28-241">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="53f28-242">Nella nuova cartella aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53f28-242">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="53f28-243">Modificare il controller Home</span><span class="sxs-lookup"><span data-stu-id="53f28-243">Modify the Home Controller</span></span>

<span data-ttu-id="53f28-244">In *HomeController.cs* aggiungere le seguenti istruzioni all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="53f28-244">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="53f28-245">Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe e ottenere un'istanza del contesto da ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="53f28-245">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="53f28-246">Sostituire il metodo `About` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53f28-246">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="53f28-247">L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="53f28-247">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE]
> <span data-ttu-id="53f28-248">Nella versione 1.0 di Entity Framework Core, l'intero set di risultati viene restituito al client e il raggruppamento avviene sul client.</span><span class="sxs-lookup"><span data-stu-id="53f28-248">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="53f28-249">In alcuni scenari questo potrebbe creare problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="53f28-249">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="53f28-250">Testare le prestazioni con volumi di produzione di dati e, se necessario, usare SQL non elaborato per eseguire il raggruppamento nel server.</span><span class="sxs-lookup"><span data-stu-id="53f28-250">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="53f28-251">Per informazioni sull'uso di SQL non elaborato, vedere l'[ultima esercitazione di questa serie](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="53f28-251">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="53f28-252">Modificare la visualizzazione della pagina About (Informazioni)</span><span class="sxs-lookup"><span data-stu-id="53f28-252">Modify the About View</span></span>

<span data-ttu-id="53f28-253">Sostituire il codice nel file *Views/Home/About.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53f28-253">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="53f28-254">Eseguire l'app e passare alla pagina About (Informazioni).</span><span class="sxs-lookup"><span data-stu-id="53f28-254">Run the app and go to the About page.</span></span> <span data-ttu-id="53f28-255">Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.</span><span class="sxs-lookup"><span data-stu-id="53f28-255">The count of students for each enrollment date is displayed in a table.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="53f28-256">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="53f28-256">Get the code</span></span>

[<span data-ttu-id="53f28-257">Scaricare o visualizzare l'applicazione completata.</span><span class="sxs-lookup"><span data-stu-id="53f28-257">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="53f28-258">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="53f28-258">Next steps</span></span>

<span data-ttu-id="53f28-259">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f28-259">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53f28-260">Aggiungere collegamenti per l'ordinamento delle colonne</span><span class="sxs-lookup"><span data-stu-id="53f28-260">Added column sort links</span></span>
> * <span data-ttu-id="53f28-261">Aggiungere una casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="53f28-261">Added a Search box</span></span>
> * <span data-ttu-id="53f28-262">Aggiungere la suddivisione in pagine per Student Index</span><span class="sxs-lookup"><span data-stu-id="53f28-262">Added paging to Students Index</span></span>
> * <span data-ttu-id="53f28-263">Aggiungere la suddivisione in pagine al metodo Index</span><span class="sxs-lookup"><span data-stu-id="53f28-263">Added paging to Index method</span></span>
> * <span data-ttu-id="53f28-264">Aggiungere collegamenti per la suddivisione in pagine</span><span class="sxs-lookup"><span data-stu-id="53f28-264">Added paging links</span></span>
> * <span data-ttu-id="53f28-265">Creare una pagina About</span><span class="sxs-lookup"><span data-stu-id="53f28-265">Created an About page</span></span>

<span data-ttu-id="53f28-266">Nella prossima esercitazione si apprenderà come gestire le modifiche al modello di dati tramite le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="53f28-266">Advance to the next article to learn how to handle data model changes by using migrations.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="53f28-267">Gestire le modifiche al modello di dati</span><span class="sxs-lookup"><span data-stu-id="53f28-267">Handle data model changes</span></span>](migrations.md)