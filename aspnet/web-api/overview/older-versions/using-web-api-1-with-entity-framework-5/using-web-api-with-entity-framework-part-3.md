---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: creazione di un controller di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556049"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="a7a43-102">Parte 3: creazione di un controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="a7a43-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="a7a43-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a7a43-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a7a43-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="a7a43-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="a7a43-105">Aggiungere un controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="a7a43-105">Add an Admin Controller</span></span>

<span data-ttu-id="a7a43-106">In questa sezione si aggiungerà un controller API Web che supporta operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) sui prodotti.</span><span class="sxs-lookup"><span data-stu-id="a7a43-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="a7a43-107">Il controller utilizzerà Entity Framework per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="a7a43-108">Solo gli amministratori saranno in grado di utilizzare questo controller.</span><span class="sxs-lookup"><span data-stu-id="a7a43-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="a7a43-109">I clienti potranno accedere ai prodotti tramite un altro controller.</span><span class="sxs-lookup"><span data-stu-id="a7a43-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="a7a43-110">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="a7a43-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a7a43-111">Selezionare **Aggiungi** e quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="a7a43-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="a7a43-112">Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="a7a43-113">In **modello**selezionare &quot;controller API con azioni di lettura/scrittura, usando Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="a7a43-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="a7a43-114">In **classe modello**selezionare "Product (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="a7a43-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="a7a43-115">In **contesto dati**selezionare "&lt;nuovo contesto dati&gt;".</span><span class="sxs-lookup"><span data-stu-id="a7a43-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a7a43-116">Se l'elenco a discesa **classe modello** non mostra alcuna classe del modello, assicurarsi di aver compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="a7a43-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="a7a43-117">Entity Framework usa la reflection, quindi richiede l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="a7a43-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="a7a43-118">Se si seleziona "&lt;nuovo contesto dati&gt;", viene aperta la finestra di dialogo **nuovo contesto dati** .</span><span class="sxs-lookup"><span data-stu-id="a7a43-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="a7a43-119">Denominare il contesto dati `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="a7a43-120">Fare clic su **OK** per chiudere la finestra di dialogo **nuovo contesto dati** .</span><span class="sxs-lookup"><span data-stu-id="a7a43-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="a7a43-121">Nella finestra di dialogo **Aggiungi controller** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a7a43-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="a7a43-122">Ecco cosa è stato aggiunto al progetto:</span><span class="sxs-lookup"><span data-stu-id="a7a43-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="a7a43-123">Classe denominata `OrdersContext` che deriva da **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="a7a43-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="a7a43-124">Questa classe fornisce l'associazione tra i modelli POCO e il database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="a7a43-125">Un controller API Web denominato `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="a7a43-126">Questo controller supporta le operazioni CRUD sulle istanze `Product`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="a7a43-127">Usa la classe `OrdersContext` per comunicare con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7a43-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="a7a43-128">Nuova stringa di connessione del database nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="a7a43-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="a7a43-129">Aprire il file OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="a7a43-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="a7a43-130">Si noti che il costruttore specifica il nome della stringa di connessione del database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="a7a43-131">Questo nome fa riferimento alla stringa di connessione aggiunta al file Web. config.</span><span class="sxs-lookup"><span data-stu-id="a7a43-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="a7a43-132">Aggiungere le proprietà seguenti alla classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="a7a43-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="a7a43-133">Un **DbSet** rappresenta un set di entità su cui è possibile eseguire una query.</span><span class="sxs-lookup"><span data-stu-id="a7a43-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="a7a43-134">Di seguito è riportato l'elenco completo per la classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="a7a43-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="a7a43-135">La classe `AdminController` definisce cinque metodi che implementano la funzionalità CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="a7a43-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="a7a43-136">Ogni metodo corrisponde a un URI che il client può richiamare:</span><span class="sxs-lookup"><span data-stu-id="a7a43-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="a7a43-137">Controller (metodo)</span><span class="sxs-lookup"><span data-stu-id="a7a43-137">Controller Method</span></span> | <span data-ttu-id="a7a43-138">Description</span><span class="sxs-lookup"><span data-stu-id="a7a43-138">Description</span></span> | <span data-ttu-id="a7a43-139">URI</span><span class="sxs-lookup"><span data-stu-id="a7a43-139">URI</span></span> | <span data-ttu-id="a7a43-140">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="a7a43-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a7a43-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="a7a43-141">GetProducts</span></span> | <span data-ttu-id="a7a43-142">Ottiene tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="a7a43-142">Gets all products.</span></span> | <span data-ttu-id="a7a43-143">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="a7a43-143">api/products</span></span> | <span data-ttu-id="a7a43-144">GET</span><span class="sxs-lookup"><span data-stu-id="a7a43-144">GET</span></span> |
| <span data-ttu-id="a7a43-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="a7a43-145">GetProduct</span></span> | <span data-ttu-id="a7a43-146">Trova un prodotto in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="a7a43-146">Finds a product by ID.</span></span> | <span data-ttu-id="a7a43-147">API/prodotti/*ID*</span><span class="sxs-lookup"><span data-stu-id="a7a43-147">api/products/*id*</span></span> | <span data-ttu-id="a7a43-148">GET</span><span class="sxs-lookup"><span data-stu-id="a7a43-148">GET</span></span> |
| <span data-ttu-id="a7a43-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="a7a43-149">PutProduct</span></span> | <span data-ttu-id="a7a43-150">Aggiorna un prodotto.</span><span class="sxs-lookup"><span data-stu-id="a7a43-150">Updates a product.</span></span> | <span data-ttu-id="a7a43-151">API/prodotti/*ID*</span><span class="sxs-lookup"><span data-stu-id="a7a43-151">api/products/*id*</span></span> | <span data-ttu-id="a7a43-152">PUT</span><span class="sxs-lookup"><span data-stu-id="a7a43-152">PUT</span></span> |
| <span data-ttu-id="a7a43-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="a7a43-153">PostProduct</span></span> | <span data-ttu-id="a7a43-154">Crea un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="a7a43-154">Creates a new product.</span></span> | <span data-ttu-id="a7a43-155">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="a7a43-155">api/products</span></span> | <span data-ttu-id="a7a43-156">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="a7a43-156">POST</span></span> |
| <span data-ttu-id="a7a43-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="a7a43-157">DeleteProduct</span></span> | <span data-ttu-id="a7a43-158">Elimina un prodotto.</span><span class="sxs-lookup"><span data-stu-id="a7a43-158">Deletes a product.</span></span> | <span data-ttu-id="a7a43-159">API/prodotti/*ID*</span><span class="sxs-lookup"><span data-stu-id="a7a43-159">api/products/*id*</span></span> | <span data-ttu-id="a7a43-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="a7a43-160">DELETE</span></span> |

<span data-ttu-id="a7a43-161">Ogni metodo chiama `OrdersContext` per eseguire una query sul database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="a7a43-162">I metodi che modificano la chiamata della raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanente le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="a7a43-163">I controller vengono creati per ogni richiesta HTTP e quindi eliminati, quindi è necessario salvare le modifiche prima che un metodo venga restituito.</span><span class="sxs-lookup"><span data-stu-id="a7a43-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="a7a43-164">Aggiungere un inizializzatore di database</span><span class="sxs-lookup"><span data-stu-id="a7a43-164">Add a Database Initializer</span></span>

<span data-ttu-id="a7a43-165">Entity Framework dispone di una funzionalità interessante che consente di popolare il database all'avvio e di ricreare automaticamente il database ogni volta che i modelli cambiano.</span><span class="sxs-lookup"><span data-stu-id="a7a43-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="a7a43-166">Questa funzionalità è utile durante lo sviluppo, perché sono sempre presenti dati di test, anche se si modificano i modelli.</span><span class="sxs-lookup"><span data-stu-id="a7a43-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="a7a43-167">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Models e creare una nuova classe denominata `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="a7a43-168">Incollare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="a7a43-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="a7a43-169">Ereditando dalla classe **DropCreateDatabaseIfModelChanges** , viene indicato Entity Framework eliminare il database ogni volta che si modificano le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="a7a43-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="a7a43-170">Quando Entity Framework crea (o ricrea) il database, chiama il metodo **Seed** per popolare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="a7a43-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="a7a43-171">Viene usato il metodo **Seed** per aggiungere alcuni prodotti di esempio e un ordine di esempio.</span><span class="sxs-lookup"><span data-stu-id="a7a43-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="a7a43-172">Questa funzionalità è ideale per i test, ma non usa la classe **DropCreateDatabaseIfModelChanges** nell'ambiente di produzione, perché è possibile perdere i dati se un utente modifica una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="a7a43-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="a7a43-173">Aprire quindi Global. asax e aggiungere il codice seguente al metodo di **avvio dell'applicazione\_** :</span><span class="sxs-lookup"><span data-stu-id="a7a43-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="a7a43-174">Inviare una richiesta al controller</span><span class="sxs-lookup"><span data-stu-id="a7a43-174">Send a Request to the Controller</span></span>

<span data-ttu-id="a7a43-175">A questo punto, non è stato scritto alcun codice client, ma è possibile richiamare l'API Web usando un Web browser o uno strumento di debug HTTP come [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="a7a43-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="a7a43-176">In Visual Studio premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="a7a43-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="a7a43-177">Il Web browser si aprirà `http://localhost:*portnum*/`, dove *portNum* è un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="a7a43-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="a7a43-178">Inviare una richiesta HTTP a "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="a7a43-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="a7a43-179">Il completamento della prima richiesta può risultare lento, perché Entity Framework necessario creare ed eseguire il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="a7a43-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="a7a43-180">La risposta dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="a7a43-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="a7a43-181">[Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a7a43-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
