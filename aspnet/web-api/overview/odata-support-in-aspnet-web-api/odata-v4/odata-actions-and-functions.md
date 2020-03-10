---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData V4 con API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: In OData, le azioni e le funzioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità. Questa esercitazione illustra come...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556224"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="02fe1-104">Azioni e funzioni in OData V4 con API Web ASP.NET 2,2</span><span class="sxs-lookup"><span data-stu-id="02fe1-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="02fe1-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="02fe1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="02fe1-106">In OData, le azioni e le funzioni sono un modo per aggiungere comportamenti lato server che non sono facilmente definiti come operazioni CRUD sulle entità.</span><span class="sxs-lookup"><span data-stu-id="02fe1-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="02fe1-107">Questa esercitazione illustra come aggiungere azioni e funzioni a un endpoint OData V4 usando l'API Web 2,2.</span><span class="sxs-lookup"><span data-stu-id="02fe1-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="02fe1-108">L'esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="02fe1-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="02fe1-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="02fe1-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="02fe1-110">API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="02fe1-110">Web API 2.2</span></span>
> - <span data-ttu-id="02fe1-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="02fe1-111">OData v4</span></span>
> - <span data-ttu-id="02fe1-112">Visual Studio 2013 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="02fe1-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="02fe1-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="02fe1-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="02fe1-114">Versioni di esercitazione</span><span class="sxs-lookup"><span data-stu-id="02fe1-114">Tutorial versions</span></span>
>
> <span data-ttu-id="02fe1-115">Per OData versione 3, vedere [azioni OData in API Web ASP.NET 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="02fe1-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="02fe1-116">La differenza tra le *azioni* e le *funzioni* è che le azioni possono avere effetti collaterali e non le funzioni.</span><span class="sxs-lookup"><span data-stu-id="02fe1-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="02fe1-117">Entrambe le azioni e le funzioni possono restituire i dati.</span><span class="sxs-lookup"><span data-stu-id="02fe1-117">Both actions and functions can return data.</span></span> <span data-ttu-id="02fe1-118">Alcuni usi per le azioni includono:</span><span class="sxs-lookup"><span data-stu-id="02fe1-118">Some uses for actions include:</span></span>

- <span data-ttu-id="02fe1-119">Transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="02fe1-119">Complex transactions.</span></span>
- <span data-ttu-id="02fe1-120">Manipolazione di più entità contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="02fe1-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="02fe1-121">Consentire aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="02fe1-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="02fe1-122">Invio di dati non appartenenti a un'entità.</span><span class="sxs-lookup"><span data-stu-id="02fe1-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="02fe1-123">Le funzioni sono utili per restituire informazioni che non corrispondono direttamente a un'entità o a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="02fe1-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="02fe1-124">Un'azione (o funzione) può essere destinata a una singola entità o a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="02fe1-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="02fe1-125">Nella terminologia OData questa è l' *associazione*.</span><span class="sxs-lookup"><span data-stu-id="02fe1-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="02fe1-126">È anche possibile avere &quot;azioni/funzioni&quot; non associate, chiamate come operazioni statiche sul servizio.</span><span class="sxs-lookup"><span data-stu-id="02fe1-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="02fe1-127">Esempio: aggiunta di un'azione</span><span class="sxs-lookup"><span data-stu-id="02fe1-127">Example: Adding an Action</span></span>

<span data-ttu-id="02fe1-128">Viene ora definita un'azione per valutare un prodotto.</span><span class="sxs-lookup"><span data-stu-id="02fe1-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="02fe1-129">Questa esercitazione si basa sull'esercitazione [creare un endpoint OData V4 usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="02fe1-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="02fe1-130">Per prima cosa, aggiungere un modello di `ProductRating` per rappresentare le classificazioni.</span><span class="sxs-lookup"><span data-stu-id="02fe1-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="02fe1-131">Aggiungere anche un **DbSet** alla classe `ProductsContext`, in modo che EF creerà una tabella Classifications nel database.</span><span class="sxs-lookup"><span data-stu-id="02fe1-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="02fe1-132">Aggiungere l'azione a EDM</span><span class="sxs-lookup"><span data-stu-id="02fe1-132">Add the Action to the EDM</span></span>

<span data-ttu-id="02fe1-133">In WebApiConfig.cs aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="02fe1-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="02fe1-134">Il metodo **EntityTypeConfiguration. Action** aggiunge un'azione a Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="02fe1-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="02fe1-135">Il metodo **Parameter** specifica un parametro tipizzato per l'azione.</span><span class="sxs-lookup"><span data-stu-id="02fe1-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="02fe1-136">Questo codice imposta anche lo spazio dei nomi per EDM.</span><span class="sxs-lookup"><span data-stu-id="02fe1-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="02fe1-137">Lo spazio dei nomi è importante perché l'URI per l'azione include il nome completo dell'azione:</span><span class="sxs-lookup"><span data-stu-id="02fe1-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="02fe1-138">In una tipica configurazione di IIS, il punto in questo URL provocherà la restituzione dell'errore 404 in IIS.</span><span class="sxs-lookup"><span data-stu-id="02fe1-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="02fe1-139">È possibile risolvere il problema aggiungendo la sezione seguente al file Web. config:</span><span class="sxs-lookup"><span data-stu-id="02fe1-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="02fe1-140">Aggiungere un metodo controller per l'azione</span><span class="sxs-lookup"><span data-stu-id="02fe1-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="02fe1-141">Per abilitare l'azione &quot;rate&quot;, aggiungere il metodo seguente per `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="02fe1-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="02fe1-142">Si noti che il nome del metodo corrisponde al nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="02fe1-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="02fe1-143">L'attributo **[HttpPost]** specifica che il metodo è un metodo HTTP post.</span><span class="sxs-lookup"><span data-stu-id="02fe1-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="02fe1-144">Per richiamare l'azione, il client invia una richiesta HTTP POST come la seguente:</span><span class="sxs-lookup"><span data-stu-id="02fe1-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="02fe1-145">Il &quot;frequenza&quot; azione è associato alle istanze del prodotto, quindi l'URI per l'azione è il nome dell'azione completo accodato all'URI dell'entità.</span><span class="sxs-lookup"><span data-stu-id="02fe1-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="02fe1-146">Si ricordi che lo spazio dei nomi EDM è stato impostato su &quot;&quot;ProductService, quindi il nome dell'azione completo è &quot;ProductService. rate&quot;.</span><span class="sxs-lookup"><span data-stu-id="02fe1-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="02fe1-147">Il corpo della richiesta contiene i parametri dell'azione come payload JSON.</span><span class="sxs-lookup"><span data-stu-id="02fe1-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="02fe1-148">L'API Web converte automaticamente il payload JSON in un oggetto **ODataActionParameters** , che è semplicemente un dizionario di valori di parametro.</span><span class="sxs-lookup"><span data-stu-id="02fe1-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="02fe1-149">Utilizzare questo dizionario per accedere ai parametri nel metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="02fe1-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="02fe1-150">Se il client invia i parametri dell'azione nel formato errato, il valore di **ModelState. IsValid** è false.</span><span class="sxs-lookup"><span data-stu-id="02fe1-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="02fe1-151">Controllare questo flag nel metodo del controller e restituire un errore se **IsValid** è false.</span><span class="sxs-lookup"><span data-stu-id="02fe1-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="02fe1-152">Esempio: aggiunta di una funzione</span><span class="sxs-lookup"><span data-stu-id="02fe1-152">Example: Adding a Function</span></span>

<span data-ttu-id="02fe1-153">A questo punto è possibile aggiungere una funzione OData che restituisce il prodotto più costoso.</span><span class="sxs-lookup"><span data-stu-id="02fe1-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="02fe1-154">Come prima, il primo passaggio consiste nell'aggiunta della funzione a EDM.</span><span class="sxs-lookup"><span data-stu-id="02fe1-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="02fe1-155">In WebApiConfig.cs aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="02fe1-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="02fe1-156">In questo caso, la funzione è associata alla raccolta di prodotti, anziché a singole istanze del prodotto.</span><span class="sxs-lookup"><span data-stu-id="02fe1-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="02fe1-157">I client richiamano la funzione inviando una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="02fe1-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="02fe1-158">Ecco il metodo controller per questa funzione:</span><span class="sxs-lookup"><span data-stu-id="02fe1-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="02fe1-159">Si noti che il nome del metodo corrisponde al nome della funzione.</span><span class="sxs-lookup"><span data-stu-id="02fe1-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="02fe1-160">L'attributo **[HttpGet]** specifica che il metodo è un metodo HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="02fe1-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="02fe1-161">La risposta HTTP è la seguente:</span><span class="sxs-lookup"><span data-stu-id="02fe1-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="02fe1-162">Esempio: aggiunta di una funzione non associata</span><span class="sxs-lookup"><span data-stu-id="02fe1-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="02fe1-163">Nell'esempio precedente è stata associata una funzione a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="02fe1-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="02fe1-164">Nell'esempio successivo verrà creata una funzione non *associata* .</span><span class="sxs-lookup"><span data-stu-id="02fe1-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="02fe1-165">Le funzioni non associate vengono chiamate come operazioni statiche sul servizio.</span><span class="sxs-lookup"><span data-stu-id="02fe1-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="02fe1-166">La funzione in questo esempio restituirà le imposte sulle vendite per un determinato codice postale.</span><span class="sxs-lookup"><span data-stu-id="02fe1-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="02fe1-167">Nel file WebApiConfig aggiungere la funzione a EDM:</span><span class="sxs-lookup"><span data-stu-id="02fe1-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="02fe1-168">Si noti che viene chiamata la **funzione** direttamente su **ODataModelBuilder**, invece del tipo di entità o della raccolta.</span><span class="sxs-lookup"><span data-stu-id="02fe1-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="02fe1-169">Indica al generatore di modelli che la funzione non è associata.</span><span class="sxs-lookup"><span data-stu-id="02fe1-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="02fe1-170">Ecco il metodo controller che implementa la funzione:</span><span class="sxs-lookup"><span data-stu-id="02fe1-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="02fe1-171">Non è rilevante quale controller API Web si inserisce questo metodo.</span><span class="sxs-lookup"><span data-stu-id="02fe1-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="02fe1-172">È possibile inserirlo in `ProductsController`o definire un controller separato.</span><span class="sxs-lookup"><span data-stu-id="02fe1-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="02fe1-173">L'attributo **[ODataRoute]** definisce il modello URI per la funzione.</span><span class="sxs-lookup"><span data-stu-id="02fe1-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="02fe1-174">Di seguito è riportato un esempio di richiesta client:</span><span class="sxs-lookup"><span data-stu-id="02fe1-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="02fe1-175">La risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="02fe1-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
