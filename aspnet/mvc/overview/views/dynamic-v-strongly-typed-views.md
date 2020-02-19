---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Confronto tra visualizzazioni dinamiche Visualizzazioni fortemente tipizzate | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455661"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="86aca-103">Confronto tra visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="86aca-103">Dynamic v.</span></span> <span data-ttu-id="86aca-104">e fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="86aca-104">Strongly Typed Views</span></span>

<span data-ttu-id="86aca-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86aca-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="86aca-106">Esistono tre modi per passare le informazioni da un controller a una visualizzazione in ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="86aca-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="86aca-107">Come oggetto modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="86aca-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="86aca-108">Come tipo dinamico (usando @model dinamico)</span><span class="sxs-lookup"><span data-stu-id="86aca-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="86aca-109">Uso di ViewBag</span><span class="sxs-lookup"><span data-stu-id="86aca-109">Using the ViewBag</span></span>

<span data-ttu-id="86aca-110">Ho scritto una semplice applicazione di Blog MVC 3 top per confrontare e contrapporre le visualizzazioni dinamiche e fortemente tipizzate.</span><span class="sxs-lookup"><span data-stu-id="86aca-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="86aca-111">Il controller inizia con un semplice elenco di Blog:</span><span class="sxs-lookup"><span data-stu-id="86aca-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="86aca-112">Fare clic con il pulsante destro del mouse sul metodo IndexNotStonglyTyped () e aggiungere una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="86aca-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="86aca-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86aca-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="86aca-114">Assicurarsi che la casella **Crea una visualizzazione fortemente tipizzata** non sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="86aca-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="86aca-115">La visualizzazione risultante non contiene molto:</span><span class="sxs-lookup"><span data-stu-id="86aca-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="86aca-116">Poiché si sta usando una visualizzazione dinamica e non fortemente tipizzata, IntelliSense non ci aiuta.</span><span class="sxs-lookup"><span data-stu-id="86aca-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="86aca-117">Il codice completato è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="86aca-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="86aca-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="86aca-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="86aca-119">A questo punto si aggiungerà una visualizzazione fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="86aca-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="86aca-120">Aggiungere il codice seguente al controller:</span><span class="sxs-lookup"><span data-stu-id="86aca-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="86aca-121">Si noti che si tratta esattamente della stessa visualizzazione restituita (Blogs); chiamare come visualizzazione non fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="86aca-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="86aca-122">Fare clic con il pulsante destro del mouse all'interno di *StonglyTypedIndex ()* e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="86aca-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="86aca-123">Questa volta selezionare la classe del modello di **Blog** e selezionare **Elenca** come modello di impalcatura.</span><span class="sxs-lookup"><span data-stu-id="86aca-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="86aca-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="86aca-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="86aca-125">All'interno del nuovo modello di vista si ottiene il supporto IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="86aca-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="86aca-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="86aca-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="86aca-127">Il progetto c# può essere scaricato [qui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="86aca-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
