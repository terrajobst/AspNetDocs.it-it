---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing degli attributi in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554985"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="494d3-102">Routing degli attributi in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="494d3-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="494d3-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="494d3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="494d3-104">Il *routing* è il modo in cui l'API Web corrisponde a un URI di un'azione.</span><span class="sxs-lookup"><span data-stu-id="494d3-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="494d3-105">L'API Web 2 supporta un nuovo tipo di routing, denominato *routing degli attributi*.</span><span class="sxs-lookup"><span data-stu-id="494d3-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="494d3-106">Come suggerisce il nome, il routing degli attributi usa gli attributi per definire le route.</span><span class="sxs-lookup"><span data-stu-id="494d3-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="494d3-107">Il routing degli attributi offre un maggiore controllo sugli URI nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="494d3-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="494d3-108">Ad esempio, è possibile creare facilmente URI che descrivono gerarchie di risorse.</span><span class="sxs-lookup"><span data-stu-id="494d3-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="494d3-109">Lo stile di routing precedente, denominato routing basato sulle convenzioni, è ancora completamente supportato.</span><span class="sxs-lookup"><span data-stu-id="494d3-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="494d3-110">Infatti, è possibile combinare entrambe le tecniche nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="494d3-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="494d3-111">In questo argomento viene illustrato come abilitare il routing degli attributi e vengono descritte le diverse opzioni per il routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="494d3-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="494d3-112">Per un'esercitazione end-to-end che usa il routing degli attributi, vedere [creare un'API REST con routing degli attributi nell'API Web 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="494d3-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="494d3-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="494d3-113">Prerequisites</span></span>

<span data-ttu-id="494d3-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="494d3-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="494d3-115">In alternativa, usare Gestione pacchetti NuGet per installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="494d3-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="494d3-116">Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="494d3-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="494d3-117">Immettere il comando seguente nella finestra console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="494d3-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="494d3-118">Perché il routing degli attributi?</span><span class="sxs-lookup"><span data-stu-id="494d3-118">Why Attribute Routing?</span></span>

<span data-ttu-id="494d3-119">La prima versione dell'API Web usava il routing *basato sulle convenzioni* .</span><span class="sxs-lookup"><span data-stu-id="494d3-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="494d3-120">In questo tipo di routing è possibile definire uno o più modelli di route, che sono fondamentalmente stringhe con parametri.</span><span class="sxs-lookup"><span data-stu-id="494d3-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="494d3-121">Quando il Framework riceve una richiesta, corrisponde all'URI rispetto al modello di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="494d3-122">Per ulteriori informazioni sul routing basato su convenzioni, vedere [routing in API Web ASP.NET](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="494d3-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="494d3-123">Un vantaggio del routing basato su convenzioni è che i modelli sono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente in tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="494d3-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="494d3-124">Sfortunatamente, il routing basato su convenzioni rende difficile il supporto di determinati modelli di URI comuni nelle API RESTful.</span><span class="sxs-lookup"><span data-stu-id="494d3-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="494d3-125">Ad esempio, le risorse contengono spesso risorse figlio: i clienti hanno gli ordini, i film hanno attori, i libri hanno autori e così via.</span><span class="sxs-lookup"><span data-stu-id="494d3-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="494d3-126">È naturale creare gli URI che riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="494d3-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="494d3-127">Questo tipo di URI è difficile da creare usando il routing basato su convenzioni.</span><span class="sxs-lookup"><span data-stu-id="494d3-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="494d3-128">Sebbene sia possibile eseguire questa operazione, i risultati non si adattano correttamente se si dispone di molti controller o tipi di risorse.</span><span class="sxs-lookup"><span data-stu-id="494d3-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="494d3-129">Con il routing degli attributi, è semplice definire una route per questo URI.</span><span class="sxs-lookup"><span data-stu-id="494d3-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="494d3-130">È sufficiente aggiungere un attributo all'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="494d3-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="494d3-131">Di seguito sono riportati alcuni altri modelli che rendono semplice il routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="494d3-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="494d3-132">**Controllo delle versioni delle API**</span><span class="sxs-lookup"><span data-stu-id="494d3-132">**API versioning**</span></span>

<span data-ttu-id="494d3-133">In questo esempio, "/API/V1/Products" verrebbe indirizzato a un controller diverso da "/API/v2/Products".</span><span class="sxs-lookup"><span data-stu-id="494d3-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="494d3-134">**Segmenti URI di overload**</span><span class="sxs-lookup"><span data-stu-id="494d3-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="494d3-135">In questo esempio "1" è un numero di ordine, ma "in sospeso" viene mappato a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="494d3-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="494d3-136">**Più tipi di parametro**</span><span class="sxs-lookup"><span data-stu-id="494d3-136">**Multiple parameter types**</span></span>

<span data-ttu-id="494d3-137">In questo esempio "1" è un numero di ordine, ma "2013/06/16" specifica una data.</span><span class="sxs-lookup"><span data-stu-id="494d3-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="494d3-138">Abilitazione del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="494d3-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="494d3-139">Per abilitare il routing degli attributi, chiamare **MapHttpAttributeRoutes** durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="494d3-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="494d3-140">Questo metodo di estensione è definito nella classe **System. Web. http. HttpConfigurationExtensions** .</span><span class="sxs-lookup"><span data-stu-id="494d3-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="494d3-141">Il routing degli attributi può essere combinato con il routing [basato su convenzioni](routing-in-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="494d3-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="494d3-142">Per definire route basate sulle convenzioni, chiamare il metodo **MapHttpRoute** .</span><span class="sxs-lookup"><span data-stu-id="494d3-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="494d3-143">Per ulteriori informazioni sulla configurazione dell'API Web, vedere [configurazione di API Web ASP.NET 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="494d3-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="494d3-144">Nota: migrazione dall'API Web 1</span><span class="sxs-lookup"><span data-stu-id="494d3-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="494d3-145">Prima dell'API Web 2, i modelli di progetto API Web generavano codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="494d3-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="494d3-146">Se il routing degli attributi è abilitato, questo codice genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="494d3-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="494d3-147">Se si aggiorna un progetto API Web esistente per usare il routing degli attributi, assicurarsi di aggiornare il codice di configurazione seguente:</span><span class="sxs-lookup"><span data-stu-id="494d3-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="494d3-148">Per altre informazioni, vedere [configurazione dell'API Web con ASP.NET hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="494d3-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="494d3-149">Aggiunta di attributi di route</span><span class="sxs-lookup"><span data-stu-id="494d3-149">Adding Route Attributes</span></span>

<span data-ttu-id="494d3-150">Di seguito è riportato un esempio di una route definita usando un attributo:</span><span class="sxs-lookup"><span data-stu-id="494d3-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="494d3-151">La stringa &quot;Customers/{customerId}/Orders&quot; è il modello URI per la route.</span><span class="sxs-lookup"><span data-stu-id="494d3-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="494d3-152">L'API Web tenta di trovare la corrispondenza con l'URI della richiesta al modello.</span><span class="sxs-lookup"><span data-stu-id="494d3-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="494d3-153">In questo esempio "Customers" e "Orders" sono segmenti letterali e "{customerId}" è un parametro variabile.</span><span class="sxs-lookup"><span data-stu-id="494d3-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="494d3-154">Gli URI seguenti corrispondono a questo modello:</span><span class="sxs-lookup"><span data-stu-id="494d3-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="494d3-155">È possibile limitare la corrispondenza usando [vincoli](#constraints), descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="494d3-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="494d3-156">Si noti che il parametro &quot;{customerId}&quot; nel modello di route corrisponde al nome del parametro *CustomerID* nel metodo.</span><span class="sxs-lookup"><span data-stu-id="494d3-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="494d3-157">Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="494d3-158">Se, ad esempio, l'URI è `http://example.com/customers/1/orders`, l'API Web tenta di associare il valore "1" al parametro *CustomerID* nell'azione.</span><span class="sxs-lookup"><span data-stu-id="494d3-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="494d3-159">Un modello URI può includere diversi parametri:</span><span class="sxs-lookup"><span data-stu-id="494d3-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="494d3-160">Qualsiasi metodo del controller che non dispone di un attributo di route usa il routing basato su convenzioni.</span><span class="sxs-lookup"><span data-stu-id="494d3-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="494d3-161">In questo modo, è possibile combinare entrambi i tipi di routing nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="494d3-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="494d3-162">Metodi HTTP</span><span class="sxs-lookup"><span data-stu-id="494d3-162">HTTP Methods</span></span>

<span data-ttu-id="494d3-163">L'API Web seleziona anche le azioni basate sul metodo HTTP della richiesta (GET, POST e così via).</span><span class="sxs-lookup"><span data-stu-id="494d3-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="494d3-164">Per impostazione predefinita, l'API Web Cerca una corrispondenza senza distinzione tra maiuscole e minuscole con l'inizio del nome del metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="494d3-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="494d3-165">Ad esempio, un metodo controller denominato `PutCustomers` corrisponde a una richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="494d3-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="494d3-166">È possibile eseguire l'override di questa convenzione decorando il metodo con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="494d3-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="494d3-167">**HttpDelete**</span><span class="sxs-lookup"><span data-stu-id="494d3-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="494d3-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="494d3-168">**[HttpGet]**</span></span>
- <span data-ttu-id="494d3-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="494d3-169">**[HttpHead]**</span></span>
- <span data-ttu-id="494d3-170">**HttpOptions**</span><span class="sxs-lookup"><span data-stu-id="494d3-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="494d3-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="494d3-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="494d3-172">**HttpPost**</span><span class="sxs-lookup"><span data-stu-id="494d3-172">**[HttpPost]**</span></span>
- <span data-ttu-id="494d3-173">**HttpPut**</span><span class="sxs-lookup"><span data-stu-id="494d3-173">**[HttpPut]**</span></span>

<span data-ttu-id="494d3-174">Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="494d3-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="494d3-175">Per tutti gli altri metodi HTTP, inclusi i metodi non standard, usare l'attributo **AcceptVerbs** , che accetta un elenco di metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="494d3-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="494d3-176">Prefissi di route</span><span class="sxs-lookup"><span data-stu-id="494d3-176">Route Prefixes</span></span>

<span data-ttu-id="494d3-177">Le route in un controller iniziano spesso con lo stesso prefisso.</span><span class="sxs-lookup"><span data-stu-id="494d3-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="494d3-178">Esempio:</span><span class="sxs-lookup"><span data-stu-id="494d3-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="494d3-179">È possibile impostare un prefisso comune per un intero controller usando l'attributo **[RoutePrefix]** :</span><span class="sxs-lookup"><span data-stu-id="494d3-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="494d3-180">Usare una tilde (~) nell'attributo del metodo per eseguire l'override del prefisso di route:</span><span class="sxs-lookup"><span data-stu-id="494d3-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="494d3-181">Il prefisso di route può includere parametri:</span><span class="sxs-lookup"><span data-stu-id="494d3-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="494d3-182">Vincoli di route</span><span class="sxs-lookup"><span data-stu-id="494d3-182">Route Constraints</span></span>

<span data-ttu-id="494d3-183">I vincoli di route consentono di limitare la corrispondenza tra i parametri nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="494d3-184">La sintassi generale è &quot;{parameter: Constraint}&quot;.</span><span class="sxs-lookup"><span data-stu-id="494d3-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="494d3-185">Esempio:</span><span class="sxs-lookup"><span data-stu-id="494d3-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="494d3-186">In questo caso, la prima route verrà selezionata solo se l'ID &quot;&quot; segmento dell'URI è un numero intero.</span><span class="sxs-lookup"><span data-stu-id="494d3-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="494d3-187">In caso contrario, verrà scelta la seconda route.</span><span class="sxs-lookup"><span data-stu-id="494d3-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="494d3-188">Nella tabella seguente sono elencati i vincoli supportati.</span><span class="sxs-lookup"><span data-stu-id="494d3-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="494d3-189">Vincolo</span><span class="sxs-lookup"><span data-stu-id="494d3-189">Constraint</span></span> | <span data-ttu-id="494d3-190">Description</span><span class="sxs-lookup"><span data-stu-id="494d3-190">Description</span></span> | <span data-ttu-id="494d3-191">Esempio</span><span class="sxs-lookup"><span data-stu-id="494d3-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="494d3-192">alpha</span><span class="sxs-lookup"><span data-stu-id="494d3-192">alpha</span></span> | <span data-ttu-id="494d3-193">Corrisponde ai caratteri dell'alfabeto latino maiuscolo o minuscolo (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="494d3-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="494d3-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="494d3-194">{x:alpha}</span></span> |
| <span data-ttu-id="494d3-195">bool</span><span class="sxs-lookup"><span data-stu-id="494d3-195">bool</span></span> | <span data-ttu-id="494d3-196">Corrisponde a un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="494d3-196">Matches a Boolean value.</span></span> | <span data-ttu-id="494d3-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="494d3-197">{x:bool}</span></span> |
| <span data-ttu-id="494d3-198">datetime</span><span class="sxs-lookup"><span data-stu-id="494d3-198">datetime</span></span> | <span data-ttu-id="494d3-199">Corrisponde a un valore **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="494d3-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="494d3-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="494d3-200">{x:datetime}</span></span> |
| <span data-ttu-id="494d3-201">decimal</span><span class="sxs-lookup"><span data-stu-id="494d3-201">decimal</span></span> | <span data-ttu-id="494d3-202">Corrisponde a un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="494d3-202">Matches a decimal value.</span></span> | <span data-ttu-id="494d3-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="494d3-203">{x:decimal}</span></span> |
| <span data-ttu-id="494d3-204">double</span><span class="sxs-lookup"><span data-stu-id="494d3-204">double</span></span> | <span data-ttu-id="494d3-205">Corrisponde a un valore a virgola mobile a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="494d3-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="494d3-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="494d3-206">{x:double}</span></span> |
| <span data-ttu-id="494d3-207">float</span><span class="sxs-lookup"><span data-stu-id="494d3-207">float</span></span> | <span data-ttu-id="494d3-208">Corrisponde a un valore a virgola mobile a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="494d3-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="494d3-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="494d3-209">{x:float}</span></span> |
| <span data-ttu-id="494d3-210">guid</span><span class="sxs-lookup"><span data-stu-id="494d3-210">guid</span></span> | <span data-ttu-id="494d3-211">Corrisponde a un valore GUID.</span><span class="sxs-lookup"><span data-stu-id="494d3-211">Matches a GUID value.</span></span> | <span data-ttu-id="494d3-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="494d3-212">{x:guid}</span></span> |
| <span data-ttu-id="494d3-213">int</span><span class="sxs-lookup"><span data-stu-id="494d3-213">int</span></span> | <span data-ttu-id="494d3-214">Corrisponde a un valore integer a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="494d3-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="494d3-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="494d3-215">{x:int}</span></span> |
| <span data-ttu-id="494d3-216">length</span><span class="sxs-lookup"><span data-stu-id="494d3-216">length</span></span> | <span data-ttu-id="494d3-217">Corrisponde a una stringa con la lunghezza specificata o all'interno di un intervallo di lunghezze specificato.</span><span class="sxs-lookup"><span data-stu-id="494d3-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="494d3-218">{x:Length (6)} {x:Length (1, 20)}</span><span class="sxs-lookup"><span data-stu-id="494d3-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="494d3-219">long</span><span class="sxs-lookup"><span data-stu-id="494d3-219">long</span></span> | <span data-ttu-id="494d3-220">Corrisponde a un valore integer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="494d3-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="494d3-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="494d3-221">{x:long}</span></span> |
| <span data-ttu-id="494d3-222">max</span><span class="sxs-lookup"><span data-stu-id="494d3-222">max</span></span> | <span data-ttu-id="494d3-223">Corrisponde a un numero intero con un valore massimo.</span><span class="sxs-lookup"><span data-stu-id="494d3-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="494d3-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="494d3-224">{x:max(10)}</span></span> |
| <span data-ttu-id="494d3-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="494d3-225">maxlength</span></span> | <span data-ttu-id="494d3-226">Trova la corrispondenza di una stringa con una lunghezza massima.</span><span class="sxs-lookup"><span data-stu-id="494d3-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="494d3-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="494d3-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="494d3-228">min</span><span class="sxs-lookup"><span data-stu-id="494d3-228">min</span></span> | <span data-ttu-id="494d3-229">Corrisponde a un numero intero con un valore minimo.</span><span class="sxs-lookup"><span data-stu-id="494d3-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="494d3-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="494d3-230">{x:min(10)}</span></span> |
| <span data-ttu-id="494d3-231">minLength</span><span class="sxs-lookup"><span data-stu-id="494d3-231">minlength</span></span> | <span data-ttu-id="494d3-232">Corrisponde a una stringa con una lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="494d3-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="494d3-233">{x:minLength (10)}</span><span class="sxs-lookup"><span data-stu-id="494d3-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="494d3-234">range</span><span class="sxs-lookup"><span data-stu-id="494d3-234">range</span></span> | <span data-ttu-id="494d3-235">Trova la corrispondenza di un intero in un intervallo di valori.</span><span class="sxs-lookup"><span data-stu-id="494d3-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="494d3-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="494d3-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="494d3-237">regex</span><span class="sxs-lookup"><span data-stu-id="494d3-237">regex</span></span> | <span data-ttu-id="494d3-238">Corrisponde a un'espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="494d3-238">Matches a regular expression.</span></span> | <span data-ttu-id="494d3-239">{x:Regex (^ \d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="494d3-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="494d3-240">Si noti che alcuni vincoli, ad esempio &quot;min&quot;, accettano gli argomenti tra parentesi.</span><span class="sxs-lookup"><span data-stu-id="494d3-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="494d3-241">È possibile applicare più vincoli a un parametro, separati da due punti.</span><span class="sxs-lookup"><span data-stu-id="494d3-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="494d3-242">Vincoli di route personalizzati</span><span class="sxs-lookup"><span data-stu-id="494d3-242">Custom Route Constraints</span></span>

<span data-ttu-id="494d3-243">È possibile creare vincoli di Route personalizzati implementando l'interfaccia **IHttpRouteConstraint** .</span><span class="sxs-lookup"><span data-stu-id="494d3-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="494d3-244">Il vincolo seguente, ad esempio, limita un parametro a un valore integer diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="494d3-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="494d3-245">Nel codice seguente viene illustrato come registrare il vincolo:</span><span class="sxs-lookup"><span data-stu-id="494d3-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="494d3-246">A questo punto è possibile applicare il vincolo nelle route:</span><span class="sxs-lookup"><span data-stu-id="494d3-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="494d3-247">È anche possibile sostituire l'intera classe **DefaultInlineConstraintResolver** implementando l'interfaccia **IInlineConstraintResolver** .</span><span class="sxs-lookup"><span data-stu-id="494d3-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="494d3-248">In questo modo verranno sostituiti tutti i vincoli predefiniti, a meno che l'implementazione di **IInlineConstraintResolver** non li aggiunga in modo specifico.</span><span class="sxs-lookup"><span data-stu-id="494d3-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="494d3-249">Parametri URI facoltativi e valori predefiniti</span><span class="sxs-lookup"><span data-stu-id="494d3-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="494d3-250">È possibile rendere facoltativo un parametro URI aggiungendo un punto interrogativo al parametro di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="494d3-251">Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="494d3-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="494d3-252">In questo esempio `/api/books/locale/1033` e `/api/books/locale` restituiscono la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="494d3-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="494d3-253">In alternativa, è possibile specificare un valore predefinito nel modello di route, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="494d3-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="494d3-254">Questo è quasi uguale all'esempio precedente, ma c'è una lieve differenza di comportamento quando viene applicato il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="494d3-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="494d3-255">Nel primo esempio ("{LCID: int?}"), il valore predefinito di 1033 viene assegnato direttamente al parametro method, quindi il parametro avrà questo valore esatto.</span><span class="sxs-lookup"><span data-stu-id="494d3-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="494d3-256">Nel secondo esempio ("{LCID: int = 1033}"), il valore predefinito di "1033" passa attraverso il processo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="494d3-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="494d3-257">Il gestore di associazione del modello predefinito convertirà "1033" nel valore numerico 1033.</span><span class="sxs-lookup"><span data-stu-id="494d3-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="494d3-258">Tuttavia, è possibile collegare uno strumento di associazione di modelli personalizzato, che potrebbe essere diverso.</span><span class="sxs-lookup"><span data-stu-id="494d3-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="494d3-259">Nella maggior parte dei casi, a meno che nella pipeline non siano presenti agenti di associazione di modelli personalizzati, i due form saranno equivalenti.</span><span class="sxs-lookup"><span data-stu-id="494d3-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="494d3-260">Nomi di route</span><span class="sxs-lookup"><span data-stu-id="494d3-260">Route Names</span></span>

<span data-ttu-id="494d3-261">Nell'API Web ogni route ha un nome.</span><span class="sxs-lookup"><span data-stu-id="494d3-261">In Web API, every route has a name.</span></span> <span data-ttu-id="494d3-262">I nomi di route sono utili per la generazione di collegamenti, in modo che sia possibile includere un collegamento in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="494d3-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="494d3-263">Per specificare il nome della route, impostare la proprietà **Name** dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="494d3-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="494d3-264">Nell'esempio seguente viene illustrato come impostare il nome della route e come utilizzare il nome della route durante la generazione di un collegamento.</span><span class="sxs-lookup"><span data-stu-id="494d3-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="494d3-265">Ordine Route</span><span class="sxs-lookup"><span data-stu-id="494d3-265">Route Order</span></span>

<span data-ttu-id="494d3-266">Quando il Framework tenta di trovare una corrispondenza con un URI con una route, valuta le route in un ordine particolare.</span><span class="sxs-lookup"><span data-stu-id="494d3-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="494d3-267">Per specificare l'ordine, impostare la proprietà **Order** sull'attributo route.</span><span class="sxs-lookup"><span data-stu-id="494d3-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="494d3-268">I valori più bassi vengono valutati per primi.</span><span class="sxs-lookup"><span data-stu-id="494d3-268">Lower values are evaluated first.</span></span> <span data-ttu-id="494d3-269">Il valore dell'ordine predefinito è zero.</span><span class="sxs-lookup"><span data-stu-id="494d3-269">The default order value is zero.</span></span>

<span data-ttu-id="494d3-270">Di seguito è riportato il modo in cui viene determinato l'ordine totale:</span><span class="sxs-lookup"><span data-stu-id="494d3-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="494d3-271">Confrontare la proprietà **Order** dell'attributo route.</span><span class="sxs-lookup"><span data-stu-id="494d3-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="494d3-272">Esaminare ogni segmento URI nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="494d3-273">Per ogni segmento, ordinare come segue:</span><span class="sxs-lookup"><span data-stu-id="494d3-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="494d3-274">Segmenti letterali.</span><span class="sxs-lookup"><span data-stu-id="494d3-274">Literal segments.</span></span>
    2. <span data-ttu-id="494d3-275">Parametri di route con vincoli.</span><span class="sxs-lookup"><span data-stu-id="494d3-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="494d3-276">Parametri di route senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="494d3-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="494d3-277">Segmenti di parametri con caratteri jolly con vincoli.</span><span class="sxs-lookup"><span data-stu-id="494d3-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="494d3-278">Segmenti di parametri con caratteri jolly senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="494d3-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="494d3-279">Nel caso di un tie, le route vengono ordinate in base a un confronto di stringhe ordinali senza distinzione tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.</span><span class="sxs-lookup"><span data-stu-id="494d3-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="494d3-280">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="494d3-280">Here is an example.</span></span> <span data-ttu-id="494d3-281">Si supponga di definire il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="494d3-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="494d3-282">Queste route sono ordinate come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="494d3-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="494d3-283">orders/details</span><span class="sxs-lookup"><span data-stu-id="494d3-283">orders/details</span></span>
2. <span data-ttu-id="494d3-284">ordini/{ID}</span><span class="sxs-lookup"><span data-stu-id="494d3-284">orders/{id}</span></span>
3. <span data-ttu-id="494d3-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="494d3-285">orders/{customerName}</span></span>
4. <span data-ttu-id="494d3-286">ordini/{\*data}</span><span class="sxs-lookup"><span data-stu-id="494d3-286">orders/{\*date}</span></span>
5. <span data-ttu-id="494d3-287">ordini/in sospeso</span><span class="sxs-lookup"><span data-stu-id="494d3-287">orders/pending</span></span>

<span data-ttu-id="494d3-288">Si noti che "Details" è un segmento letterale e viene visualizzato prima di "{ID}", ma "Pending" viene visualizzato per ultimo perché la proprietà **Order** è 1.</span><span class="sxs-lookup"><span data-stu-id="494d3-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="494d3-289">In questo esempio si presuppone che non esistano clienti denominati "Details" o "Pending".</span><span class="sxs-lookup"><span data-stu-id="494d3-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="494d3-290">In generale, provare a evitare Route ambigue.</span><span class="sxs-lookup"><span data-stu-id="494d3-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="494d3-291">In questo esempio, un modello di route migliore per `GetByCustomer` è "Customers/{CustomerName}")</span><span class="sxs-lookup"><span data-stu-id="494d3-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
