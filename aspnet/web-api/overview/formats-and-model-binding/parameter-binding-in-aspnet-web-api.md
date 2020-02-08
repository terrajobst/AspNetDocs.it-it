---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associazione di parametri in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come l'API Web associa i parametri e come personalizzare il processo di associazione in ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074904"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="4cacd-103">Associazione di parametri in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cacd-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="4cacd-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4cacd-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="4cacd-105">Questo articolo descrive il modo in cui l'API Web associa i parametri e come è possibile personalizzare il processo di associazione.</span><span class="sxs-lookup"><span data-stu-id="4cacd-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="4cacd-106">Quando l'API Web chiama un metodo su un controller, deve impostare i valori per i parametri, un processo denominato *Binding*.</span><span class="sxs-lookup"><span data-stu-id="4cacd-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="4cacd-107">Per impostazione predefinita, l'API Web usa le regole seguenti per associare i parametri:</span><span class="sxs-lookup"><span data-stu-id="4cacd-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="4cacd-108">Se il parametro è di tipo "Simple", l'API Web tenta di ottenere il valore dall'URI.</span><span class="sxs-lookup"><span data-stu-id="4cacd-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="4cacd-109">I tipi semplici includono i [tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .NET (**int**, **bool**, **Double**e così via), più **TimeSpan**, **DateTime**, **GUID**, **Decimal**e **String**, *più* qualsiasi tipo con un convertitore di tipi che può convertire da una stringa.</span><span class="sxs-lookup"><span data-stu-id="4cacd-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="4cacd-110">Ulteriori informazioni sui convertitori di tipi in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="4cacd-110">(More about type converters later.)</span></span>
- <span data-ttu-id="4cacd-111">Per i tipi complessi, l'API Web tenta di leggere il valore dal corpo del messaggio, usando un [formattatore di media type](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="4cacd-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="4cacd-112">Ad esempio, di seguito è riportato un tipico metodo del controller API Web:</span><span class="sxs-lookup"><span data-stu-id="4cacd-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4cacd-113">Il parametro *ID* è un &quot;tipo di&quot; semplice, quindi l'API Web tenta di ottenere il valore dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="4cacd-114">Il parametro *Item* è un tipo complesso, quindi l'API Web usa un formattatore di tipo supporto per leggere il valore dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="4cacd-115">Per ottenere un valore dall'URI, l'API Web Cerca nei dati della route e nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="4cacd-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="4cacd-116">I dati della route vengono popolati quando il sistema di routing analizza l'URI e lo associa a una route.</span><span class="sxs-lookup"><span data-stu-id="4cacd-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="4cacd-117">Per ulteriori informazioni, vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4cacd-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="4cacd-118">Nella parte restante di questo articolo verrà illustrato come personalizzare il processo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="4cacd-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="4cacd-119">Per i tipi complessi, tuttavia, è consigliabile usare i formattatori del tipo di supporto quando possibile.</span><span class="sxs-lookup"><span data-stu-id="4cacd-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="4cacd-120">Un principio chiave di HTTP è che le risorse vengono inviate nel corpo del messaggio, usando la negoziazione del contenuto per specificare la rappresentazione della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4cacd-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="4cacd-121">I formattatori del tipo di supporto sono stati progettati esattamente per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="4cacd-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="4cacd-122">Uso di [FromUri]</span><span class="sxs-lookup"><span data-stu-id="4cacd-122">Using [FromUri]</span></span>

<span data-ttu-id="4cacd-123">Per forzare l'API Web a leggere un tipo complesso dall'URI, aggiungere l'attributo **[FromUri]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="4cacd-124">Nell'esempio seguente viene definito un tipo di `GeoPoint`, insieme a un metodo controller che ottiene l'`GeoPoint` dall'URI.</span><span class="sxs-lookup"><span data-stu-id="4cacd-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4cacd-125">Il client può inserire i valori di latitudine e longitudine nella stringa di query e l'API Web li userà per costruire un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="4cacd-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="4cacd-126">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="4cacd-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="4cacd-127">Uso di [FromBody]</span><span class="sxs-lookup"><span data-stu-id="4cacd-127">Using [FromBody]</span></span>

<span data-ttu-id="4cacd-128">Per forzare la lettura di un tipo semplice dal corpo della richiesta da parte dell'API Web, aggiungere l'attributo **[FromBody]** al parametro:</span><span class="sxs-lookup"><span data-stu-id="4cacd-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="4cacd-129">In questo esempio, l'API Web userà un formattatore di Media Type per leggere il valore di *Name* dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="4cacd-130">Di seguito è riportato un esempio di richiesta client.</span><span class="sxs-lookup"><span data-stu-id="4cacd-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="4cacd-131">Quando un parametro è [FromBody], l'API Web usa l'intestazione Content-Type per selezionare un formattatore.</span><span class="sxs-lookup"><span data-stu-id="4cacd-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="4cacd-132">In questo esempio, il tipo di contenuto è &quot;&quot; Application/JSON e il corpo della richiesta è una stringa JSON non elaborata, non un oggetto JSON.</span><span class="sxs-lookup"><span data-stu-id="4cacd-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="4cacd-133">Al massimo un parametro è autorizzato a leggere dal corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="4cacd-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="4cacd-134">Quindi, questa operazione non funzionerà:</span><span class="sxs-lookup"><span data-stu-id="4cacd-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="4cacd-135">Il motivo di questa regola è che il corpo della richiesta può essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="4cacd-136">Convertitori di tipi</span><span class="sxs-lookup"><span data-stu-id="4cacd-136">Type Converters</span></span>

<span data-ttu-id="4cacd-137">È possibile fare in modo che un'API Web consideri una classe come un tipo semplice (in modo che l'API Web tenti di associarla dall'URI) creando un **TypeConverter** e fornendo una conversione di stringa.</span><span class="sxs-lookup"><span data-stu-id="4cacd-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="4cacd-138">Nel codice riportato di seguito viene illustrata una classe `GeoPoint` che rappresenta un punto geografico, oltre a un oggetto **TypeConverter** che converte le stringhe in istanze di `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="4cacd-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="4cacd-139">La classe `GeoPoint` è decorata con un attributo **[TypeConverter]** per specificare il convertitore di tipi.</span><span class="sxs-lookup"><span data-stu-id="4cacd-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="4cacd-140">Questo esempio è stato ispirato dal post di Blog di Mike Stall [come associazione a oggetti personalizzati nelle firme delle azioni in MVC/WebApi](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).</span><span class="sxs-lookup"><span data-stu-id="4cacd-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="4cacd-141">Ora l'API Web considererà `GeoPoint` come un tipo semplice, ovvero tenterà di associare `GeoPoint` parametri dall'URI.</span><span class="sxs-lookup"><span data-stu-id="4cacd-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="4cacd-142">Non è necessario includere **[FromUri]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="4cacd-143">Il client può richiamare il metodo con un URI analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="4cacd-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="4cacd-144">Raccoglitori di modelli</span><span class="sxs-lookup"><span data-stu-id="4cacd-144">Model Binders</span></span>

<span data-ttu-id="4cacd-145">Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare uno strumento di associazione di modelli personalizzato.</span><span class="sxs-lookup"><span data-stu-id="4cacd-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="4cacd-146">Con uno strumento di associazione di modelli, è possibile accedere a elementi quali la richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="4cacd-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="4cacd-147">Per creare uno strumento di associazione di modelli, implementare l'interfaccia **IModelBinder** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="4cacd-148">Questa interfaccia definisce un singolo metodo, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="4cacd-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="4cacd-149">Di seguito è riportato uno strumento di associazione di modelli per oggetti `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="4cacd-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="4cacd-150">Uno strumento di associazione di modelli ottiene valori di input non elaborati da un *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="4cacd-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="4cacd-151">Questa progettazione separa due funzioni distinte:</span><span class="sxs-lookup"><span data-stu-id="4cacd-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="4cacd-152">Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="4cacd-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="4cacd-153">Lo strumento di associazione di modelli utilizza questo dizionario per popolare il modello.</span><span class="sxs-lookup"><span data-stu-id="4cacd-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="4cacd-154">Il provider di valori predefinito nell'API Web ottiene i valori dai dati della route e dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="4cacd-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="4cacd-155">Se, ad esempio, l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori crea le coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cacd-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="4cacd-156">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="4cacd-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="4cacd-157">località = &quot;48.122&quot;</span><span class="sxs-lookup"><span data-stu-id="4cacd-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="4cacd-158">Si presuppone che il modello di route predefinito, ovvero &quot;API/{controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="4cacd-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="4cacd-159">Il nome del parametro da associare è archiviato nella proprietà **ModelBindingContext. ModelName** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="4cacd-160">Lo strumento di associazione di modelli cerca una chiave con questo valore nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="4cacd-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="4cacd-161">Se il valore esiste e può essere convertito in un `GeoPoint`, lo strumento di associazione di modelli assegna il valore associato alla proprietà **ModelBindingContext. Model** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="4cacd-162">Si noti che lo strumento di associazione di modelli non è limitato a una semplice conversione di tipi.</span><span class="sxs-lookup"><span data-stu-id="4cacd-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="4cacd-163">In questo esempio, lo strumento di associazione di modelli esegue prima una ricerca in una tabella di posizioni note e, in caso di esito negativo, usa la conversione del tipo.</span><span class="sxs-lookup"><span data-stu-id="4cacd-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="4cacd-164">**Impostazione dello strumento di associazione di modelli**</span><span class="sxs-lookup"><span data-stu-id="4cacd-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="4cacd-165">Sono disponibili diversi modi per impostare uno strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="4cacd-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="4cacd-166">In primo luogo, è possibile aggiungere un attributo **[ModelBinder]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="4cacd-167">È anche possibile aggiungere un attributo **[ModelBinder]** al tipo.</span><span class="sxs-lookup"><span data-stu-id="4cacd-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="4cacd-168">L'API Web utilizzerà lo strumento di associazione di modelli specificato per tutti i parametri di quel tipo.</span><span class="sxs-lookup"><span data-stu-id="4cacd-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="4cacd-169">Infine, è possibile aggiungere un provider dello strumento di associazione di modelli a **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="4cacd-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="4cacd-170">Un provider dello strumento di associazione di modelli è semplicemente una classe factory che crea uno strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="4cacd-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="4cacd-171">È possibile creare un provider derivando dalla classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) .</span><span class="sxs-lookup"><span data-stu-id="4cacd-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="4cacd-172">Tuttavia, se lo strumento di associazione di modelli gestisce un solo tipo, è più facile usare il **SimpleModelBinderProvider**incorporato, progettato a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="4cacd-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="4cacd-173">A tal fine, osservare il codice indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4cacd-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="4cacd-174">Con un provider di associazione di modelli è comunque necessario aggiungere l'attributo **[ModelBinder]** al parametro per indicare all'API Web che deve usare uno strumento di associazione di modelli e non un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="4cacd-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="4cacd-175">Non è ora necessario specificare il tipo di strumento di associazione di modelli nell'attributo:</span><span class="sxs-lookup"><span data-stu-id="4cacd-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="4cacd-176">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="4cacd-176">Value Providers</span></span>

<span data-ttu-id="4cacd-177">Ho già indicato che uno strumento di associazione di modelli ottiene i valori da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="4cacd-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="4cacd-178">Per scrivere un provider di valori personalizzato, implementare l'interfaccia **IValueProvider** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="4cacd-179">Di seguito è riportato un esempio che estrae i valori dai cookie nella richiesta:</span><span class="sxs-lookup"><span data-stu-id="4cacd-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="4cacd-180">È anche necessario creare una factory del provider di valori derivando dalla classe **ValueProviderFactory** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="4cacd-181">Aggiungere la factory del provider di valori a **HttpConfiguration** come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4cacd-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="4cacd-182">L'API Web compone tutti i provider di valori, pertanto quando uno strumento di associazione di modelli chiama **ValueProvider. GetValue**, lo strumento di associazione di modelli riceve il valore dal primo provider di valori che è in grado di produrre.</span><span class="sxs-lookup"><span data-stu-id="4cacd-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="4cacd-183">In alternativa, è possibile impostare la factory del provider di valori a livello di parametro usando l'attributo **ValueProvider** , come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4cacd-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="4cacd-184">Ciò indica all'API Web di usare l'associazione di modelli con la factory del provider di valori specificata e non usare gli altri provider di valori registrati.</span><span class="sxs-lookup"><span data-stu-id="4cacd-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="4cacd-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="4cacd-185">HttpParameterBinding</span></span>

<span data-ttu-id="4cacd-186">Gli associazione di modelli sono un'istanza specifica di un meccanismo più generale.</span><span class="sxs-lookup"><span data-stu-id="4cacd-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="4cacd-187">Se si osserva l'attributo **[ModelBinder]** , si noterà che deriva dalla classe astratta **ParameterBindingAttribute** .</span><span class="sxs-lookup"><span data-stu-id="4cacd-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="4cacd-188">Questa classe definisce un solo metodo, **GetBinding**, che restituisce un oggetto **HttpParameterBinding** :</span><span class="sxs-lookup"><span data-stu-id="4cacd-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="4cacd-189">Un **HttpParameterBinding** è responsabile dell'associazione di un parametro a un valore.</span><span class="sxs-lookup"><span data-stu-id="4cacd-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="4cacd-190">Nel caso di **[ModelBinder]** , l'attributo restituisce un'implementazione **HttpParameterBinding** che usa un **IModelBinder** per eseguire l'associazione effettiva.</span><span class="sxs-lookup"><span data-stu-id="4cacd-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="4cacd-191">È anche possibile implementare il proprio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="4cacd-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="4cacd-192">Si supponga, ad esempio, di voler ottenere gli ETag dalle intestazioni `if-match` e `if-none-match` nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="4cacd-193">Si inizierà definendo una classe per rappresentare gli ETag.</span><span class="sxs-lookup"><span data-stu-id="4cacd-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="4cacd-194">Si definirà anche un'enumerazione per indicare se ottenere l'ETag dall'intestazione `if-match` o dall'intestazione `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="4cacd-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="4cacd-195">Ecco un **HttpParameterBinding** che ottiene l'ETag dall'intestazione desiderata e lo associa a un parametro di tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="4cacd-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="4cacd-196">Il metodo **ExecuteBindingAsync** esegue l'associazione.</span><span class="sxs-lookup"><span data-stu-id="4cacd-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="4cacd-197">All'interno di questo metodo, aggiungere il valore del parametro associato al dizionario **ActionArgument** in **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="4cacd-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="4cacd-198">Se il metodo **ExecuteBindingAsync** legge il corpo del messaggio di richiesta, eseguire l'override della proprietà **WillReadBody** per restituire true.</span><span class="sxs-lookup"><span data-stu-id="4cacd-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="4cacd-199">Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto una sola volta, quindi l'API Web impone una regola che al massimo un'associazione può leggere il corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="4cacd-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="4cacd-200">Per applicare un **HttpParameterBinding**personalizzato, è possibile definire un attributo che deriva da **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="4cacd-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="4cacd-201">Per `ETagParameterBinding`, verranno definiti due attributi, uno per `if-match` le intestazioni e uno per `if-none-match` le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="4cacd-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="4cacd-202">Entrambi derivano da una classe base astratta.</span><span class="sxs-lookup"><span data-stu-id="4cacd-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="4cacd-203">Ecco un metodo del controller che usa l'attributo `[IfNoneMatch]`.</span><span class="sxs-lookup"><span data-stu-id="4cacd-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="4cacd-204">Oltre a **ParameterBindingAttribute**, è disponibile un altro hook per l'aggiunta di un **HttpParameterBinding**personalizzato.</span><span class="sxs-lookup"><span data-stu-id="4cacd-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="4cacd-205">Nell'oggetto **HttpConfiguration** la proprietà **ParameterBindingRules** è una raccolta di funzioni anonime di tipo (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="4cacd-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="4cacd-206">Ad esempio, è possibile aggiungere una regola che qualsiasi parametro ETag in un metodo GET USA `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="4cacd-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="4cacd-207">La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="4cacd-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="4cacd-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="4cacd-208">IActionValueBinder</span></span>

<span data-ttu-id="4cacd-209">L'intero processo di associazione di parametri è controllato da un servizio innestabile, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="4cacd-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="4cacd-210">L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cacd-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="4cacd-211">Cercare un **ParameterBindingAttribute** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="4cacd-212">Sono inclusi gli attributi **[FromBody]** , **[FromUri]** e **[ModelBinder]** oppure gli attributi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4cacd-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="4cacd-213">In caso contrario, esaminare **HttpConfiguration. ParameterBindingRules** per una funzione che restituisce un **HttpParameterBinding**non null.</span><span class="sxs-lookup"><span data-stu-id="4cacd-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="4cacd-214">In caso contrario, usare le regole predefinite descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4cacd-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="4cacd-215">Se il tipo di parametro è "Simple" o ha un convertitore di tipi, eseguire l'associazione dall'URI.</span><span class="sxs-lookup"><span data-stu-id="4cacd-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="4cacd-216">Equivale a inserire l'attributo **[FromUri]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="4cacd-217">In caso contrario, provare a leggere il parametro dal corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="4cacd-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="4cacd-218">Equivale a inserire **[FromBody]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="4cacd-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="4cacd-219">Se lo si desidera, è possibile sostituire l'intero servizio **IActionValueBinder** con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="4cacd-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cacd-220">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cacd-220">Additional Resources</span></span>

[<span data-ttu-id="4cacd-221">Esempio di associazione di parametri personalizzati</span><span class="sxs-lookup"><span data-stu-id="4cacd-221">Custom Parameter Binding Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

<span data-ttu-id="4cacd-222">Mike Stall ha scritto una serie di post di Blog sull'associazione dei parametri dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="4cacd-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="4cacd-223">Come l'API Web esegue l'associazione di parametri</span><span class="sxs-lookup"><span data-stu-id="4cacd-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="4cacd-224">Associazione di parametri dello stile MVC per l'API Web</span><span class="sxs-lookup"><span data-stu-id="4cacd-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="4cacd-225">Come eseguire l'associazione a oggetti personalizzati nelle firme delle azioni in MVC/API Web</span><span class="sxs-lookup"><span data-stu-id="4cacd-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="4cacd-226">Come creare un provider di valori personalizzati nell'API Web</span><span class="sxs-lookup"><span data-stu-id="4cacd-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="4cacd-227">Associazione di parametri dell'API Web dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="4cacd-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
