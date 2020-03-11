---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestione delle relazioni tra entità | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557463"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="d3ae0-102">Gestione delle relazioni tra entità</span><span class="sxs-lookup"><span data-stu-id="d3ae0-102">Handling Entity Relations</span></span>

<span data-ttu-id="d3ae0-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d3ae0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d3ae0-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="d3ae0-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d3ae0-105">In questa sezione vengono descritti alcuni dettagli sul modo in cui EF carica le entità correlate e come gestire le proprietà di navigazione circolari nelle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="d3ae0-106">Questa sezione fornisce informazioni di base e non è necessaria per completare l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="d3ae0-107">Se preferisci, vai alla [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="d3ae0-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="d3ae0-108">Caricamento eager rispetto al caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="d3ae0-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="d3ae0-109">Quando si usa EF con un database relazionale, è importante comprendere in che modo EF carica i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="d3ae0-110">È anche utile vedere le query SQL generate da EF.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="d3ae0-111">Per tracciare l'oggetto SQL, aggiungere la seguente riga di codice al costruttore `BookServiceContext`:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="d3ae0-112">Se si invia una richiesta GET a/API/Books, viene restituito JSON come il seguente:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="d3ae0-113">Si noterà che la proprietà Author è null, anche se il libro contiene una valida autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="d3ae0-114">Questo perché EF non carica le entità Author correlate.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="d3ae0-115">Il log di traccia della query SQL conferma quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="d3ae0-116">L'istruzione SELECT prende dalla tabella books e non fa riferimento alla tabella author.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="d3ae0-117">Per riferimento, di seguito è riportato il metodo nella classe `BooksController` che restituisce l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="d3ae0-118">Vediamo come possiamo restituire l'autore come parte dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="d3ae0-119">Sono disponibili tre modi per caricare i dati correlati in Entity Framework: caricamento eager, caricamento lazy e caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="d3ae0-120">Esistono compromessi per ciascuna tecnica, quindi è importante comprenderne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="d3ae0-121">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="d3ae0-121">Eager Loading</span></span>

<span data-ttu-id="d3ae0-122">Con il *caricamento eager*, EF carica le entità correlate come parte della query di database iniziale.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="d3ae0-123">Per eseguire il caricamento eager, usare il metodo di estensione **System. Data. Entity. include** .</span><span class="sxs-lookup"><span data-stu-id="d3ae0-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="d3ae0-124">Ciò indica a EF di includere i dati dell'autore nella query.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="d3ae0-125">Se si apportano queste modifiche ed è stata eseguita l'app, ora i dati JSON sono simili ai seguenti:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="d3ae0-126">Il log di traccia Mostra che EF ha eseguito un join sulle tabelle Book e Author.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="d3ae0-127">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="d3ae0-127">Lazy Loading</span></span>

<span data-ttu-id="d3ae0-128">Con il caricamento lazy, EF carica automaticamente un'entità correlata quando viene dereferenziata la proprietà di navigazione per l'entità.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="d3ae0-129">Per abilitare il caricamento lazy, rendere virtuale la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="d3ae0-130">Ad esempio, nella classe Book:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="d3ae0-131">Si consideri ora il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="d3ae0-132">Quando il caricamento lazy è abilitato, l'accesso alla proprietà `Author` in `books[0]` causa l'esecuzione di una query sul database per l'autore da parte di EF.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="d3ae0-133">Il caricamento lazy richiede più percorsi di database, perché EF invia una query ogni volta che viene recuperata un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="d3ae0-134">In genere, è necessario disabilitare il caricamento lazy per gli oggetti serializzati.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="d3ae0-135">Il serializzatore deve leggere tutte le proprietà del modello, che attiva il caricamento delle entità correlate.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="d3ae0-136">Ad esempio, di seguito sono riportate le query SQL quando EF serializza l'elenco di libri in cui è abilitato il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="d3ae0-137">Come si può notare, EF esegue tre query separate per i tre autori.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="d3ae0-138">In alcuni casi è possibile utilizzare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="d3ae0-139">Il caricamento eager può causare la generazione di un join molto complesso da parte di EF.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="d3ae0-140">In alternativa, potrebbero essere necessarie entità correlate per un piccolo subset di dati e il caricamento lazy risulta più efficiente.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="d3ae0-141">Un modo per evitare problemi di serializzazione consiste nel serializzare gli oggetti DTO (Data Transfer Objects) anziché gli oggetti entità.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="d3ae0-142">Questo approccio verrà illustrato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="d3ae0-143">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="d3ae0-143">Explicit Loading</span></span>

<span data-ttu-id="d3ae0-144">Il caricamento esplicito è simile al caricamento lazy, tranne per il fatto che si ottengono in modo esplicito i dati correlati nel codice; non viene eseguita automaticamente quando si accede a una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="d3ae0-145">Il caricamento esplicito offre un maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="d3ae0-146">Per ulteriori informazioni sul caricamento esplicito, vedere [caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="d3ae0-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="d3ae0-147">Proprietà di navigazione e riferimenti circolari</span><span class="sxs-lookup"><span data-stu-id="d3ae0-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="d3ae0-148">Quando sono stati definiti i modelli Book e Author, è stata definita una proprietà di navigazione nella classe `Book` per la relazione book-author, ma non è stata definita una proprietà di navigazione nell'altra direzione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="d3ae0-149">Cosa accade se si aggiunge la proprietà di navigazione corrispondente alla classe `Author`?</span><span class="sxs-lookup"><span data-stu-id="d3ae0-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="d3ae0-150">Sfortunatamente, questo crea un problema quando si serializzano i modelli.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="d3ae0-151">Se si caricano i dati correlati, viene creato un oggetto grafico circolare.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="d3ae0-152">Quando il formattatore JSON o XML tenta di serializzare il grafo, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="d3ae0-153">I due formattatori generano messaggi di eccezione diversi.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="d3ae0-154">Di seguito è riportato un esempio per il formattatore JSON:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="d3ae0-155">Il formattatore XML è il seguente:</span><span class="sxs-lookup"><span data-stu-id="d3ae0-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="d3ae0-156">Una soluzione consiste nell'usare dto, descritto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="d3ae0-157">In alternativa, è possibile configurare i formattatori JSON e XML per gestire i cicli Graph.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="d3ae0-158">Per ulteriori informazioni, vedere [gestione di riferimenti a oggetti circolari](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="d3ae0-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="d3ae0-159">Per questa esercitazione non è necessaria la proprietà di navigazione `Author.Book`, quindi è possibile lasciarla invariata.</span><span class="sxs-lookup"><span data-stu-id="d3ae0-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3ae0-160">[Precedente](part-3.md)
> [Successivo](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d3ae0-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
