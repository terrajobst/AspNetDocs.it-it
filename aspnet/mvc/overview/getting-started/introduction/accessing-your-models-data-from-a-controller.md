---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: e01953dcfb2abf2db53a8aa869aa75b40485daca
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519089"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="4a4f5-102">Accesso ai dati del modello da un controller</span><span class="sxs-lookup"><span data-stu-id="4a4f5-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="4a4f5-103">di [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4a4f5-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="4a4f5-104">In questa sezione si creerà una nuova classe `MoviesController` e si scriverà il codice che recupera i dati dei film e li Visualizza nel browser usando un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="4a4f5-105">**Compilare l'applicazione** prima di andare al passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="4a4f5-106">Se non si compila l'applicazione, si otterrà un errore durante l'aggiunta di un controller.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="4a4f5-107">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *controller* , quindi scegliere **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="4a4f5-108">Nella finestra di dialogo **Aggiungi impalcatura** fare clic su **controller MVC 5 con visualizzazioni, usando Entity Framework**e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="4a4f5-109">Selezionare **Movie (MvcMovie. Models)** per la classe del modello.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="4a4f5-110">Selezionare **MovieDBContext (MvcMovie. Models)** per la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="4a4f5-111">Per il nome del controller immettere **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="4a4f5-112">L'immagine seguente mostra la finestra di dialogo completata.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="4a4f5-113">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-113">Click **Add**.</span></span> <span data-ttu-id="4a4f5-114">Se viene restituito un errore, è probabile che l'applicazione non sia stata compilata prima di iniziare ad aggiungere il controller. In Visual Studio vengono creati i file e le cartelle seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="4a4f5-115">*Un file MoviesController.cs* nella cartella *Controllers* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="4a4f5-116">Una cartella *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="4a4f5-117">*Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* nella nuova cartella *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="4a4f5-118">Visual Studio ha creato automaticamente i metodi di azione [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creazione, lettura, aggiornamento ed eliminazione) e le visualizzazioni (la creazione automatica di metodi di azione CRUD e visualizzazioni è nota come impalcatura).</span><span class="sxs-lookup"><span data-stu-id="4a4f5-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="4a4f5-119">A questo punto si dispone di un'applicazione Web completamente funzionante che consente di creare, elencare, modificare ed eliminare voci di film.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="4a4f5-120">Eseguire l'applicazione e fare clic sul collegamento **MVC Movie** (oppure selezionare il controller di `Movies` aggiungendo */Movies* all'URL nella barra degli indirizzi del browser).</span><span class="sxs-lookup"><span data-stu-id="4a4f5-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="4a4f5-121">Poiché l'applicazione si basa sul routing predefinito (definito nel file *App\_Start\RouteConfig.cs* ), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzata al metodo di azione `Index` predefinito del controller `Movies`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="4a4f5-122">In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` è effettivamente identica a quella della richiesta del browser `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="4a4f5-123">Il risultato è un elenco vuoto di filmati, perché non sono stati ancora aggiunti.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="4a4f5-124">Creazione di un film</span><span class="sxs-lookup"><span data-stu-id="4a4f5-124">Creating a Movie</span></span>

<span data-ttu-id="4a4f5-125">Selezionare il collegamento **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-125">Select the **Create New** link.</span></span> <span data-ttu-id="4a4f5-126">Immettere alcuni dettagli su un film, quindi fare clic sul pulsante **Crea** .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="4a4f5-127">Potrebbe non essere possibile immettere punti decimali o virgole nel campo price.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="4a4f5-128">per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in inglese (Stati Uniti), è necessario includere *globalizzate. js* e il file *Cultures/globalizzate. culture. js* specifico (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per usare `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="4a4f5-129">Verrà illustrato come eseguire questa operazione nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="4a4f5-130">Per il momento, immettere solo numeri interi come 10.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="4a4f5-131">Se si fa clic sul pulsante **Crea** , il form viene inviato al server, dove le informazioni sul film vengono salvate nel database.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="4a4f5-132">Si viene quindi reindirizzati all'URL */Movies* , in cui è possibile visualizzare il film appena creato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="4a4f5-133">Creare altre due voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-133">Create a couple more movie entries.</span></span> <span data-ttu-id="4a4f5-134">Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="4a4f5-135">Esame del codice generato</span><span class="sxs-lookup"><span data-stu-id="4a4f5-135">Examining the Generated Code</span></span>

<span data-ttu-id="4a4f5-136">Aprire il file *Controllers\MoviesController.cs* ed esaminare il metodo `Index` generato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="4a4f5-137">Di seguito è riportata una parte del controller di film con il metodo `Index`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="4a4f5-138">Una richiesta al controller di `Movies` restituisce tutte le voci della tabella `Movies` e quindi passa i risultati alla visualizzazione `Index`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="4a4f5-139">La riga seguente dalla classe `MoviesController` crea un'istanza di un contesto di database di film, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="4a4f5-140">È possibile usare il contesto del database dei film per eseguire query, modificare ed eliminare i film.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="4a4f5-141">Modelli fortemente tipizzati e parola chiave @model</span><span class="sxs-lookup"><span data-stu-id="4a4f5-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="4a4f5-142">In precedenza in questa esercitazione è stato illustrato come un controller può passare dati o oggetti a un modello di visualizzazione utilizzando l'oggetto `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="4a4f5-143">Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico di associazione tardiva per passare informazioni a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="4a4f5-144">MVC offre inoltre la possibilità di passare oggetti *fortemente* tipizzati a un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="4a4f5-145">Questo approccio fortemente tipizzato consente un migliore controllo della fase di compilazione del codice e [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) più completo nell'editor di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="4a4f5-146">Il meccanismo di impalcatura in Visual Studio usava questo approccio (ovvero passando un modello *fortemente* tipizzato) con la classe `MoviesController` e i modelli di visualizzazione quando creava i metodi e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="4a4f5-147">Nel file *Controllers\MoviesController.cs* esaminare il metodo `Details` generato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="4a4f5-148">Di seguito è illustrato il metodo `Details`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="4a4f5-149">Il parametro `id` viene in genere passato come dati di route, ad esempio `http://localhost:1234/movies/details/1` imposta il controller sul controller di film, l'azione da `details` e la `id` su 1.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="4a4f5-150">È anche possibile passare l'ID con una stringa di query come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="4a4f5-151">Se viene rilevata una `Movie`, viene passata un'istanza del modello di `Movie` alla visualizzazione `Details`:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="4a4f5-152">Esaminare il contenuto del file *Views\Movies\Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="4a4f5-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="4a4f5-153">Includendo un'istruzione `@model` nella parte superiore del file di modello di visualizzazione, è possibile specificare il tipo di oggetto previsto dalla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="4a4f5-154">Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="4a4f5-155">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="4a4f5-156">Nel modello *Details. cshtml* , ad esempio, il codice passa ogni campo film agli helper HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con l'oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="4a4f5-157">I metodi `Create` e `Edit` e i modelli di visualizzazione passano anche un oggetto modello di film.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="4a4f5-158">Esaminare il modello di vista *index. cshtml* e il metodo `Index` nel file *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="4a4f5-159">Si noti che il codice crea un oggetto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) quando chiama il metodo helper `View` nel metodo di azione `Index`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="4a4f5-160">Il codice passa quindi questo elenco `Movies` dal metodo di azione `Index` alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="4a4f5-161">Quando è stato creato il controller di film, Visual Studio ha automaticamente incluso l'istruzione `@model` seguente all'inizio del file *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="4a4f5-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="4a4f5-162">Questa `@model` direttiva consente di accedere all'elenco di filmati che il controller ha passato alla visualizzazione usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="4a4f5-163">Nel modello *index. cshtml* , ad esempio, il codice scorre i film eseguendo un'istruzione `foreach` sull'oggetto `Model` fortemente tipizzato:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="4a4f5-164">Poiché l'oggetto `Model` è fortemente tipizzato (come oggetto `IEnumerable<Movie>`), ogni oggetto `item` nel ciclo viene tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="4a4f5-165">Tra gli altri vantaggi, questo significa che si ottiene il controllo in fase di compilazione del codice e il supporto completo di IntelliSense nell'editor di codice:</span><span class="sxs-lookup"><span data-stu-id="4a4f5-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="4a4f5-167">Utilizzo di SQL Server Local DB</span><span class="sxs-lookup"><span data-stu-id="4a4f5-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="4a4f5-168">Entity Framework Code First rilevato che la stringa di connessione del database fornita fa riferimento a un database di `Movies` che non esisteva ancora, quindi Code First creato automaticamente il database.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="4a4f5-169">È possibile verificare che sia stato creato cercando nella cartella *App\_data* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="4a4f5-170">Se il file *Movies. MDF* non viene visualizzato, fare clic sul pulsante **Mostra tutti i file** nella barra degli strumenti **Esplora soluzioni** , fare clic sul pulsante **aggiorna** , quindi espandere la cartella *app\_data* .</span><span class="sxs-lookup"><span data-stu-id="4a4f5-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="4a4f5-171">Fare doppio clic su *Movies. MDF* per aprire **Esplora server**, quindi espandere la cartella **tabelle** per visualizzare la tabella Movies.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="4a4f5-172">Si noti l'icona a forma di chiave accanto a ID.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-172">Note the key icon next to ID.</span></span> <span data-ttu-id="4a4f5-173">Per impostazione predefinita, EF renderà una proprietà denominata ID la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="4a4f5-174">Per ulteriori informazioni su Entity Framework e MVC, vedere la pagina relativa all'eccellente esercitazione su [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="4a4f5-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="4a4f5-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="4a4f5-176">Fare clic con il pulsante destro del mouse sulla tabella `Movies` e scegliere **Mostra dati tabella** per visualizzare i dati creati.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="4a4f5-177">Fare clic con il pulsante destro del mouse sulla tabella `Movies` e selezionare **Apri definizione tabella** per visualizzare la struttura della tabella che Entity Framework Code First creata.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="4a4f5-178">Si noti come viene eseguito il mapping dello schema della tabella `Movies` alla classe `Movie` creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="4a4f5-179">Entity Framework Code First creato automaticamente questo schema in base alla classe di `Movie`.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="4a4f5-180">Al termine, chiudere la connessione facendo clic con il pulsante destro del mouse su *MovieDBContext* e scegliendo **Chiudi connessione**.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="4a4f5-181">Se non si chiude la connessione, è possibile che venga ricevuto un errore alla successiva esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="4a4f5-182">È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="4a4f5-183">Nell'esercitazione successiva si esaminerà il resto del codice con impalcature e si aggiungerà un metodo di `SearchIndex` e una visualizzazione `SearchIndex` che consente di cercare i film in questo database.</span><span class="sxs-lookup"><span data-stu-id="4a4f5-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="4a4f5-184">Per altre informazioni sull'uso di Entity Framework con MVC, vedere [creazione di un modello di dati Entity Framework per un'applicazione mvc ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="4a4f5-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4a4f5-185">[Precedente](creating-a-connection-string.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="4a4f5-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
