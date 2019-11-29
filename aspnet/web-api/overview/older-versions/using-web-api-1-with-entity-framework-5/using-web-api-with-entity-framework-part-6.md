---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: creazione di controller di prodotto e di ordine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600020"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="892d8-102">Parte 6: creazione di controller di prodotto e di ordine</span><span class="sxs-lookup"><span data-stu-id="892d8-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="892d8-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="892d8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="892d8-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="892d8-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="892d8-105">Aggiungere un controller dei prodotti</span><span class="sxs-lookup"><span data-stu-id="892d8-105">Add a Products Controller</span></span>

<span data-ttu-id="892d8-106">Il controller di amministrazione è per gli utenti con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="892d8-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="892d8-107">I clienti, d'altra parte, possono visualizzare i prodotti ma non possono crearli, aggiornarli o eliminarli.</span><span class="sxs-lookup"><span data-stu-id="892d8-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="892d8-108">È possibile limitare facilmente l'accesso ai metodi post, put e DELETE, lasciando aperti i metodi Get.</span><span class="sxs-lookup"><span data-stu-id="892d8-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="892d8-109">Esaminare tuttavia i dati restituiti per un prodotto:</span><span class="sxs-lookup"><span data-stu-id="892d8-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="892d8-110">Il `ActualCost` proprietà non deve essere visibile ai clienti.</span><span class="sxs-lookup"><span data-stu-id="892d8-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="892d8-111">La soluzione consiste nel definire un *oggetto DTO (Data Transfer Object* ) che include un subset di proprietà che devono essere visibili ai clienti.</span><span class="sxs-lookup"><span data-stu-id="892d8-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="892d8-112">Si utilizzerà LINQ per proiettare le istanze di `Product` per `ProductDTO` istanze.</span><span class="sxs-lookup"><span data-stu-id="892d8-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="892d8-113">Aggiungere una classe denominata `ProductDTO` alla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="892d8-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="892d8-114">A questo punto aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="892d8-114">Now add the controller.</span></span> <span data-ttu-id="892d8-115">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="892d8-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="892d8-116">Selezionare **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="892d8-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="892d8-117">Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="892d8-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="892d8-118">In **modello**selezionare **controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="892d8-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="892d8-119">Sostituire tutti gli elementi nel file di origine con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="892d8-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="892d8-120">Il controller usa ancora il `OrdersContext` per eseguire una query sul database.</span><span class="sxs-lookup"><span data-stu-id="892d8-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="892d8-121">Anziché restituire direttamente le istanze di `Product`, viene chiamato `MapProducts` per proiettarle nelle istanze di `ProductDTO`:</span><span class="sxs-lookup"><span data-stu-id="892d8-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="892d8-122">Il metodo `MapProducts` restituisce un oggetto **IQueryable**, quindi è possibile comporre il risultato con altri parametri di query.</span><span class="sxs-lookup"><span data-stu-id="892d8-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="892d8-123">Come si può notare, nel metodo `GetProduct`, che aggiunge una clausola **where** alla query:</span><span class="sxs-lookup"><span data-stu-id="892d8-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="892d8-124">Aggiungere un controller degli ordini</span><span class="sxs-lookup"><span data-stu-id="892d8-124">Add an Orders Controller</span></span>

<span data-ttu-id="892d8-125">Successivamente, aggiungere un controller che consenta agli utenti di creare e visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="892d8-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="892d8-126">Si inizierà con un altro DTO.</span><span class="sxs-lookup"><span data-stu-id="892d8-126">We'll start with another DTO.</span></span> <span data-ttu-id="892d8-127">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe denominata `OrderDTO` utilizzare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="892d8-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="892d8-128">A questo punto aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="892d8-128">Now add the controller.</span></span> <span data-ttu-id="892d8-129">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="892d8-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="892d8-130">Selezionare **Aggiungi**, quindi **controller**.</span><span class="sxs-lookup"><span data-stu-id="892d8-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="892d8-131">Nella finestra di dialogo **Aggiungi controller** impostare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="892d8-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="892d8-132">In **nome controller**immettere "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="892d8-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="892d8-133">In **modello**selezionare "controller API con azioni di lettura/scrittura, utilizzando Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="892d8-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="892d8-134">In **classe modello**selezionare &quot;Order (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="892d8-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="892d8-135">In **classe contesto dati**selezionare &quot;&quot;OrdersContext (ProductStore. Models).</span><span class="sxs-lookup"><span data-stu-id="892d8-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="892d8-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="892d8-136">Click **Add**.</span></span> <span data-ttu-id="892d8-137">Verrà aggiunto un file denominato OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="892d8-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="892d8-138">Successivamente, è necessario modificare l'implementazione predefinita del controller.</span><span class="sxs-lookup"><span data-stu-id="892d8-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="892d8-139">Prima di tutto, eliminare i metodi `PutOrder` e `DeleteOrder`.</span><span class="sxs-lookup"><span data-stu-id="892d8-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="892d8-140">Per questo esempio, i clienti non possono modificare o eliminare gli ordini esistenti.</span><span class="sxs-lookup"><span data-stu-id="892d8-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="892d8-141">In un'applicazione reale, è necessario un numero elevato di logica back-end per gestire questi casi.</span><span class="sxs-lookup"><span data-stu-id="892d8-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="892d8-142">(Ad esempio, l'ordine è già stato spedito?)</span><span class="sxs-lookup"><span data-stu-id="892d8-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="892d8-143">Modificare il metodo `GetOrders` per restituire solo gli ordini che appartengono all'utente:</span><span class="sxs-lookup"><span data-stu-id="892d8-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="892d8-144">Modificare il metodo `GetOrder` come segue:</span><span class="sxs-lookup"><span data-stu-id="892d8-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="892d8-145">Di seguito sono riportate le modifiche apportate al metodo:</span><span class="sxs-lookup"><span data-stu-id="892d8-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="892d8-146">Il valore restituito è un'istanza di `OrderDTO`, anziché una `Order`.</span><span class="sxs-lookup"><span data-stu-id="892d8-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="892d8-147">Quando si esegue una query sul database per l'ordine, viene usato il metodo [DbQuery. include](https://msdn.microsoft.com/library/gg696395) per recuperare le entità `OrderDetail` e `Product` correlate.</span><span class="sxs-lookup"><span data-stu-id="892d8-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="892d8-148">Il risultato viene appiattito usando una proiezione.</span><span class="sxs-lookup"><span data-stu-id="892d8-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="892d8-149">La risposta HTTP conterrà una matrice di prodotti con quantità:</span><span class="sxs-lookup"><span data-stu-id="892d8-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="892d8-150">Questo formato è più semplice per l'utilizzo da parte dei client rispetto all'oggetto grafico originale, che contiene le entità nidificate (Order, Details e Products).</span><span class="sxs-lookup"><span data-stu-id="892d8-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="892d8-151">Ultimo metodo da considerare `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="892d8-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="892d8-152">Al momento, questo metodo accetta un'istanza di `Order`.</span><span class="sxs-lookup"><span data-stu-id="892d8-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="892d8-153">Tuttavia, si consideri cosa accade se un client invia un corpo della richiesta come il seguente:</span><span class="sxs-lookup"><span data-stu-id="892d8-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="892d8-154">Si tratta di un ordine ben strutturato e Entity Framework lo inserirà tranquillamente nel database.</span><span class="sxs-lookup"><span data-stu-id="892d8-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="892d8-155">Contiene tuttavia un'entità Product che non esisteva in precedenza.</span><span class="sxs-lookup"><span data-stu-id="892d8-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="892d8-156">Il client ha appena creato un nuovo prodotto nel database.</span><span class="sxs-lookup"><span data-stu-id="892d8-156">The client just created a new product in our database!</span></span> <span data-ttu-id="892d8-157">Si tratta di una sorpresa per il reparto evasione degli ordini, quando viene visualizzato un ordine per gli orsi Koala.</span><span class="sxs-lookup"><span data-stu-id="892d8-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="892d8-158">Il morale è, prestare molta attenzione ai dati accettati in una richiesta POST o PUT.</span><span class="sxs-lookup"><span data-stu-id="892d8-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="892d8-159">Per evitare questo problema, modificare il metodo `PostOrder` per eseguire un'istanza di `OrderDTO`.</span><span class="sxs-lookup"><span data-stu-id="892d8-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="892d8-160">Usare il `OrderDTO` per creare il `Order`.</span><span class="sxs-lookup"><span data-stu-id="892d8-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="892d8-161">Si noti che si usano le proprietà `ProductID` e `Quantity` e si ignorano tutti i valori inviati dal client per il nome del prodotto o il prezzo.</span><span class="sxs-lookup"><span data-stu-id="892d8-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="892d8-162">Se l'ID prodotto non è valido, verrà violato il vincolo di chiave esterna nel database e l'inserimento avrà esito negativo, come dovrebbe.</span><span class="sxs-lookup"><span data-stu-id="892d8-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="892d8-163">Ecco il metodo di `PostOrder` completo:</span><span class="sxs-lookup"><span data-stu-id="892d8-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="892d8-164">Aggiungere infine l'attributo **autorizza** al controller:</span><span class="sxs-lookup"><span data-stu-id="892d8-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="892d8-165">A questo punto, solo gli utenti registrati possono creare o visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="892d8-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="892d8-166">[Precedente](using-web-api-with-entity-framework-part-5.md)
> [Successivo](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="892d8-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
