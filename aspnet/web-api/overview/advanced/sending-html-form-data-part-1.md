---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Invio di dati del modulo HTML in API Web ASP.NET: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: Questo articolo illustra come inserire i dati di form-urlencoded in un controller API Web con ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557603"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="5b55d-103">Invio di dati del modulo HTML in API Web ASP.NET: dati form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="5b55d-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="5b55d-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5b55d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="5b55d-105">Parte 1: form-urlencoded data</span><span class="sxs-lookup"><span data-stu-id="5b55d-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="5b55d-106">Questo articolo illustra come inserire i dati di form-urlencoded in un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="5b55d-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="5b55d-107">Panoramica dei moduli HTML</span><span class="sxs-lookup"><span data-stu-id="5b55d-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="5b55d-108">Invio di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="5b55d-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="5b55d-109">Invio di dati del modulo tramite AJAX</span><span class="sxs-lookup"><span data-stu-id="5b55d-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="5b55d-110">Invio di tipi semplici</span><span class="sxs-lookup"><span data-stu-id="5b55d-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="5b55d-111">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="5b55d-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="5b55d-112">Panoramica dei moduli HTML</span><span class="sxs-lookup"><span data-stu-id="5b55d-112">Overview of HTML Forms</span></span>

<span data-ttu-id="5b55d-113">I form HTML usano GET o POST per inviare dati al server.</span><span class="sxs-lookup"><span data-stu-id="5b55d-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="5b55d-114">L'attributo **Method** dell'elemento **form** fornisce il metodo http:</span><span class="sxs-lookup"><span data-stu-id="5b55d-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="5b55d-115">Il metodo predefinito è GET.</span><span class="sxs-lookup"><span data-stu-id="5b55d-115">The default method is GET.</span></span> <span data-ttu-id="5b55d-116">Se il modulo usa GET, i dati del modulo vengono codificati nell'URI come stringa di query.</span><span class="sxs-lookup"><span data-stu-id="5b55d-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="5b55d-117">Se il modulo usa POST, i dati del modulo vengono inseriti nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="5b55d-118">Per i dati pubblicati, **l'attributo di un attributo** consente di specificare il formato del corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="5b55d-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="5b55d-119">enctype</span><span class="sxs-lookup"><span data-stu-id="5b55d-119">enctype</span></span> | <span data-ttu-id="5b55d-120">Description</span><span class="sxs-lookup"><span data-stu-id="5b55d-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5b55d-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="5b55d-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="5b55d-122">I dati del modulo vengono codificati come coppie nome/valore, in modo analogo a una stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="5b55d-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="5b55d-123">Questo è il formato predefinito per POST.</span><span class="sxs-lookup"><span data-stu-id="5b55d-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="5b55d-124">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="5b55d-124">multipart/form-data</span></span> | <span data-ttu-id="5b55d-125">I dati del modulo vengono codificati come messaggio MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="5b55d-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="5b55d-126">Usare questo formato se si sta caricando un file nel server.</span><span class="sxs-lookup"><span data-stu-id="5b55d-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="5b55d-127">La parte 1 di questo articolo esamina il formato x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="5b55d-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="5b55d-128">Nella [parte 2](sending-html-form-data-part-2.md) viene descritto MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="5b55d-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="5b55d-129">Invio di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="5b55d-129">Sending Complex Types</span></span>

<span data-ttu-id="5b55d-130">In genere, si invierà un tipo complesso, composto da valori ricavati da diversi controlli form.</span><span class="sxs-lookup"><span data-stu-id="5b55d-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="5b55d-131">Si consideri il modello seguente che rappresenta un aggiornamento dello stato:</span><span class="sxs-lookup"><span data-stu-id="5b55d-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="5b55d-132">Di seguito è riportato un controller API Web che accetta un oggetto `Update` tramite POST.</span><span class="sxs-lookup"><span data-stu-id="5b55d-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="5b55d-133">Questo controller usa il [routing basato sulle azioni](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), quindi il modello di route è &quot;API/{controller}/{action}/{id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b55d-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="5b55d-134">Il client pubblicherà i dati &quot;&quot;/API/Updates/Complex.</span><span class="sxs-lookup"><span data-stu-id="5b55d-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="5b55d-135">Scriviamo ora un modulo HTML per consentire agli utenti di inviare un aggiornamento dello stato.</span><span class="sxs-lookup"><span data-stu-id="5b55d-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="5b55d-136">Si noti che l'attributo **Action** nel form è l'URI dell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="5b55d-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="5b55d-137">Ecco il modulo con alcuni valori immessi in:</span><span class="sxs-lookup"><span data-stu-id="5b55d-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="5b55d-138">Quando l'utente fa clic su Invia, il browser invia una richiesta HTTP simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="5b55d-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="5b55d-139">Si noti che il corpo della richiesta contiene i dati del modulo, formattati come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="5b55d-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="5b55d-140">L'API Web converte automaticamente le coppie nome/valore in un'istanza della classe `Update`.</span><span class="sxs-lookup"><span data-stu-id="5b55d-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="5b55d-141">Invio di dati del modulo tramite AJAX</span><span class="sxs-lookup"><span data-stu-id="5b55d-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="5b55d-142">Quando un utente invia un modulo, il browser si sposta dalla pagina corrente ed esegue il rendering del corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="5b55d-143">Questo è accettabile quando la risposta è una pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="5b55d-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="5b55d-144">Con un'API Web, tuttavia, il corpo della risposta è in genere vuoto o contiene dati strutturati, ad esempio JSON.</span><span class="sxs-lookup"><span data-stu-id="5b55d-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="5b55d-145">In tal caso, è più sensato inviare i dati del modulo usando una richiesta AJAX, in modo che la pagina possa elaborare la risposta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="5b55d-146">Il codice seguente illustra come inviare i dati del form tramite jQuery.</span><span class="sxs-lookup"><span data-stu-id="5b55d-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="5b55d-147">La funzione di **invio** jQuery sostituisce l'azione form con una nuova funzione.</span><span class="sxs-lookup"><span data-stu-id="5b55d-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="5b55d-148">Questa operazione sostituisce il comportamento predefinito del pulsante Submit.</span><span class="sxs-lookup"><span data-stu-id="5b55d-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="5b55d-149">La funzione **Serialize** serializza i dati del modulo in coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="5b55d-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="5b55d-150">Per inviare i dati del modulo al server, chiamare `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="5b55d-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="5b55d-151">Al termine della richiesta, il gestore `.success()` o `.error()` Visualizza un messaggio appropriato per l'utente.</span><span class="sxs-lookup"><span data-stu-id="5b55d-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="5b55d-152">Invio di tipi semplici</span><span class="sxs-lookup"><span data-stu-id="5b55d-152">Sending Simple Types</span></span>

<span data-ttu-id="5b55d-153">Nelle sezioni precedenti è stato inviato un tipo complesso, che API Web è stato deserializzato in un'istanza di una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="5b55d-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="5b55d-154">È anche possibile inviare tipi semplici, ad esempio una stringa.</span><span class="sxs-lookup"><span data-stu-id="5b55d-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="5b55d-155">Prima di inviare un tipo semplice, è consigliabile eseguire il wrapping del valore in un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="5b55d-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="5b55d-156">Ciò offre i vantaggi della convalida dei modelli sul lato server e semplifica l'estensione del modello, se necessario.</span><span class="sxs-lookup"><span data-stu-id="5b55d-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="5b55d-157">I passaggi di base per inviare un tipo semplice sono gli stessi, ma vi sono due differenze minime.</span><span class="sxs-lookup"><span data-stu-id="5b55d-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="5b55d-158">Per prima cosa, nel controller è necessario decorare il nome del parametro con l'attributo **FromBody** .</span><span class="sxs-lookup"><span data-stu-id="5b55d-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="5b55d-159">Per impostazione predefinita, l'API Web tenta di ottenere tipi semplici dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="5b55d-160">L'attributo **FromBody** indica all'API Web di leggere il valore dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="5b55d-161">L'API Web legge il corpo della risposta al massimo una volta, quindi solo un parametro di un'azione può provenire dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b55d-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="5b55d-162">Se è necessario ottenere più valori dal corpo della richiesta, definire un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="5b55d-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="5b55d-163">In secondo luogo, il client deve inviare il valore con il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="5b55d-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="5b55d-164">In particolare, la parte relativa al nome della coppia nome/valore deve essere vuota per un tipo semplice.</span><span class="sxs-lookup"><span data-stu-id="5b55d-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="5b55d-165">Non tutti i browser supportano questa operazione per i form HTML, ma si crea questo formato nello script come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5b55d-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="5b55d-166">Di seguito è riportato un esempio di formato:</span><span class="sxs-lookup"><span data-stu-id="5b55d-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="5b55d-167">Di seguito è riportato lo script per l'invio del valore del modulo.</span><span class="sxs-lookup"><span data-stu-id="5b55d-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="5b55d-168">L'unica differenza rispetto allo script precedente è l'argomento passato alla funzione **post** .</span><span class="sxs-lookup"><span data-stu-id="5b55d-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="5b55d-169">È possibile usare lo stesso approccio per inviare una matrice di tipi semplici:</span><span class="sxs-lookup"><span data-stu-id="5b55d-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="5b55d-170">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5b55d-170">Additional Resources</span></span>

[<span data-ttu-id="5b55d-171">Parte 2: caricamento di file e MIME multipart</span><span class="sxs-lookup"><span data-stu-id="5b55d-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
