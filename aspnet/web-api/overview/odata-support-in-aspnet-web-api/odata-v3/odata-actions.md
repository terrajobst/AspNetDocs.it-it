---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supporto delle azioni OData in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'In OData le azioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Alcuni usi per le azioni includono: implementa...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556357"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="fe29a-104">Supporto delle azioni OData in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="fe29a-104">Supporting OData Actions in ASP.NET Web API 2</span></span>

<span data-ttu-id="fe29a-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fe29a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="fe29a-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="fe29a-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="fe29a-107">In OData le *azioni* sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="fe29a-108">Alcuni usi per le azioni includono:</span><span class="sxs-lookup"><span data-stu-id="fe29a-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="fe29a-109">Implementazione di transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="fe29a-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="fe29a-110">Manipolazione di più entità contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="fe29a-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="fe29a-111">Consentire aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="fe29a-112">Invio di informazioni al server non definito in un'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fe29a-113">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="fe29a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fe29a-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="fe29a-114">Web API 2</span></span>
> - <span data-ttu-id="fe29a-115">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="fe29a-115">OData Version 3</span></span>
> - <span data-ttu-id="fe29a-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="fe29a-116">Entity Framework 6</span></span>

## <a name="example-rating-a-product"></a><span data-ttu-id="fe29a-117">Esempio: valutazione di un prodotto</span><span class="sxs-lookup"><span data-stu-id="fe29a-117">Example: Rating a Product</span></span>

<span data-ttu-id="fe29a-118">In questo esempio si vuole consentire agli utenti di valutare i prodotti e quindi esporre le classificazioni medie per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="fe29a-119">Nel database viene archiviato un elenco di classificazioni, con chiave per i prodotti.</span><span class="sxs-lookup"><span data-stu-id="fe29a-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="fe29a-120">Ecco il modello che è possibile usare per rappresentare le classificazioni in Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="fe29a-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="fe29a-121">Ma non si vuole che i client invii un oggetto `ProductRating` a una raccolta "classificazioni".</span><span class="sxs-lookup"><span data-stu-id="fe29a-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="fe29a-122">In modo intuitivo, la classificazione è associata alla raccolta di prodotti e il client deve solo pubblicare il valore della classificazione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="fe29a-123">Quindi, invece di usare le normali operazioni CRUD, viene definita un'azione che un client può richiamare su un prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="fe29a-124">Nella terminologia OData l'azione è *associata* a entità prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="fe29a-125">Le azioni hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="fe29a-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="fe29a-126">Per questo motivo, vengono richiamati tramite richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="fe29a-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="fe29a-127">Le azioni possono avere parametri e tipi restituiti, descritti nei metadati del servizio.</span><span class="sxs-lookup"><span data-stu-id="fe29a-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="fe29a-128">Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="fe29a-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="fe29a-129">Per richiamare l'azione "rate Product", il client invia un POST a un URI simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe29a-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="fe29a-130">I dati nella richiesta POST sono semplicemente la valutazione del prodotto:</span><span class="sxs-lookup"><span data-stu-id="fe29a-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="fe29a-131">Dichiarare l'azione nell'Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="fe29a-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="fe29a-132">Nella configurazione dell'API Web aggiungere l'azione a Entity Data Model (EDM):</span><span class="sxs-lookup"><span data-stu-id="fe29a-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="fe29a-133">Questo codice definisce "RateProduct" come azione che può essere eseguita sulle entità prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="fe29a-134">Dichiara inoltre che l'azione accetta un parametro **int** denominato "rating" e restituisce un valore **int** .</span><span class="sxs-lookup"><span data-stu-id="fe29a-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="fe29a-135">Aggiungere l'azione al controller</span><span class="sxs-lookup"><span data-stu-id="fe29a-135">Add the Action to the Controller</span></span>

<span data-ttu-id="fe29a-136">L'azione "RateProduct" è associata a entità prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="fe29a-137">Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller dei prodotti:</span><span class="sxs-lookup"><span data-stu-id="fe29a-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="fe29a-138">Si noti che il nome del metodo corrisponde al nome dell'azione nel modello EDM.</span><span class="sxs-lookup"><span data-stu-id="fe29a-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="fe29a-139">Il metodo ha due parametri:</span><span class="sxs-lookup"><span data-stu-id="fe29a-139">The method has two parameters:</span></span>

- <span data-ttu-id="fe29a-140">*Key*: chiave per il prodotto da valutare.</span><span class="sxs-lookup"><span data-stu-id="fe29a-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="fe29a-141">*Parameters*: un dizionario di valori di parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="fe29a-142">Se si utilizzano le convenzioni di routing predefinite, il parametro della chiave deve essere denominato "Key".</span><span class="sxs-lookup"><span data-stu-id="fe29a-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="fe29a-143">È inoltre importante includere l'attributo **[FromOdataUri]** , come illustrato.</span><span class="sxs-lookup"><span data-stu-id="fe29a-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="fe29a-144">Questo attributo indica all'API Web di usare le regole di sintassi OData durante l'analisi della chiave dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="fe29a-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="fe29a-145">Usare il dizionario dei *parametri* per ottenere i parametri dell'azione:</span><span class="sxs-lookup"><span data-stu-id="fe29a-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="fe29a-146">Se il client invia i parametri dell'azione nel formato corretto, il valore di **ModelState. IsValid** è true.</span><span class="sxs-lookup"><span data-stu-id="fe29a-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="fe29a-147">In tal caso, è possibile usare il dizionario **ODataActionParameters** per ottenere i valori dei parametri.</span><span class="sxs-lookup"><span data-stu-id="fe29a-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="fe29a-148">In questo esempio, l'azione `RateProduct` accetta un solo parametro denominato "rating".</span><span class="sxs-lookup"><span data-stu-id="fe29a-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="fe29a-149">Metadati dell'azione</span><span class="sxs-lookup"><span data-stu-id="fe29a-149">Action Metadata</span></span>

<span data-ttu-id="fe29a-150">Per visualizzare i metadati del servizio, inviare una richiesta GET a/OData/$metadata.</span><span class="sxs-lookup"><span data-stu-id="fe29a-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="fe29a-151">Ecco la parte dei metadati che dichiara l'azione `RateProduct`:</span><span class="sxs-lookup"><span data-stu-id="fe29a-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="fe29a-152">L'elemento **FunctionImport** dichiara l'azione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="fe29a-153">La maggior parte dei campi è di chiara interpretazione, ma due sono degni di Nota:</span><span class="sxs-lookup"><span data-stu-id="fe29a-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="fe29a-154">Il **bindingable** indica che l'azione può essere richiamata sull'entità di destinazione, almeno una parte del tempo.</span><span class="sxs-lookup"><span data-stu-id="fe29a-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="fe29a-155">**IsAlwaysBindable** indica che l'azione può essere sempre richiamata sull'entità di destinazione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="fe29a-156">La differenza è che alcune azioni sono sempre disponibili per i client, mentre altre azioni potrebbero dipendere dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="fe29a-157">Si supponga, ad esempio, di definire un'azione di "acquisto".</span><span class="sxs-lookup"><span data-stu-id="fe29a-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="fe29a-158">È possibile acquistare solo un elemento in magazzino.</span><span class="sxs-lookup"><span data-stu-id="fe29a-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="fe29a-159">Se l'elemento non è disponibile, un client non è in grado di richiamare tale azione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="fe29a-160">Quando si definisce EDM, il metodo di **azione** crea un'azione sempre associabile:</span><span class="sxs-lookup"><span data-stu-id="fe29a-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="fe29a-161">Più avanti in questo argomento verranno illustrate le azioni non sempre associabili, denominate anche azioni *temporanee* .</span><span class="sxs-lookup"><span data-stu-id="fe29a-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="fe29a-162">Richiamo dell'azione</span><span class="sxs-lookup"><span data-stu-id="fe29a-162">Invoking the Action</span></span>

<span data-ttu-id="fe29a-163">Vediamo ora come un client richiama questa azione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="fe29a-164">Si supponga che il client desideri assegnare una classificazione pari a 2 al prodotto con ID = 4.</span><span class="sxs-lookup"><span data-stu-id="fe29a-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="fe29a-165">Di seguito è riportato un esempio di messaggio di richiesta, usando il formato JSON per il corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="fe29a-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="fe29a-166">Di seguito è riportato il messaggio di risposta:</span><span class="sxs-lookup"><span data-stu-id="fe29a-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="fe29a-167">Associazione di un'azione a un set di entità</span><span class="sxs-lookup"><span data-stu-id="fe29a-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="fe29a-168">Nell'esempio precedente l'azione è associata a una singola entità: il client valuta un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe29a-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="fe29a-169">È anche possibile associare un'azione a una raccolta di entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="fe29a-170">È sufficiente apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe29a-170">Just make the following changes:</span></span>

<span data-ttu-id="fe29a-171">In EDM aggiungere l'azione alla proprietà della **raccolta** dell'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="fe29a-172">Nel metodo controller omettere il parametro *Key* .</span><span class="sxs-lookup"><span data-stu-id="fe29a-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="fe29a-173">A questo punto il client richiama l'azione sul set di entità Products:</span><span class="sxs-lookup"><span data-stu-id="fe29a-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="fe29a-174">Azioni con parametri di raccolta</span><span class="sxs-lookup"><span data-stu-id="fe29a-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="fe29a-175">Le azioni possono avere parametri che accettano una raccolta di valori.</span><span class="sxs-lookup"><span data-stu-id="fe29a-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="fe29a-176">In EDM usare **CollectionParameter&lt;t&gt;** per dichiarare il parametro.</span><span class="sxs-lookup"><span data-stu-id="fe29a-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="fe29a-177">Viene dichiarato un parametro denominato "ratings" che accetta una raccolta di valori **int** .</span><span class="sxs-lookup"><span data-stu-id="fe29a-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="fe29a-178">Nel metodo controller si ottiene comunque il valore del parametro dall'oggetto **ODataActionParameters** , ma ora il valore è un valore **ICollection&lt;int&gt;** :</span><span class="sxs-lookup"><span data-stu-id="fe29a-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="fe29a-179">Azioni temporanee</span><span class="sxs-lookup"><span data-stu-id="fe29a-179">Transient Actions</span></span>

<span data-ttu-id="fe29a-180">Nell'esempio "RateProduct" gli utenti possono sempre valutare un prodotto, pertanto l'azione è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="fe29a-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="fe29a-181">Tuttavia, alcune azioni dipendono dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="fe29a-182">Ad esempio, in un servizio di noleggio video, l'azione "Estrai" non è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="fe29a-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="fe29a-183">Dipende dal fatto che sia disponibile una copia del video. Questo tipo di azione è denominato azione *temporanea* .</span><span class="sxs-lookup"><span data-stu-id="fe29a-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="fe29a-184">Nei metadati del servizio, un'azione temporanea ha **IsAlwaysBindable** uguale a false.</span><span class="sxs-lookup"><span data-stu-id="fe29a-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="fe29a-185">Si tratta in realtà del valore predefinito, quindi i metadati sono simili ai seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe29a-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="fe29a-186">Ecco perché è importante: se un'azione è temporanea, il server deve indicare al client quando l'azione è disponibile.</span><span class="sxs-lookup"><span data-stu-id="fe29a-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="fe29a-187">A tale scopo, include un collegamento all'azione nell'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="fe29a-188">Di seguito è riportato un esempio di un'entità Movie:</span><span class="sxs-lookup"><span data-stu-id="fe29a-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="fe29a-189">La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="fe29a-190">Se l'azione non è disponibile, il server omette il collegamento.</span><span class="sxs-lookup"><span data-stu-id="fe29a-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="fe29a-191">Per dichiarare un'azione temporanea in EDM, chiamare il metodo **TransientAction** :</span><span class="sxs-lookup"><span data-stu-id="fe29a-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="fe29a-192">Inoltre, è necessario fornire una funzione che restituisca un collegamento all'azione per un'entità specificata.</span><span class="sxs-lookup"><span data-stu-id="fe29a-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="fe29a-193">Impostare questa funzione chiamando **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="fe29a-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="fe29a-194">È possibile scrivere la funzione come espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="fe29a-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="fe29a-195">Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione.</span><span class="sxs-lookup"><span data-stu-id="fe29a-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="fe29a-196">Il serializzatore OData include questo collegamento durante la serializzazione dell'entità.</span><span class="sxs-lookup"><span data-stu-id="fe29a-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="fe29a-197">Quando l'azione non è disponibile, la funzione restituisce `null`.</span><span class="sxs-lookup"><span data-stu-id="fe29a-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe29a-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fe29a-198">Additional Resources</span></span>

[<span data-ttu-id="fe29a-199">Esempio di azioni OData</span><span class="sxs-lookup"><span data-stu-id="fe29a-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
