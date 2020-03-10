---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Creazione di un vincolo diC#Route () | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther illustra come è possibile controllare il modo in cui le richieste del browser corrispondono alle route creando vincoli di route con espressioni regolari.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582012"
---
# <a name="creating-a-route-constraint-c"></a><span data-ttu-id="cf178-103">Creazione di un vincolo di route (C#)</span><span class="sxs-lookup"><span data-stu-id="cf178-103">Creating a Route Constraint (C#)</span></span>

<span data-ttu-id="cf178-104">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="cf178-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="cf178-105">In questa esercitazione, Stephen Walther illustra come è possibile controllare il modo in cui le richieste del browser corrispondono alle route creando vincoli di route con espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="cf178-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="cf178-106">Usare i vincoli di route per limitare le richieste del browser che corrispondono a una determinata route.</span><span class="sxs-lookup"><span data-stu-id="cf178-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="cf178-107">È possibile usare un'espressione regolare per specificare un vincolo di route.</span><span class="sxs-lookup"><span data-stu-id="cf178-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="cf178-108">Si supponga, ad esempio, di aver definito la route nel listato 1 nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="cf178-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="cf178-109">**Listato 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="cf178-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="cf178-110">Il listato 1 contiene una route denominata Product.</span><span class="sxs-lookup"><span data-stu-id="cf178-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="cf178-111">È possibile usare la route prodotto per eseguire il mapping delle richieste del browser a ProductController contenute nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="cf178-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="cf178-112">**Listato 2-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="cf178-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="cf178-113">Si noti che l'azione dettagli () esposta dal controller del prodotto accetta un solo parametro denominato productId.</span><span class="sxs-lookup"><span data-stu-id="cf178-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="cf178-114">Questo parametro è un parametro integer.</span><span class="sxs-lookup"><span data-stu-id="cf178-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="cf178-115">La route definita nel listato 1 corrisponderà a uno qualsiasi degli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf178-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="cf178-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="cf178-116">/Product/23</span></span>
- <span data-ttu-id="cf178-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="cf178-117">/Product/7</span></span>

<span data-ttu-id="cf178-118">Sfortunatamente, la route corrisponderà anche agli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf178-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="cf178-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="cf178-119">/Product/blah</span></span>
- <span data-ttu-id="cf178-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="cf178-120">/Product/apple</span></span>

<span data-ttu-id="cf178-121">Poiché l'azione Details () prevede un parametro Integer, l'esecuzione di una richiesta che contiene un valore diverso da un Integer provocherà un errore.</span><span class="sxs-lookup"><span data-stu-id="cf178-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="cf178-122">Se ad esempio si digita l'URL/Product/Apple nel browser, si otterrà la pagina di errore nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="cf178-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="cf178-123">[![finestra di dialogo nuovo progetto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cf178-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="cf178-124">**Figura 01**: visualizzazione di una pagina esplosa ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="cf178-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>

<span data-ttu-id="cf178-125">Ciò che realmente si vuole fare è trovare solo gli URL che contengono un intero productId appropriato.</span><span class="sxs-lookup"><span data-stu-id="cf178-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="cf178-126">È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route.</span><span class="sxs-lookup"><span data-stu-id="cf178-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="cf178-127">La route del prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo a numeri interi.</span><span class="sxs-lookup"><span data-stu-id="cf178-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="cf178-128">**Listato 3-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="cf178-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="cf178-129">L'espressione regolare \d + corrisponde a uno o più numeri interi.</span><span class="sxs-lookup"><span data-stu-id="cf178-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="cf178-130">Questo vincolo causa la corrispondenza tra la route del prodotto e gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf178-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="cf178-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="cf178-131">/Product/3</span></span>
- <span data-ttu-id="cf178-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="cf178-132">/Product/8999</span></span>

<span data-ttu-id="cf178-133">Ma non gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf178-133">But not the following URLs:</span></span>

- <span data-ttu-id="cf178-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="cf178-134">/Product/apple</span></span>
- <span data-ttu-id="cf178-135">/Product</span><span class="sxs-lookup"><span data-stu-id="cf178-135">/Product</span></span>

- <span data-ttu-id="cf178-136">Queste richieste del browser verranno gestite da un'altra route o, se non sono presenti route corrispondenti, verrà restituito un errore relativo *alla risorsa non trovata* .</span><span class="sxs-lookup"><span data-stu-id="cf178-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cf178-137">[Precedente](creating-custom-routes-cs.md)
> [Successivo](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="cf178-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
