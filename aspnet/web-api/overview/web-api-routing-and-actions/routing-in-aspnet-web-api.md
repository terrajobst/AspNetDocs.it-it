---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557610"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="e3af8-102">Routing in ASP.NET Web API (in inglese)</span><span class="sxs-lookup"><span data-stu-id="e3af8-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="e3af8-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3af8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e3af8-104">Questo articolo descrive il modo in cui API Web ASP.NET instrada le richieste HTTP ai controller.</span><span class="sxs-lookup"><span data-stu-id="e3af8-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="e3af8-105">Se si ha familiarità con ASP.NET MVC, il routing dell'API Web è molto simile al routing MVC.</span><span class="sxs-lookup"><span data-stu-id="e3af8-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="e3af8-106">La differenza principale è che l'API Web usa il verbo HTTP, non il percorso URI, per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="e3af8-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="e3af8-107">È anche possibile usare il routing di tipo MVC nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="e3af8-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="e3af8-108">Questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3af8-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="e3af8-109">Tabelle di routing</span><span class="sxs-lookup"><span data-stu-id="e3af8-109">Routing Tables</span></span>

<span data-ttu-id="e3af8-110">In API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3af8-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="e3af8-111">I metodi pubblici del controller sono denominati *metodi di azione* o semplicemente *azioni*.</span><span class="sxs-lookup"><span data-stu-id="e3af8-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="e3af8-112">Quando il Framework API Web riceve una richiesta, instrada la richiesta a un'azione.</span><span class="sxs-lookup"><span data-stu-id="e3af8-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="e3af8-113">Per determinare l'azione da richiamare, il Framework utilizza una *tabella di routing*.</span><span class="sxs-lookup"><span data-stu-id="e3af8-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="e3af8-114">Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:</span><span class="sxs-lookup"><span data-stu-id="e3af8-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e3af8-115">Questa route viene definita nel file *WebApiConfig.cs* , che viene inserito nell' *app\_directory iniziale* :</span><span class="sxs-lookup"><span data-stu-id="e3af8-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e3af8-116">Per ulteriori informazioni sulla classe `WebApiConfig`, vedere [Configuring API Web ASP.NET](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e3af8-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="e3af8-117">Se l'API Web viene ospitata autonomamente, è necessario impostare la tabella di routing direttamente nell'oggetto `HttpSelfHostConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="e3af8-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="e3af8-118">Per ulteriori informazioni, vedere la pagina relativa all' [hosting automatico di un'API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e3af8-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="e3af8-119">Ogni voce della tabella di routing contiene un *modello di route*.</span><span class="sxs-lookup"><span data-stu-id="e3af8-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="e3af8-120">Il modello di route predefinito per l'API Web è &quot;API/{controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3af8-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="e3af8-121">In questo modello &quot;API&quot; è un segmento di percorso letterale e {controller} e {ID} sono variabili segnaposto.</span><span class="sxs-lookup"><span data-stu-id="e3af8-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="e3af8-122">Quando il Framework API Web riceve una richiesta HTTP, tenta di trovare una corrispondenza con l'URI rispetto a uno dei modelli di route nella tabella di routing.</span><span class="sxs-lookup"><span data-stu-id="e3af8-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="e3af8-123">Se nessuna route corrisponde, il client riceve un errore 404.</span><span class="sxs-lookup"><span data-stu-id="e3af8-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="e3af8-124">Ad esempio, gli URI seguenti corrispondono alla route predefinita:</span><span class="sxs-lookup"><span data-stu-id="e3af8-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="e3af8-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="e3af8-125">/api/contacts</span></span>
- <span data-ttu-id="e3af8-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="e3af8-126">/api/contacts/1</span></span>
- <span data-ttu-id="e3af8-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="e3af8-127">/api/products/gizmo1</span></span>

<span data-ttu-id="e3af8-128">Tuttavia, l'URI seguente non corrisponde, perché manca il segmento&quot; API &quot;:</span><span class="sxs-lookup"><span data-stu-id="e3af8-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="e3af8-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="e3af8-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="e3af8-130">Il motivo per usare "API" nella route è evitare collisioni con il routing ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3af8-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="e3af8-131">In questo modo, è possibile avere &quot;/contatti&quot; passare a un controller MVC e &quot;/API/Contacts&quot; passare a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="e3af8-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="e3af8-132">Naturalmente, se non si preferisce questa convenzione, è possibile modificare la tabella di route predefinita.</span><span class="sxs-lookup"><span data-stu-id="e3af8-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="e3af8-133">Una volta trovata una route corrispondente, l'API Web seleziona il controller e l'azione:</span><span class="sxs-lookup"><span data-stu-id="e3af8-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="e3af8-134">Per trovare il controller, l'API Web aggiunge &quot;controller&quot; al valore della variabile *{controller}* .</span><span class="sxs-lookup"><span data-stu-id="e3af8-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="e3af8-135">Per trovare l'azione, l'API Web analizza il verbo HTTP e quindi cerca un'azione il cui nome inizia con il nome del verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3af8-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="e3af8-136">Con una richiesta GET, ad esempio, l'API Web Cerca un'azione con il prefisso &quot;Get&quot;, ad esempio &quot;GetContact&quot; o &quot;&quot;GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="e3af8-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="e3af8-137">Questa convenzione si applica solo ai verbi GET, POST, PUT, DELETE, HEAD, OPTIONS e PATCH.</span><span class="sxs-lookup"><span data-stu-id="e3af8-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="e3af8-138">È possibile abilitare altri verbi HTTP usando gli attributi sul controller.</span><span class="sxs-lookup"><span data-stu-id="e3af8-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="e3af8-139">Ne verrà visualizzato un esempio in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="e3af8-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="e3af8-140">Altre variabili segnaposto nel modello di route, ad esempio *{ID},* vengono mappate ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e3af8-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="e3af8-141">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="e3af8-141">Let's look at an example.</span></span> <span data-ttu-id="e3af8-142">Si supponga di definire il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="e3af8-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e3af8-143">Di seguito sono riportate alcune possibili richieste HTTP, insieme all'azione che viene richiamata per ogni:</span><span class="sxs-lookup"><span data-stu-id="e3af8-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="e3af8-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="e3af8-144">HTTP Verb</span></span> | <span data-ttu-id="e3af8-145">Percorso URI</span><span class="sxs-lookup"><span data-stu-id="e3af8-145">URI Path</span></span> | <span data-ttu-id="e3af8-146">Azione</span><span class="sxs-lookup"><span data-stu-id="e3af8-146">Action</span></span> | <span data-ttu-id="e3af8-147">Parametro</span><span class="sxs-lookup"><span data-stu-id="e3af8-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e3af8-148">GET</span><span class="sxs-lookup"><span data-stu-id="e3af8-148">GET</span></span> | <span data-ttu-id="e3af8-149">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="e3af8-149">api/products</span></span> | <span data-ttu-id="e3af8-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="e3af8-150">GetAllProducts</span></span> | <span data-ttu-id="e3af8-151">*nessuno*</span><span class="sxs-lookup"><span data-stu-id="e3af8-151">*(none)*</span></span> |
| <span data-ttu-id="e3af8-152">GET</span><span class="sxs-lookup"><span data-stu-id="e3af8-152">GET</span></span> | <span data-ttu-id="e3af8-153">API/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="e3af8-153">api/products/4</span></span> | <span data-ttu-id="e3af8-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="e3af8-154">GetProductById</span></span> | <span data-ttu-id="e3af8-155">4</span><span class="sxs-lookup"><span data-stu-id="e3af8-155">4</span></span> |
| <span data-ttu-id="e3af8-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="e3af8-156">DELETE</span></span> | <span data-ttu-id="e3af8-157">API/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="e3af8-157">api/products/4</span></span> | <span data-ttu-id="e3af8-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="e3af8-158">DeleteProduct</span></span> | <span data-ttu-id="e3af8-159">4</span><span class="sxs-lookup"><span data-stu-id="e3af8-159">4</span></span> |
| <span data-ttu-id="e3af8-160">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="e3af8-160">POST</span></span> | <span data-ttu-id="e3af8-161">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="e3af8-161">api/products</span></span> | <span data-ttu-id="e3af8-162">*(nessuna corrispondenza)*</span><span class="sxs-lookup"><span data-stu-id="e3af8-162">*(no match)*</span></span> |  |

<span data-ttu-id="e3af8-163">Si noti che il segmento *{ID}* dell'URI, se presente, viene mappato al parametro *ID* dell'azione.</span><span class="sxs-lookup"><span data-stu-id="e3af8-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="e3af8-164">In questo esempio, il controller definisce due metodi GET, uno con un parametro *ID* e uno senza parametri.</span><span class="sxs-lookup"><span data-stu-id="e3af8-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="e3af8-165">Si noti inoltre che la richiesta POST avrà esito negativo perché il controller non definisce un metodo &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3af8-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="e3af8-166">Variazioni di routing</span><span class="sxs-lookup"><span data-stu-id="e3af8-166">Routing Variations</span></span>

<span data-ttu-id="e3af8-167">Nella sezione precedente è stato descritto il meccanismo di routing di base per API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e3af8-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="e3af8-168">In questa sezione vengono descritte alcune varianti.</span><span class="sxs-lookup"><span data-stu-id="e3af8-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="e3af8-169">Verbi HTTP</span><span class="sxs-lookup"><span data-stu-id="e3af8-169">HTTP verbs</span></span>

<span data-ttu-id="e3af8-170">Anziché utilizzare la convenzione di denominazione per i verbi HTTP, è possibile specificare in modo esplicito il verbo HTTP per un'azione decorando il metodo di azione con uno degli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e3af8-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="e3af8-171">Nell'esempio seguente viene eseguito il mapping del metodo `FindProduct` alle richieste GET:</span><span class="sxs-lookup"><span data-stu-id="e3af8-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e3af8-172">Per consentire più verbi HTTP per un'azione o per consentire verbi HTTP diversi da GET, PUT, POST, DELETE, HEAD, OPTIONS e PATCH, usare l'attributo `[AcceptVerbs]`, che accetta un elenco di verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3af8-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="e3af8-173">Routing per nome azione</span><span class="sxs-lookup"><span data-stu-id="e3af8-173">Routing by Action Name</span></span>

<span data-ttu-id="e3af8-174">Con il modello di routing predefinito, l'API Web usa il verbo HTTP per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="e3af8-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="e3af8-175">Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:</span><span class="sxs-lookup"><span data-stu-id="e3af8-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e3af8-176">In questo modello di route, il parametro *{Action}* assegna un nome al metodo di azione nel controller.</span><span class="sxs-lookup"><span data-stu-id="e3af8-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="e3af8-177">Con questo stile di routing, usare gli attributi per specificare i verbi HTTP consentiti.</span><span class="sxs-lookup"><span data-stu-id="e3af8-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="e3af8-178">Si supponga, ad esempio, che il controller abbia il seguente metodo:</span><span class="sxs-lookup"><span data-stu-id="e3af8-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e3af8-179">In questo caso, è possibile eseguire il mapping di una richiesta GET per "API/Products/Details/1" al metodo `Details`.</span><span class="sxs-lookup"><span data-stu-id="e3af8-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="e3af8-180">Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di tipo RPC.</span><span class="sxs-lookup"><span data-stu-id="e3af8-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="e3af8-181">È possibile eseguire l'override del nome dell'azione usando l'attributo `[ActionName]`.</span><span class="sxs-lookup"><span data-stu-id="e3af8-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="e3af8-182">Nell'esempio seguente sono disponibili due azioni che eseguono il mapping a &quot;API/Products/thumbnail/*ID*. Una supporta GET e l'altra supporta POST:</span><span class="sxs-lookup"><span data-stu-id="e3af8-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="e3af8-183">Non azioni</span><span class="sxs-lookup"><span data-stu-id="e3af8-183">Non-Actions</span></span>

<span data-ttu-id="e3af8-184">Per impedire che un metodo venga richiamato come azione, usare l'attributo `[NonAction]`.</span><span class="sxs-lookup"><span data-stu-id="e3af8-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="e3af8-185">Viene segnalato al Framework che il metodo non è un'azione, anche se altrimenti corrisponderebbe alle regole di routing.</span><span class="sxs-lookup"><span data-stu-id="e3af8-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="e3af8-186">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="e3af8-186">Further Reading</span></span>

<span data-ttu-id="e3af8-187">In questo argomento è stata fornita una panoramica di alto livello del routing.</span><span class="sxs-lookup"><span data-stu-id="e3af8-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="e3af8-188">Per informazioni dettagliate, vedere [selezione di routing e azione](routing-and-action-selection.md), che descrive esattamente il modo in cui il Framework corrisponde a un URI di una route, seleziona un controller e quindi seleziona l'azione da richiamare.</span><span class="sxs-lookup"><span data-stu-id="e3af8-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
