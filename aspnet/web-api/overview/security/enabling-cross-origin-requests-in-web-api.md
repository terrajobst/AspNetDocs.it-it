---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitazione di richieste tra le origini in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come supportare la condivisione di risorse tra le origini (CORS) in API Web ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555706"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="81beb-103">Abilitare le richieste tra le origini in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="81beb-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>

<span data-ttu-id="81beb-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81beb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="81beb-105">La sicurezza del browser impedisce a una pagina Web di creare richieste AJAX per un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="81beb-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="81beb-106">Questa restrizione è detta *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="81beb-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="81beb-107">Tuttavia, in alcuni casi potrebbe essere necessario lasciare che altri siti chiamino l'API Web.</span><span class="sxs-lookup"><span data-stu-id="81beb-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="81beb-108">La [condivisione di risorse tra le origini](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server di ridurre i criteri di origine.</span><span class="sxs-lookup"><span data-stu-id="81beb-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="81beb-109">Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="81beb-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="81beb-110">CORS è più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="81beb-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="81beb-111">Questa esercitazione illustra come abilitare CORS nell'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="81beb-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="81beb-112">Software usato nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="81beb-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="81beb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81beb-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="81beb-114">API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="81beb-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="81beb-115">Introduzione</span><span class="sxs-lookup"><span data-stu-id="81beb-115">Introduction</span></span>

<span data-ttu-id="81beb-116">Questa esercitazione illustra il supporto di CORS in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81beb-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="81beb-117">Si inizierà creando due progetti ASP.NET, uno denominato "WebService", che ospita un controller API Web e l'altro denominato "WebClient", che chiama WebService.</span><span class="sxs-lookup"><span data-stu-id="81beb-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="81beb-118">Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient a WebService è una richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="81beb-119">Che cos'è la stessa origine?</span><span class="sxs-lookup"><span data-stu-id="81beb-119">What is "same origin"?</span></span>

<span data-ttu-id="81beb-120">Due URL hanno la stessa origine se hanno schemi, host e porte identici.</span><span class="sxs-lookup"><span data-stu-id="81beb-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="81beb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="81beb-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="81beb-122">Questi due URL hanno la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="81beb-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="81beb-123">Questi URL hanno origini diverse rispetto alle due precedenti:</span><span class="sxs-lookup"><span data-stu-id="81beb-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="81beb-124">`http://example.net`-dominio diverso</span><span class="sxs-lookup"><span data-stu-id="81beb-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="81beb-125">`http://example.com:9000/foo.html`-porta diversa</span><span class="sxs-lookup"><span data-stu-id="81beb-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="81beb-126">`https://example.com/foo.html`-schema diverso</span><span class="sxs-lookup"><span data-stu-id="81beb-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="81beb-127">`http://www.example.com/foo.html`-sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="81beb-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="81beb-128">Internet Explorer non considera la porta durante il confronto delle origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="81beb-129">Creare il progetto WebService</span><span class="sxs-lookup"><span data-stu-id="81beb-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="81beb-130">Questa sezione presuppone che si sappia già come creare progetti API Web.</span><span class="sxs-lookup"><span data-stu-id="81beb-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="81beb-131">In caso contrario, vedere [Introduzione con API Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="81beb-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="81beb-132">Avviare Visual Studio e creare un nuovo progetto di **applicazione Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="81beb-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="81beb-133">Nella finestra di dialogo **nuova applicazione Web ASP.NET** selezionare il modello di progetto **vuoto** .</span><span class="sxs-lookup"><span data-stu-id="81beb-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="81beb-134">In **Aggiungi cartelle e riferimenti principali per**selezionare la casella di controllo **API Web** .</span><span class="sxs-lookup"><span data-stu-id="81beb-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Finestra di dialogo nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="81beb-136">Aggiungere un controller API Web denominato `TestController` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81beb-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="81beb-137">È possibile eseguire l'applicazione localmente o distribuirla in Azure.</span><span class="sxs-lookup"><span data-stu-id="81beb-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="81beb-138">Per le schermate di questa esercitazione, l'app viene distribuita in app Azure app Web del servizio. Per verificare che l'API Web funzioni, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è stata distribuita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="81beb-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="81beb-139">Dovrebbe essere visualizzato il testo della risposta &quot;GET: test&quot;Message.</span><span class="sxs-lookup"><span data-stu-id="81beb-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Web browser visualizzazione del messaggio di prova](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="81beb-141">Creare il progetto WebClient</span><span class="sxs-lookup"><span data-stu-id="81beb-141">Create the WebClient project</span></span>

1. <span data-ttu-id="81beb-142">Creare un altro progetto di **applicazione Web ASP.NET (.NET Framework)** e selezionare il modello di progetto **MVC** .</span><span class="sxs-lookup"><span data-stu-id="81beb-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="81beb-143">Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="81beb-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="81beb-144">Per questa esercitazione non è necessaria l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="81beb-144">You don't need authentication for this tutorial.</span></span>

   ![Modello MVC nella finestra di dialogo nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="81beb-146">In **Esplora soluzioni**aprire il file *views/Home/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="81beb-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="81beb-147">Sostituire il codice in questo file con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="81beb-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="81beb-148">Per la variabile *serviceUrl* , usare l'URI dell'app WebService.</span><span class="sxs-lookup"><span data-stu-id="81beb-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="81beb-149">Eseguire l'app WebClient localmente o pubblicarla in un altro sito Web.</span><span class="sxs-lookup"><span data-stu-id="81beb-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="81beb-150">Quando si fa clic sul pulsante "prova", viene inviata una richiesta AJAX all'app WebService usando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT).</span><span class="sxs-lookup"><span data-stu-id="81beb-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="81beb-151">In questo modo è possibile esaminare diverse richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="81beb-152">Attualmente, l'app WebService non supporta CORS, quindi se si fa clic sul pulsante si otterrà un errore.</span><span class="sxs-lookup"><span data-stu-id="81beb-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Errore ' try it ' nel browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="81beb-154">Se si osserva il traffico HTTP in uno strumento come [Fiddler](https://www.telerik.com/fiddler), si noterà che il browser invia la richiesta GET e la richiesta ha esito positivo, ma la chiamata AJAX restituisce un errore.</span><span class="sxs-lookup"><span data-stu-id="81beb-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="81beb-155">È importante comprendere che i criteri della stessa origine non impediscono al browser di *inviare* la richiesta.</span><span class="sxs-lookup"><span data-stu-id="81beb-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="81beb-156">Ma impedisce all'applicazione di visualizzare la *risposta*.</span><span class="sxs-lookup"><span data-stu-id="81beb-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Debugger Web Fiddler che mostra le richieste Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="81beb-158">Abilitare CORS</span><span class="sxs-lookup"><span data-stu-id="81beb-158">Enable CORS</span></span>

<span data-ttu-id="81beb-159">A questo punto è possibile abilitare CORS nell'app WebService.</span><span class="sxs-lookup"><span data-stu-id="81beb-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="81beb-160">Per prima cosa, aggiungere il pacchetto NuGet CORS.</span><span class="sxs-lookup"><span data-stu-id="81beb-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="81beb-161">In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="81beb-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="81beb-162">Nella finestra console di gestione pacchetti digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="81beb-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="81beb-163">Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie API Web di base.</span><span class="sxs-lookup"><span data-stu-id="81beb-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="81beb-164">Usare il flag di `-Version` per impostare come destinazione una versione specifica.</span><span class="sxs-lookup"><span data-stu-id="81beb-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="81beb-165">Il pacchetto CORS richiede l'API Web 2,0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="81beb-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="81beb-166">Aprire l' *app file\_Start/WebApiConfig. cs*.</span><span class="sxs-lookup"><span data-stu-id="81beb-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="81beb-167">Aggiungere il codice seguente al metodo **WebApiConfig. Register** :</span><span class="sxs-lookup"><span data-stu-id="81beb-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="81beb-168">Successivamente, aggiungere l'attributo **[EnableCors]** alla classe `TestController`:</span><span class="sxs-lookup"><span data-stu-id="81beb-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="81beb-169">Per il parametro *Origins* , usare l'URI in cui è stata distribuita l'applicazione WebClient.</span><span class="sxs-lookup"><span data-stu-id="81beb-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="81beb-170">Ciò consente le richieste tra le origini da WebClient, pur non consentindo comunque tutte le altre richieste tra domini.</span><span class="sxs-lookup"><span data-stu-id="81beb-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="81beb-171">In seguito verranno descritti in modo più dettagliato i parametri per **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="81beb-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="81beb-172">Non includere una barra alla fine dell'URL delle *origini* .</span><span class="sxs-lookup"><span data-stu-id="81beb-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="81beb-173">Ridistribuire l'applicazione WebService aggiornata.</span><span class="sxs-lookup"><span data-stu-id="81beb-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="81beb-174">Non è necessario aggiornare WebClient.</span><span class="sxs-lookup"><span data-stu-id="81beb-174">You don't need to update WebClient.</span></span> <span data-ttu-id="81beb-175">A questo punto la richiesta AJAX da WebClient dovrebbe avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="81beb-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="81beb-176">Sono consentiti tutti i metodi GET, PUT e POST.</span><span class="sxs-lookup"><span data-stu-id="81beb-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Web browser visualizzazione del messaggio di prova riuscito](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="81beb-178">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="81beb-178">How CORS Works</span></span>

<span data-ttu-id="81beb-179">In questa sezione viene descritto cosa accade in una richiesta CORS, a livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="81beb-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="81beb-180">È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare correttamente l'attributo **[EnableCors]** e risolvere i problemi se le cose non funzionano come previsto.</span><span class="sxs-lookup"><span data-stu-id="81beb-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="81beb-181">La specifica CORS introduce diverse nuove intestazioni HTTP che consentono richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="81beb-182">Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini. non è necessario eseguire alcuna operazione speciale nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="81beb-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="81beb-183">Di seguito è riportato un esempio di richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="81beb-184">L'intestazione "Origin" fornisce il dominio del sito che sta effettuando la richiesta.</span><span class="sxs-lookup"><span data-stu-id="81beb-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="81beb-185">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="81beb-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="81beb-186">Il valore di questa intestazione corrisponde all'intestazione Origin o è il valore jolly "\*", che indica che è consentita un'origine.</span><span class="sxs-lookup"><span data-stu-id="81beb-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="81beb-187">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="81beb-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="81beb-188">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="81beb-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="81beb-189">Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="81beb-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="81beb-190">**Richieste preliminari**</span><span class="sxs-lookup"><span data-stu-id="81beb-190">**Preflight Requests**</span></span>

<span data-ttu-id="81beb-191">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare", prima di inviare la richiesta effettiva per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="81beb-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="81beb-192">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="81beb-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="81beb-193">Il metodo di richiesta è GET, HEAD o POST *e*</span><span class="sxs-lookup"><span data-stu-id="81beb-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="81beb-194">L'applicazione non imposta intestazioni di richiesta diverse da Accept, Accept-Language, Content-Language, Content-Type o Last-Event-ID *e*</span><span class="sxs-lookup"><span data-stu-id="81beb-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="81beb-195">L'intestazione Content-Type (se impostata) è una delle seguenti:</span><span class="sxs-lookup"><span data-stu-id="81beb-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="81beb-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="81beb-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="81beb-197">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="81beb-197">multipart/form-data</span></span>
    - <span data-ttu-id="81beb-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="81beb-198">text/plain</span></span>

<span data-ttu-id="81beb-199">La regola sulle intestazioni della richiesta si applica alle intestazioni impostate dall'applicazione chiamando **setRequestHeader** sull'oggetto **XMLHttpRequest** .</span><span class="sxs-lookup"><span data-stu-id="81beb-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="81beb-200">(La specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni che possono essere impostate dal *browser* , ad esempio l'agente utente, l'host o la lunghezza del contenuto.</span><span class="sxs-lookup"><span data-stu-id="81beb-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="81beb-201">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="81beb-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="81beb-202">La richiesta pre-flight usa il metodo delle opzioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="81beb-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="81beb-203">Sono incluse due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="81beb-203">It includes two special headers:</span></span>

- <span data-ttu-id="81beb-204">Access-Control-request-method: il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="81beb-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="81beb-205">Access-Control-Request-Headers: elenco di intestazioni di richiesta impostate dall' *applicazione* per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="81beb-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="81beb-206">Anche in questo caso non sono incluse le intestazioni impostate dal browser.</span><span class="sxs-lookup"><span data-stu-id="81beb-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="81beb-207">Di seguito è riportato un esempio di risposta, presupponendo che il server consenta la richiesta:</span><span class="sxs-lookup"><span data-stu-id="81beb-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="81beb-208">La risposta include un'intestazione Access-Control-Allow-methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Allow-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="81beb-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="81beb-209">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="81beb-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="81beb-210">Gli strumenti comunemente usati per testare gli endpoint con le richieste di opzioni preliminari (ad esempio [Fiddler](https://www.telerik.com/fiddler) e [postazione](https://www.getpostman.com/)) non inviano le intestazioni delle opzioni richieste per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="81beb-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="81beb-211">Verificare che le intestazioni `Access-Control-Request-Method` e `Access-Control-Request-Headers` vengano inviate con la richiesta e che le intestazioni delle opzioni raggiungano l'app tramite IIS.</span><span class="sxs-lookup"><span data-stu-id="81beb-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="81beb-212">Per configurare IIS per consentire a un'app ASP.NET di ricevere e gestire le richieste di opzioni, aggiungere la configurazione seguente al file *Web. config* dell'app nella sezione `<system.webServer><handlers>`:</span><span class="sxs-lookup"><span data-stu-id="81beb-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="81beb-213">La rimozione di `OPTIONSVerbHandler` impedisce a IIS di gestire le richieste di opzioni.</span><span class="sxs-lookup"><span data-stu-id="81beb-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="81beb-214">La sostituzione di `ExtensionlessUrlHandler-Integrated-4.0` consente alle richieste di opzioni di raggiungere l'app perché la registrazione dei moduli predefinita consente solo richieste GET, HEAD, POST e DEBUG con URL senza estensione.</span><span class="sxs-lookup"><span data-stu-id="81beb-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="81beb-215">Regole di ambito per [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="81beb-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="81beb-216">È possibile abilitare CORS per azione, per controller o a livello globale per tutti i controller API Web nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="81beb-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="81beb-217">**Per azione**</span><span class="sxs-lookup"><span data-stu-id="81beb-217">**Per Action**</span></span>

<span data-ttu-id="81beb-218">Per abilitare CORS per una singola azione, impostare l'attributo **[EnableCors]** nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="81beb-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="81beb-219">Nell'esempio seguente viene abilitato CORS solo per il metodo `GetItem`.</span><span class="sxs-lookup"><span data-stu-id="81beb-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="81beb-220">**Per controller**</span><span class="sxs-lookup"><span data-stu-id="81beb-220">**Per Controller**</span></span>

<span data-ttu-id="81beb-221">Se si imposta **[EnableCors]** nella classe controller, si applica a tutte le azioni sul controller.</span><span class="sxs-lookup"><span data-stu-id="81beb-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="81beb-222">Per disabilitare CORS per un'azione, aggiungere l'attributo **[DisableCors]** all'azione.</span><span class="sxs-lookup"><span data-stu-id="81beb-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="81beb-223">Nell'esempio seguente viene abilitato CORS per ogni metodo eccetto `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="81beb-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="81beb-224">**Globalmente**</span><span class="sxs-lookup"><span data-stu-id="81beb-224">**Globally**</span></span>

<span data-ttu-id="81beb-225">Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un'istanza di **EnableCorsAttribute** al metodo **EnableCors** :</span><span class="sxs-lookup"><span data-stu-id="81beb-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="81beb-226">Se si imposta l'attributo in più di un ambito, l'ordine di precedenza è:</span><span class="sxs-lookup"><span data-stu-id="81beb-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="81beb-227">Azione</span><span class="sxs-lookup"><span data-stu-id="81beb-227">Action</span></span>
2. <span data-ttu-id="81beb-228">Controller</span><span class="sxs-lookup"><span data-stu-id="81beb-228">Controller</span></span>
3. <span data-ttu-id="81beb-229">Global</span><span class="sxs-lookup"><span data-stu-id="81beb-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="81beb-230">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="81beb-230">Set the allowed origins</span></span>

<span data-ttu-id="81beb-231">Il parametro *Origins* dell'attributo **[EnableCors]** specifica quali origini sono autorizzate ad accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="81beb-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="81beb-232">Il valore è un elenco delimitato da virgole delle origini consentite.</span><span class="sxs-lookup"><span data-stu-id="81beb-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="81beb-233">È anche possibile usare il valore jolly "\*" per consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="81beb-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="81beb-234">Valutare attentamente prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="81beb-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="81beb-235">Ciò significa che, letteralmente, qualsiasi sito Web può effettuare chiamate AJAX all'API Web.</span><span class="sxs-lookup"><span data-stu-id="81beb-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="81beb-236">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="81beb-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="81beb-237">Il parametro *Methods* dell'attributo **[EnableCors]** specifica quali metodi HTTP sono autorizzati ad accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="81beb-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="81beb-238">Per consentire tutti i metodi, usare il valore jolly "\*".</span><span class="sxs-lookup"><span data-stu-id="81beb-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="81beb-239">Nell'esempio seguente vengono consentite solo le richieste GET e POST.</span><span class="sxs-lookup"><span data-stu-id="81beb-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="81beb-240">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="81beb-240">Set the allowed request headers</span></span>

<span data-ttu-id="81beb-241">Questo articolo descrive in precedenza come una richiesta preliminare può includere un'intestazione Access-Control-Request-Headers, che elenca le intestazioni HTTP impostate dall'applicazione (le cosiddette "intestazioni della richiesta di autore").</span><span class="sxs-lookup"><span data-stu-id="81beb-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="81beb-242">Il parametro *headers* dell'attributo **[EnableCors]** specifica quali intestazioni della richiesta di autore sono consentite.</span><span class="sxs-lookup"><span data-stu-id="81beb-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="81beb-243">Per consentire qualsiasi intestazione, impostare le *intestazioni* su "\*".</span><span class="sxs-lookup"><span data-stu-id="81beb-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="81beb-244">Per includere intestazioni specifiche, impostare le *intestazioni* su un elenco delimitato da virgole delle intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="81beb-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="81beb-245">Tuttavia, i browser non sono completamente coerenti nella modalità di impostazione di Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="81beb-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="81beb-246">Ad esempio, Chrome include attualmente "Origin".</span><span class="sxs-lookup"><span data-stu-id="81beb-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="81beb-247">FireFox non include intestazioni standard, ad esempio "Accept", anche quando l'applicazione le imposta nello script.</span><span class="sxs-lookup"><span data-stu-id="81beb-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="81beb-248">Se si impostano le *intestazioni* su un valore diverso da "\*", è necessario includere almeno "Accept", "Content-Type" e "Origin", più eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="81beb-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="81beb-249">Impostare le intestazioni di risposta consentite</span><span class="sxs-lookup"><span data-stu-id="81beb-249">Set the allowed response headers</span></span>

<span data-ttu-id="81beb-250">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="81beb-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="81beb-251">Le intestazioni di risposta disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="81beb-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="81beb-252">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="81beb-252">Cache-Control</span></span>
- <span data-ttu-id="81beb-253">Content-Language</span><span class="sxs-lookup"><span data-stu-id="81beb-253">Content-Language</span></span>
- <span data-ttu-id="81beb-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="81beb-254">Content-Type</span></span>
- <span data-ttu-id="81beb-255">Scadenza</span><span class="sxs-lookup"><span data-stu-id="81beb-255">Expires</span></span>
- <span data-ttu-id="81beb-256">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="81beb-256">Last-Modified</span></span>
- <span data-ttu-id="81beb-257">Pragma</span><span class="sxs-lookup"><span data-stu-id="81beb-257">Pragma</span></span>

<span data-ttu-id="81beb-258">La specifica CORS chiama queste [semplici intestazioni di risposta](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="81beb-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="81beb-259">Per rendere disponibili altre intestazioni per l'applicazione, impostare il parametro *exposedHeaders* di **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="81beb-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="81beb-260">Nell'esempio seguente, il metodo `Get` del controller imposta un'intestazione personalizzata denominata ' X-custom-header '.</span><span class="sxs-lookup"><span data-stu-id="81beb-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="81beb-261">Per impostazione predefinita, il browser non esporrà questa intestazione in una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="81beb-262">Per rendere disponibile l'intestazione, includere ' X-custom-header ' in *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="81beb-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="81beb-263">Passare le credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="81beb-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="81beb-264">Le credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="81beb-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="81beb-265">Per impostazione predefinita, il browser non invia alcuna credenziale con una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="81beb-266">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="81beb-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="81beb-267">Per inviare le credenziali con una richiesta tra le origini, il client deve impostare **XMLHttpRequest. withCredentials** su true.</span><span class="sxs-lookup"><span data-stu-id="81beb-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="81beb-268">Utilizzando direttamente **XMLHttpRequest** :</span><span class="sxs-lookup"><span data-stu-id="81beb-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="81beb-269">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="81beb-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="81beb-270">Inoltre, il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="81beb-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="81beb-271">Per consentire le credenziali tra le origini nell'API Web, impostare la proprietà **SupportsCredentials** su true nell'attributo **[EnableCors]** :</span><span class="sxs-lookup"><span data-stu-id="81beb-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="81beb-272">Se questa proprietà è true, la risposta HTTP includerà un'intestazione Access-Control-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="81beb-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="81beb-273">Questa intestazione indica al browser che il server consente le credenziali per una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="81beb-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="81beb-274">Se il browser invia le credenziali, ma la risposta non include un'intestazione Access-Control-Allow-Credentials valida, il browser non esporrà la risposta all'applicazione e la richiesta AJAX avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="81beb-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="81beb-275">Prestare attenzione quando si imposta **SupportsCredentials** su true, in quanto significa che un sito Web in un altro dominio può inviare le credenziali di un utente connesso all'API Web per conto dell'utente, senza che l'utente sia in grado di riconoscere.</span><span class="sxs-lookup"><span data-stu-id="81beb-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="81beb-276">La specifica CORS indica anche che l'impostazione delle *origini* per &quot;\*&quot; non è valida se **SupportsCredentials** è true.</span><span class="sxs-lookup"><span data-stu-id="81beb-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="81beb-277">Provider di criteri CORS personalizzati</span><span class="sxs-lookup"><span data-stu-id="81beb-277">Custom CORS policy providers</span></span>

<span data-ttu-id="81beb-278">L'attributo **[EnableCors]** implementa l'interfaccia **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="81beb-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="81beb-279">È possibile fornire un'implementazione personalizzata creando una classe che deriva da **attribute** e implementa **ICorsPolicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="81beb-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsPolicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="81beb-280">A questo punto è possibile applicare l'attributo a qualsiasi posizione da inserire **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="81beb-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="81beb-281">Un provider di criteri CORS personalizzato, ad esempio, potrebbe leggere le impostazioni da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="81beb-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="81beb-282">In alternativa all'uso degli attributi, è possibile registrare un oggetto **ICorsPolicyProviderFactory** che crea oggetti **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="81beb-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="81beb-283">Per impostare **ICorsPolicyProviderFactory**, chiamare il metodo di estensione **SetCorsPolicyProviderFactory** all'avvio, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="81beb-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="81beb-284">Supporto browser</span><span class="sxs-lookup"><span data-stu-id="81beb-284">Browser support</span></span>

<span data-ttu-id="81beb-285">Il pacchetto CORS dell'API Web è una tecnologia lato server.</span><span class="sxs-lookup"><span data-stu-id="81beb-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="81beb-286">Il browser dell'utente deve anche supportare CORS.</span><span class="sxs-lookup"><span data-stu-id="81beb-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="81beb-287">Fortunatamente, le versioni correnti di tutti i principali browser includono il [supporto per CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="81beb-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
