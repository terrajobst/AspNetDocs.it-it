---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Creazione di route personalizzate (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come modificare la tabella di route predefinita nel file Global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 22b44e9e575c9d404881a23ee735bb0c8b7109e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601297"
---
# <a name="creating-custom-routes-vb"></a><span data-ttu-id="81561-104">Creazione di route personalizzate (VB)</span><span class="sxs-lookup"><span data-stu-id="81561-104">Creating Custom Routes (VB)</span></span>

<span data-ttu-id="81561-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="81561-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="81561-106">Informazioni su come aggiungere route personalizzate a un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81561-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="81561-107">In questa esercitazione si apprenderà come modificare la tabella di route predefinita nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="81561-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="81561-108">In questa esercitazione si apprenderà come aggiungere una route personalizzata a un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81561-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="81561-109">Si apprenderà come modificare la tabella di route predefinita nel file Global. asax con una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="81561-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="81561-110">Nelle applicazioni MVC ASP.NET la tabella di route predefinita funzionerà correttamente.</span><span class="sxs-lookup"><span data-stu-id="81561-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="81561-111">Tuttavia, si potrebbe scoprire di avere esigenze specifiche di routing.</span><span class="sxs-lookup"><span data-stu-id="81561-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="81561-112">In tal caso, è possibile creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="81561-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="81561-113">Si supponga, ad esempio, che si stia compilando un'applicazione Blog.</span><span class="sxs-lookup"><span data-stu-id="81561-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="81561-114">Potrebbe essere necessario gestire le richieste in ingresso simili alle seguenti:</span><span class="sxs-lookup"><span data-stu-id="81561-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="81561-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="81561-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="81561-116">Quando un utente immette questa richiesta, si vuole restituire il post di Blog che corrisponde alla data 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="81561-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="81561-117">Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="81561-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="81561-118">Il file Global. asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste che hanno un aspetto simile a/Archive/*Data di immissione*.</span><span class="sxs-lookup"><span data-stu-id="81561-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="81561-119">**Listato 1-Global. asax (con route personalizzata)**</span><span class="sxs-lookup"><span data-stu-id="81561-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="81561-120">L'ordine delle route aggiunte alla tabella di route è importante.</span><span class="sxs-lookup"><span data-stu-id="81561-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="81561-121">La nuova route di Blog personalizzata viene aggiunta prima della route predefinita esistente.</span><span class="sxs-lookup"><span data-stu-id="81561-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="81561-122">Se l'ordine è stato invertito, la route predefinita verrà sempre chiamata al posto della route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="81561-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="81561-123">La route di Blog personalizzata corrisponde a qualsiasi richiesta che inizia con/Archive/.</span><span class="sxs-lookup"><span data-stu-id="81561-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="81561-124">Quindi, corrisponde a tutti gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="81561-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="81561-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="81561-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="81561-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="81561-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="81561-127">/Archive/apple</span><span class="sxs-lookup"><span data-stu-id="81561-127">/Archive/apple</span></span>

<span data-ttu-id="81561-128">La route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato Archive e richiama l'azione Entry ().</span><span class="sxs-lookup"><span data-stu-id="81561-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="81561-129">Quando viene chiamato il metodo Entry (), la data di immissione viene passata come parametro denominato entryDate.</span><span class="sxs-lookup"><span data-stu-id="81561-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="81561-130">È possibile usare la route personalizzata del Blog con il controller nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="81561-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="81561-131">**Listato 2-ArchiveController. vb**</span><span class="sxs-lookup"><span data-stu-id="81561-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="81561-132">Si noti che il metodo Entry () nell'elenco 2 accetta un parametro di tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="81561-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="81561-133">Il framework MVC è sufficientemente intelligente da convertire automaticamente la data di immissione dall'URL in un valore DateTime.</span><span class="sxs-lookup"><span data-stu-id="81561-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="81561-134">Se il parametro della data di immissione dell'URL non può essere convertito in DateTime, viene generato un errore (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="81561-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="81561-135">**Figura 1-errore durante la conversione del parametro**</span><span class="sxs-lookup"><span data-stu-id="81561-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="81561-136">[![finestra di dialogo nuovo progetto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="81561-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="81561-137">**Figura 01**: errore durante la conversione del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="81561-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="81561-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="81561-138">Summary</span></span>

<span data-ttu-id="81561-139">L'obiettivo di questa esercitazione è illustrare come creare una route personalizzata.</span><span class="sxs-lookup"><span data-stu-id="81561-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="81561-140">Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global. asax che rappresenta le voci di Blog.</span><span class="sxs-lookup"><span data-stu-id="81561-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="81561-141">È stato illustrato come eseguire il mapping delle richieste per le voci di Blog a un controller denominato ArchiveController e a un'azione del controller denominata entry ().</span><span class="sxs-lookup"><span data-stu-id="81561-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="81561-142">[Precedente](asp-net-mvc-controller-overview-vb.md)
> [Successivo](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="81561-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
