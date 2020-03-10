---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Panoramica del routing MVC ASP.NETC#() | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther Mostra come il Framework di MVC ASP.NET esegue il mapping delle richieste del browser alle azioni del controller.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544142"
---
# <a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="2a052-103">Panoramica del routing di ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="2a052-103">ASP.NET MVC Routing Overview (C#)</span></span>

<span data-ttu-id="2a052-104">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2a052-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2a052-105">In questa esercitazione, Stephen Walther Mostra come il Framework di MVC ASP.NET esegue il mapping delle richieste del browser alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="2a052-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="2a052-106">In questa esercitazione viene introdotta una funzionalità importante di ogni applicazione ASP.NET MVC denominata *ASP.NET routing*.</span><span class="sxs-lookup"><span data-stu-id="2a052-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="2a052-107">Il modulo di routing ASP.NET è responsabile del mapping delle richieste del browser in ingresso a specifiche azioni del controller MVC.</span><span class="sxs-lookup"><span data-stu-id="2a052-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="2a052-108">Al termine di questa esercitazione, si apprenderà come la tabella di route standard esegue il mapping delle richieste alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="2a052-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="2a052-109">Uso della tabella di route predefinita</span><span class="sxs-lookup"><span data-stu-id="2a052-109">Using the Default Route Table</span></span>

<span data-ttu-id="2a052-110">Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurata per l'uso del routing ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a052-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="2a052-111">Il routing ASP.NET viene configurato in due posizioni.</span><span class="sxs-lookup"><span data-stu-id="2a052-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="2a052-112">Per prima cosa, il routing ASP.NET è abilitato nel file di configurazione Web dell'applicazione (file Web. config).</span><span class="sxs-lookup"><span data-stu-id="2a052-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="2a052-113">Nel file di configurazione sono presenti quattro sezioni rilevanti per il routing: la sezione System. Web. httpModules, la sezione System. Web. httpHandlers, la sezione System. webserver. Modules e la sezione System. webserver. handler.</span><span class="sxs-lookup"><span data-stu-id="2a052-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="2a052-114">Prestare attenzione a non eliminare queste sezioni perché senza queste sezioni il routing non funzionerà più.</span><span class="sxs-lookup"><span data-stu-id="2a052-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="2a052-115">Secondo e, più importante, viene creata una tabella di route nel file Global. asax dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a052-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="2a052-116">Il file Global. asax è un file speciale che contiene i gestori eventi per gli eventi del ciclo di vita dell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a052-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="2a052-117">La tabella di route viene creata durante l'evento di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a052-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="2a052-118">Il file nel listato 1 contiene il file Global. asax predefinito per un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a052-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="2a052-119">**Listato 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="2a052-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="2a052-120">Quando un'applicazione MVC viene avviata per la prima volta, viene chiamato il metodo Start () dell'applicazione\_.</span><span class="sxs-lookup"><span data-stu-id="2a052-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="2a052-121">Questo metodo, a sua volta, chiama il metodo RegisterRoutes ().</span><span class="sxs-lookup"><span data-stu-id="2a052-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="2a052-122">Il metodo RegisterRoutes () crea la tabella di route.</span><span class="sxs-lookup"><span data-stu-id="2a052-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="2a052-123">La tabella di route predefinita contiene una singola route (denominata default).</span><span class="sxs-lookup"><span data-stu-id="2a052-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="2a052-124">La route predefinita esegue il mapping del primo segmento di un URL a un nome di controller, il secondo segmento di un URL di un'azione del controller e il terzo segmento a un parametro denominato **ID**.</span><span class="sxs-lookup"><span data-stu-id="2a052-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="2a052-125">Si supponga di immettere l'URL seguente nella barra degli indirizzi del Web browser:</span><span class="sxs-lookup"><span data-stu-id="2a052-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="2a052-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="2a052-126">/Home/Index/3</span></span>

<span data-ttu-id="2a052-127">La route predefinita esegue il mapping di questo URL ai parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="2a052-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="2a052-128">controller = Home</span><span class="sxs-lookup"><span data-stu-id="2a052-128">controller = Home</span></span>

- <span data-ttu-id="2a052-129">Action = indice</span><span class="sxs-lookup"><span data-stu-id="2a052-129">action = Index</span></span>

- <span data-ttu-id="2a052-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="2a052-130">id = 3</span></span>

<span data-ttu-id="2a052-131">Quando si richiede l'URL/Home/Index/3, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2a052-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="2a052-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="2a052-132">HomeController.Index(3)</span></span>

<span data-ttu-id="2a052-133">La route predefinita include i valori predefiniti per tutti e tre i parametri.</span><span class="sxs-lookup"><span data-stu-id="2a052-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="2a052-134">Se non si fornisce un controller, il parametro controller viene impostato sul valore **Home**.</span><span class="sxs-lookup"><span data-stu-id="2a052-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="2a052-135">Se non si specifica un'azione, il valore predefinito del parametro dell'azione è l' **Indice**del valore.</span><span class="sxs-lookup"><span data-stu-id="2a052-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="2a052-136">Infine, se non si specifica un ID, il parametro ID viene impostato come valore predefinito su una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="2a052-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="2a052-137">Esaminiamo alcuni esempi di come la route predefinita esegue il mapping degli URL alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="2a052-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="2a052-138">Si supponga di immettere l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="2a052-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="2a052-139">/Home</span><span class="sxs-lookup"><span data-stu-id="2a052-139">/Home</span></span>

<span data-ttu-id="2a052-140">A causa delle impostazioni predefinite predefinite dei parametri di route, l'immissione di questo URL provocherà la chiamata del metodo index () della classe HomeController nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="2a052-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="2a052-141">**Listato 2-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="2a052-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="2a052-142">Nel listato 2 la classe HomeController include un metodo denominato index () che accetta un singolo parametro denominato ID. L'URL/Home causa la chiamata al metodo index () con una stringa vuota come valore del parametro ID.</span><span class="sxs-lookup"><span data-stu-id="2a052-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="2a052-143">A causa del modo in cui il framework MVC richiama le azioni del controller, l'URL/Home corrisponde anche al metodo index () della classe HomeController nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="2a052-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="2a052-144">**Listato 3-HomeController.cs (azione index senza parametri)**</span><span class="sxs-lookup"><span data-stu-id="2a052-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="2a052-145">Il metodo index () nel listato 3 non accetta parametri.</span><span class="sxs-lookup"><span data-stu-id="2a052-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="2a052-146">L'URL/Home provocherà la chiamata al metodo index ().</span><span class="sxs-lookup"><span data-stu-id="2a052-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="2a052-147">L'URL/Home/Index/3 richiama anche questo metodo (l'ID viene ignorato).</span><span class="sxs-lookup"><span data-stu-id="2a052-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="2a052-148">L'URL/Home corrisponde anche al metodo index () della classe HomeController nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="2a052-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="2a052-149">**Listato 4-HomeController.cs (azione index con parametro Nullable)**</span><span class="sxs-lookup"><span data-stu-id="2a052-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="2a052-150">Nel listato 4, il metodo index () ha un parametro integer.</span><span class="sxs-lookup"><span data-stu-id="2a052-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="2a052-151">Poiché il parametro è un parametro Nullable (il valore può essere null), l'indice () può essere chiamato senza generare un errore.</span><span class="sxs-lookup"><span data-stu-id="2a052-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="2a052-152">Infine, richiamando il metodo index () nel listato 5 con l'URL/Home, viene generata un'eccezione perché il parametro ID *non è* un parametro nullable.</span><span class="sxs-lookup"><span data-stu-id="2a052-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="2a052-153">Se si tenta di richiamare il metodo index (), si ottiene l'errore visualizzato nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="2a052-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="2a052-154">**Listato 5-HomeController.cs (azione index con parametro ID)**</span><span class="sxs-lookup"><span data-stu-id="2a052-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

<span data-ttu-id="2a052-155">[![richiamare un'azione del controller che prevede un valore di parametro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a052-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="2a052-156">**Figura 01**: richiamo di un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2a052-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="2a052-157">L'URL/Home/Index/3, d'altra parte, funziona correttamente con l'azione del controller di indice nel listato 5.</span><span class="sxs-lookup"><span data-stu-id="2a052-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="2a052-158">La richiesta/Home/Index/3 fa sì che il metodo index () venga chiamato con un parametro ID con valore 3.</span><span class="sxs-lookup"><span data-stu-id="2a052-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="2a052-159">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2a052-159">Summary</span></span>

<span data-ttu-id="2a052-160">L'obiettivo di questa esercitazione è fornire una breve introduzione al routing ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a052-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="2a052-161">È stata esaminata la tabella di route predefinita che si ottiene con una nuova applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a052-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="2a052-162">Si è appreso come la route predefinita esegue il mapping degli URL alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="2a052-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2a052-163">avanti</span><span class="sxs-lookup"><span data-stu-id="2a052-163">Next</span></span>](understanding-action-filters-cs.md)
