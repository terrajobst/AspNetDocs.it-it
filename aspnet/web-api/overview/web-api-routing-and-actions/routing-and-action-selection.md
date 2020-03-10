---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Selezione dell'azione e del routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554887"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="c09aa-102">Selezione di routing e azione in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c09aa-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="c09aa-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c09aa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c09aa-104">Questo articolo descrive il modo in cui API Web ASP.NET instrada una richiesta HTTP a una determinata azione su un controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="c09aa-105">Per una panoramica di alto livello del routing, vedere [routing in API Web ASP.NET](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c09aa-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="c09aa-106">In questo articolo vengono esaminati i dettagli del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="c09aa-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="c09aa-107">Se si crea un progetto API Web e si scopre che alcune richieste non vengono instradate nel modo previsto, questo articolo è utile.</span><span class="sxs-lookup"><span data-stu-id="c09aa-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="c09aa-108">Il routing prevede tre fasi principali:</span><span class="sxs-lookup"><span data-stu-id="c09aa-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="c09aa-109">Corrispondenza dell'URI con un modello di route.</span><span class="sxs-lookup"><span data-stu-id="c09aa-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="c09aa-110">Selezione di un controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-110">Selecting a controller.</span></span>
3. <span data-ttu-id="c09aa-111">Selezione di un'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-111">Selecting an action.</span></span>

<span data-ttu-id="c09aa-112">È possibile sostituire alcune parti del processo con comportamenti personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c09aa-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="c09aa-113">In questo articolo viene descritto il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="c09aa-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="c09aa-114">Alla fine, annotare le posizioni in cui è possibile personalizzare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="c09aa-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="c09aa-115">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="c09aa-115">Route Templates</span></span>

<span data-ttu-id="c09aa-116">Un modello di route ha un aspetto simile a quello di un percorso URI, ma può avere valori segnaposto, indicati con parentesi graffe:</span><span class="sxs-lookup"><span data-stu-id="c09aa-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="c09aa-117">Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:</span><span class="sxs-lookup"><span data-stu-id="c09aa-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="c09aa-118">È anche possibile fornire vincoli che limitano il modo in cui un segmento URI può corrispondere a un segnaposto:</span><span class="sxs-lookup"><span data-stu-id="c09aa-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="c09aa-119">Il Framework tenta di far corrispondere i segmenti nel percorso URI al modello.</span><span class="sxs-lookup"><span data-stu-id="c09aa-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="c09aa-120">I valori letterali nel modello devono corrispondere esattamente.</span><span class="sxs-lookup"><span data-stu-id="c09aa-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="c09aa-121">Un segnaposto corrisponde a qualsiasi valore, a meno che non si specifichino i vincoli.</span><span class="sxs-lookup"><span data-stu-id="c09aa-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="c09aa-122">Il Framework non corrisponde ad altre parti dell'URI, ad esempio il nome host o i parametri della query.</span><span class="sxs-lookup"><span data-stu-id="c09aa-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="c09aa-123">Il Framework seleziona la prima route nella tabella di route corrispondente all'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="c09aa-124">Sono presenti due segnaposto speciali: "{controller}" e "{Action}".</span><span class="sxs-lookup"><span data-stu-id="c09aa-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="c09aa-125">"{controller}" fornisce il nome del controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="c09aa-126">"{Action}" fornisce il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="c09aa-127">Nell'API Web, la convenzione usuale consiste nel omettere "{Action}".</span><span class="sxs-lookup"><span data-stu-id="c09aa-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="c09aa-128">attività</span><span class="sxs-lookup"><span data-stu-id="c09aa-128">Defaults</span></span>

<span data-ttu-id="c09aa-129">Se si specificano le impostazioni predefinite, la route corrisponderà a un URI privo di questi segmenti.</span><span class="sxs-lookup"><span data-stu-id="c09aa-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="c09aa-130">Esempio:</span><span class="sxs-lookup"><span data-stu-id="c09aa-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="c09aa-131">Gli URI `http://localhost/api/products/all` e `http://localhost/api/products` corrispondono alla route precedente.</span><span class="sxs-lookup"><span data-stu-id="c09aa-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="c09aa-132">Nel secondo URI, al segmento di `{category}` mancante viene assegnato il valore predefinito `all`.</span><span class="sxs-lookup"><span data-stu-id="c09aa-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="c09aa-133">Dizionario di route</span><span class="sxs-lookup"><span data-stu-id="c09aa-133">Route Dictionary</span></span>

<span data-ttu-id="c09aa-134">Se il Framework trova una corrispondenza per un URI, viene creato un dizionario che contiene il valore per ogni segnaposto.</span><span class="sxs-lookup"><span data-stu-id="c09aa-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="c09aa-135">Le chiavi sono i nomi di segnaposto, escluse le parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="c09aa-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="c09aa-136">I valori vengono ricavati dal percorso URI o dalle impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="c09aa-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="c09aa-137">Il dizionario viene archiviato nell'oggetto **IHttpRouteData** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="c09aa-138">Durante questa fase di corrispondenza della route, i segnaposto "{controller}" e "{Action}" speciali vengono considerati come gli altri segnaposto.</span><span class="sxs-lookup"><span data-stu-id="c09aa-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="c09aa-139">Vengono semplicemente archiviati nel dizionario con gli altri valori.</span><span class="sxs-lookup"><span data-stu-id="c09aa-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="c09aa-140">Un valore predefinito può avere il valore speciale **RouteParameter. optional**.</span><span class="sxs-lookup"><span data-stu-id="c09aa-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="c09aa-141">Se a un segnaposto viene assegnato questo valore, il valore non viene aggiunto al dizionario di route.</span><span class="sxs-lookup"><span data-stu-id="c09aa-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="c09aa-142">Esempio:</span><span class="sxs-lookup"><span data-stu-id="c09aa-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="c09aa-143">Per il percorso URI "API/Products", il dizionario di route conterrà:</span><span class="sxs-lookup"><span data-stu-id="c09aa-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="c09aa-144">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="c09aa-144">controller: "products"</span></span>
- <span data-ttu-id="c09aa-145">category: "all"</span><span class="sxs-lookup"><span data-stu-id="c09aa-145">category: "all"</span></span>

<span data-ttu-id="c09aa-146">Per "API/Products/Toys/123", tuttavia, il dizionario di route conterrà:</span><span class="sxs-lookup"><span data-stu-id="c09aa-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="c09aa-147">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="c09aa-147">controller: "products"</span></span>
- <span data-ttu-id="c09aa-148">category: "toys"</span><span class="sxs-lookup"><span data-stu-id="c09aa-148">category: "toys"</span></span>
- <span data-ttu-id="c09aa-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="c09aa-149">id: "123"</span></span>

<span data-ttu-id="c09aa-150">Le impostazioni predefinite possono includere anche un valore che non viene visualizzato in un punto qualsiasi del modello di route.</span><span class="sxs-lookup"><span data-stu-id="c09aa-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="c09aa-151">Se la route corrisponde, il valore viene archiviato nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="c09aa-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="c09aa-152">Esempio:</span><span class="sxs-lookup"><span data-stu-id="c09aa-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="c09aa-153">Se il percorso URI è "API/root/8", il dizionario conterrà due valori:</span><span class="sxs-lookup"><span data-stu-id="c09aa-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="c09aa-154">controller: "Customers"</span><span class="sxs-lookup"><span data-stu-id="c09aa-154">controller: "customers"</span></span>
- <span data-ttu-id="c09aa-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="c09aa-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="c09aa-156">Selezione di un controller</span><span class="sxs-lookup"><span data-stu-id="c09aa-156">Selecting a Controller</span></span>

<span data-ttu-id="c09aa-157">La selezione del controller viene gestita dal metodo **IHttpControllerSelector. SelectController** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="c09aa-158">Questo metodo accetta un'istanza di **HttpRequestMessage** e restituisce un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="c09aa-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="c09aa-159">L'implementazione predefinita viene fornita dalla classe **DefaultHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="c09aa-160">Questa classe usa un algoritmo semplice:</span><span class="sxs-lookup"><span data-stu-id="c09aa-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="c09aa-161">Esaminare il dizionario di route per la chiave "controller".</span><span class="sxs-lookup"><span data-stu-id="c09aa-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="c09aa-162">Prendere il valore della chiave e aggiungere la stringa "controller" per ottenere il nome del tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="c09aa-163">Cercare un controller API Web con questo nome di tipo.</span><span class="sxs-lookup"><span data-stu-id="c09aa-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="c09aa-164">Se, ad esempio, il dizionario di route contiene la coppia chiave-valore "controller" = "prodotti", il tipo di controller è "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="c09aa-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="c09aa-165">Se non esiste un tipo corrispondente o più corrispondenze, il Framework restituisce un errore al client.</span><span class="sxs-lookup"><span data-stu-id="c09aa-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="c09aa-166">Per il passaggio 3, **DefaultHttpControllerSelector** usa l'interfaccia **IHttpControllerTypeResolver** per ottenere l'elenco dei tipi di controller API Web.</span><span class="sxs-lookup"><span data-stu-id="c09aa-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="c09aa-167">L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che (a) implementano **IHttpController**, (b) non sono astratte e (c) hanno un nome che termina con "controller".</span><span class="sxs-lookup"><span data-stu-id="c09aa-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="c09aa-168">Selezione azione</span><span class="sxs-lookup"><span data-stu-id="c09aa-168">Action Selection</span></span>

<span data-ttu-id="c09aa-169">Dopo aver selezionato il controller, il Framework seleziona l'azione chiamando il metodo **IHttpActionSelector. SelectAction** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="c09aa-170">Questo metodo accetta un **HttpControllerContext** e restituisce un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="c09aa-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="c09aa-171">L'implementazione predefinita viene fornita dalla classe **ApiControllerActionSelector** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="c09aa-172">Per selezionare un'azione, viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c09aa-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="c09aa-173">Metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c09aa-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="c09aa-174">Segnaposto "{Action}" nel modello di route, se presente.</span><span class="sxs-lookup"><span data-stu-id="c09aa-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="c09aa-175">Parametri delle azioni sul controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="c09aa-176">Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti delle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="c09aa-177">**Quali metodi sul controller sono considerati "azioni"?**</span><span class="sxs-lookup"><span data-stu-id="c09aa-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="c09aa-178">Quando si seleziona un'azione, il Framework esamina solo i metodi di istanza pubblica sul controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="c09aa-179">Esclude inoltre i metodi ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (costruttori, eventi, overload degli operatori e così via) e i metodi ereditati dalla classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="c09aa-180">**Metodi HTTP.**</span><span class="sxs-lookup"><span data-stu-id="c09aa-180">**HTTP Methods.**</span></span> <span data-ttu-id="c09aa-181">Il Framework sceglie solo le azioni che corrispondono al metodo HTTP della richiesta, determinato nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="c09aa-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="c09aa-182">È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**o **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="c09aa-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="c09aa-183">In caso contrario, se il nome del metodo del controller inizia con "Get", "post", "put", "Delete", "Head", "Options" o "patch", per convenzione l'azione supporta tale metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="c09aa-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="c09aa-184">Se nessuno dei precedenti, il metodo supporta POST.</span><span class="sxs-lookup"><span data-stu-id="c09aa-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="c09aa-185">**Associazioni di parametri.**</span><span class="sxs-lookup"><span data-stu-id="c09aa-185">**Parameter Bindings.**</span></span> <span data-ttu-id="c09aa-186">Un'associazione di parametri è il modo in cui l'API Web crea un valore per un parametro.</span><span class="sxs-lookup"><span data-stu-id="c09aa-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="c09aa-187">Di seguito è illustrata la regola predefinita per l'associazione di parametri:</span><span class="sxs-lookup"><span data-stu-id="c09aa-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="c09aa-188">I tipi semplici vengono ricavati dall'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="c09aa-189">I tipi complessi vengono ricavati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c09aa-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="c09aa-190">I tipi semplici includono tutti i [.NET Framework tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive), più **DateTime**, **Decimal**, **GUID**, **String**e **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="c09aa-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="c09aa-191">Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c09aa-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="c09aa-192">È possibile eseguire l'override delle regole di binding predefinite.</span><span class="sxs-lookup"><span data-stu-id="c09aa-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="c09aa-193">Vedere [binding di parametri WebAPI dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="c09aa-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="c09aa-194">Con questo background, ecco l'algoritmo di selezione delle azioni.</span><span class="sxs-lookup"><span data-stu-id="c09aa-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="c09aa-195">Creare un elenco di tutte le azioni sul controller che corrispondono al metodo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c09aa-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="c09aa-196">Se il dizionario di route include una voce "Action", rimuovere le azioni il cui nome non corrisponde a questo valore.</span><span class="sxs-lookup"><span data-stu-id="c09aa-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="c09aa-197">Provare a trovare una corrispondenza tra i parametri di azione e l'URI, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c09aa-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="c09aa-198">Per ogni azione, ottenere un elenco dei parametri che sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="c09aa-199">Escludere i parametri facoltativi.</span><span class="sxs-lookup"><span data-stu-id="c09aa-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="c09aa-200">Da questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, nel dizionario di route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="c09aa-201">Le corrispondenze non fanno distinzione tra maiuscole e minuscole e non dipendono dall'ordine dei parametri.</span><span class="sxs-lookup"><span data-stu-id="c09aa-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="c09aa-202">Consente di selezionare un'azione in cui ogni parametro dell'elenco presenta una corrispondenza nell'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="c09aa-203">Se più di un'azione soddisfa questi criteri, scegliere quella con la maggior parte dei parametri corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="c09aa-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="c09aa-204">Ignorare le azioni con l'attributo **[NonAction]** .</span><span class="sxs-lookup"><span data-stu-id="c09aa-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="c09aa-205">Il passaggio #3 è probabilmente il più confuso.</span><span class="sxs-lookup"><span data-stu-id="c09aa-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="c09aa-206">Il concetto di base è che un parametro può ottenere il proprio valore dall'URI, dal corpo della richiesta o da un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="c09aa-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="c09aa-207">Per i parametri che provengono dall'URI, è necessario assicurarsi che l'URI contenga effettivamente un valore per il parametro, sia nel percorso (tramite il dizionario di route) che nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c09aa-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="c09aa-208">Si consideri, ad esempio, l'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="c09aa-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="c09aa-209">Il parametro *ID* viene associato all'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="c09aa-210">Pertanto, questa azione può corrispondere solo a un URI che contiene un valore per "ID", nel dizionario di route o nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c09aa-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="c09aa-211">I parametri facoltativi sono un'eccezione, perché sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="c09aa-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="c09aa-212">Per un parametro facoltativo, è accettabile se l'associazione non è in grado di ottenere il valore dall'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="c09aa-213">I tipi complessi costituiscono un'eccezione per un motivo diverso.</span><span class="sxs-lookup"><span data-stu-id="c09aa-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="c09aa-214">Un tipo complesso può essere associato solo all'URI tramite un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="c09aa-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="c09aa-215">In tal caso, tuttavia, il Framework non è in grado di stabilire in anticipo se il parametro verrebbe associato a un particolare URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="c09aa-216">Per scoprirlo, è necessario richiamare l'associazione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="c09aa-217">L'obiettivo dell'algoritmo di selezione consiste nel selezionare un'azione dalla descrizione statica, prima di richiamare qualsiasi binding.</span><span class="sxs-lookup"><span data-stu-id="c09aa-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="c09aa-218">Pertanto, i tipi complessi vengono esclusi dall'algoritmo corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c09aa-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="c09aa-219">Dopo aver selezionato l'azione, vengono richiamate tutte le associazioni di parametri.</span><span class="sxs-lookup"><span data-stu-id="c09aa-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="c09aa-220">Riepilogo:</span><span class="sxs-lookup"><span data-stu-id="c09aa-220">Summary:</span></span>

- <span data-ttu-id="c09aa-221">L'azione deve corrispondere al metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c09aa-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="c09aa-222">Il nome dell'azione deve corrispondere alla voce "Action" nel dizionario di route, se presente.</span><span class="sxs-lookup"><span data-stu-id="c09aa-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="c09aa-223">Per ogni parametro dell'azione, se il parametro viene tratto dall'URI, il nome del parametro deve trovarsi nel dizionario di route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="c09aa-224">I parametri e i parametri facoltativi con tipi complessi vengono esclusi.</span><span class="sxs-lookup"><span data-stu-id="c09aa-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="c09aa-225">Provare a trovare la corrispondenza con il numero massimo di parametri.</span><span class="sxs-lookup"><span data-stu-id="c09aa-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="c09aa-226">La corrispondenza migliore potrebbe essere un metodo senza parametri.</span><span class="sxs-lookup"><span data-stu-id="c09aa-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="c09aa-227">Esempio esteso</span><span class="sxs-lookup"><span data-stu-id="c09aa-227">Extended Example</span></span>

<span data-ttu-id="c09aa-228">Percorsi</span><span class="sxs-lookup"><span data-stu-id="c09aa-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="c09aa-229">Controller:</span><span class="sxs-lookup"><span data-stu-id="c09aa-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="c09aa-230">Richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="c09aa-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="c09aa-231">Corrispondenza Route</span><span class="sxs-lookup"><span data-stu-id="c09aa-231">Route Matching</span></span>

<span data-ttu-id="c09aa-232">L'URI corrisponde alla route denominata "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="c09aa-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="c09aa-233">Il dizionario di route contiene le voci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c09aa-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="c09aa-234">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="c09aa-234">controller: "products"</span></span>
- <span data-ttu-id="c09aa-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="c09aa-235">id: "1"</span></span>

<span data-ttu-id="c09aa-236">Il dizionario di route non contiene i parametri della stringa di query "Version" e "Details", ma questi verranno comunque considerati durante la selezione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="c09aa-237">Selezione controller</span><span class="sxs-lookup"><span data-stu-id="c09aa-237">Controller Selection</span></span>

<span data-ttu-id="c09aa-238">Dalla voce "controller" nel dizionario di route, il tipo di controller è `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c09aa-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="c09aa-239">Selezione azione</span><span class="sxs-lookup"><span data-stu-id="c09aa-239">Action Selection</span></span>

<span data-ttu-id="c09aa-240">La richiesta HTTP è una richiesta GET.</span><span class="sxs-lookup"><span data-stu-id="c09aa-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="c09aa-241">Le azioni del controller che supportano GET sono `GetAll`, `GetById`e `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="c09aa-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="c09aa-242">Il dizionario di route non contiene una voce per "Action", quindi non è necessario che corrisponda al nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="c09aa-243">Si tenta quindi di trovare la corrispondenza con i nomi dei parametri per le azioni, cercando solo le azioni GET.</span><span class="sxs-lookup"><span data-stu-id="c09aa-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="c09aa-244">Azione</span><span class="sxs-lookup"><span data-stu-id="c09aa-244">Action</span></span> | <span data-ttu-id="c09aa-245">Parametri di cui trovare una corrispondenza</span><span class="sxs-lookup"><span data-stu-id="c09aa-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="c09aa-246">nessuno</span><span class="sxs-lookup"><span data-stu-id="c09aa-246">none</span></span> |
| `GetById` | <span data-ttu-id="c09aa-247">"id"</span><span class="sxs-lookup"><span data-stu-id="c09aa-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="c09aa-248">nome</span><span class="sxs-lookup"><span data-stu-id="c09aa-248">"name"</span></span> |

<span data-ttu-id="c09aa-249">Si noti che il parametro *Version* di `GetById` non viene considerato perché è un parametro facoltativo.</span><span class="sxs-lookup"><span data-stu-id="c09aa-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="c09aa-250">Il metodo `GetAll` corrisponde in modo banale.</span><span class="sxs-lookup"><span data-stu-id="c09aa-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="c09aa-251">Anche il metodo `GetById` corrisponde, perché il dizionario di route contiene "ID".</span><span class="sxs-lookup"><span data-stu-id="c09aa-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="c09aa-252">Il metodo `FindProductsByName` non corrisponde a.</span><span class="sxs-lookup"><span data-stu-id="c09aa-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="c09aa-253">Il metodo `GetById` prevale, perché corrisponde a un parametro e non a parametri per `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="c09aa-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="c09aa-254">Il metodo viene richiamato con i valori di parametro seguenti:</span><span class="sxs-lookup"><span data-stu-id="c09aa-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="c09aa-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="c09aa-255">*id* = 1</span></span>
- <span data-ttu-id="c09aa-256">*versione* = 1,5</span><span class="sxs-lookup"><span data-stu-id="c09aa-256">*version* = 1.5</span></span>

<span data-ttu-id="c09aa-257">Si noti che anche se la *versione* non è stata usata nell'algoritmo di selezione, il valore del parametro deriva dalla stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="c09aa-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="c09aa-258">Punti di estensione</span><span class="sxs-lookup"><span data-stu-id="c09aa-258">Extension Points</span></span>

<span data-ttu-id="c09aa-259">L'API Web fornisce punti di estensione per alcune parti del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="c09aa-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="c09aa-260">Interfaccia</span><span class="sxs-lookup"><span data-stu-id="c09aa-260">Interface</span></span> | <span data-ttu-id="c09aa-261">Description</span><span class="sxs-lookup"><span data-stu-id="c09aa-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c09aa-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="c09aa-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="c09aa-263">Consente di selezionare il controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-263">Selects the controller.</span></span> |
| <span data-ttu-id="c09aa-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="c09aa-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="c09aa-265">Ottiene l'elenco dei tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-265">Gets the list of controller types.</span></span> <span data-ttu-id="c09aa-266">**DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco.</span><span class="sxs-lookup"><span data-stu-id="c09aa-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="c09aa-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="c09aa-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="c09aa-268">Ottiene l'elenco di assembly del progetto.</span><span class="sxs-lookup"><span data-stu-id="c09aa-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="c09aa-269">L'interfaccia **IHttpControllerTypeResolver** usa questo elenco per trovare i tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="c09aa-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="c09aa-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="c09aa-271">Crea nuove istanze del controller.</span><span class="sxs-lookup"><span data-stu-id="c09aa-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="c09aa-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="c09aa-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="c09aa-273">Seleziona l'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-273">Selects the action.</span></span> |
| <span data-ttu-id="c09aa-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="c09aa-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="c09aa-275">Richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="c09aa-275">Invokes the action.</span></span> |

<span data-ttu-id="c09aa-276">Per fornire un'implementazione personalizzata per una di queste interfacce, utilizzare la raccolta **Services** nell'oggetto **HttpConfiguration** :</span><span class="sxs-lookup"><span data-stu-id="c09aa-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
