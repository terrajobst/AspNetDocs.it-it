---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Ereditarietà dei tipi complessi in OData V4 con API Web ASP.NET | Microsoft Docs
author: microsoft
description: In base alla specifica OData V4, un tipo complesso può ereditare da un altro tipo complesso. Un tipo complesso è un tipo strutturato senza chiave. API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556308"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="47547-104">Ereditarietà dei tipi complessi in OData V4 con API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="47547-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="47547-105">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="47547-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="47547-106">In base alla [specifica](http://www.odata.org/documentation/odata-version-4-0/)OData V4, un tipo complesso può ereditare da un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="47547-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="47547-107">Un tipo *complesso* è un tipo strutturato senza chiave. L'API Web OData 5,3 supporta l'ereditarietà di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="47547-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="47547-108">In questo argomento viene illustrato come compilare un modello EDM (Entity Data Model) con tipi di ereditarietà complessi.</span><span class="sxs-lookup"><span data-stu-id="47547-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="47547-109">Per il codice sorgente completo, vedere [esempio di ereditarietà dei tipi complessi OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="47547-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="47547-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="47547-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="47547-111">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="47547-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="47547-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="47547-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="47547-113">Gerarchia del modello</span><span class="sxs-lookup"><span data-stu-id="47547-113">Model Hierarchy</span></span>

<span data-ttu-id="47547-114">Per illustrare l'ereditarietà dei tipi complessi, verrà usata la gerarchia di classi seguente.</span><span class="sxs-lookup"><span data-stu-id="47547-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="47547-115">`Shape` è un tipo complesso astratto.</span><span class="sxs-lookup"><span data-stu-id="47547-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="47547-116">`Rectangle`, `Triangle`e `Circle` sono tipi complessi derivati da `Shape`e `RoundRectangle` deriva da `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="47547-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="47547-117">`Window` è un tipo di entità e contiene un'istanza di `Shape`.</span><span class="sxs-lookup"><span data-stu-id="47547-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="47547-118">Di seguito sono riportate le classi CLR che definiscono questi tipi.</span><span class="sxs-lookup"><span data-stu-id="47547-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="47547-119">Compilare il modello EDM</span><span class="sxs-lookup"><span data-stu-id="47547-119">Build the EDM Model</span></span>

<span data-ttu-id="47547-120">Per creare il modello EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deduce le relazioni di ereditarietà dai tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="47547-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="47547-121">È anche possibile compilare EDM in modo esplicito tramite **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="47547-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="47547-122">Questa operazione richiede un maggior numero di codice, ma offre un maggiore controllo sull'EDM.</span><span class="sxs-lookup"><span data-stu-id="47547-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="47547-123">In questi due esempi viene creato lo stesso schema EDM.</span><span class="sxs-lookup"><span data-stu-id="47547-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="47547-124">Documento di metadati</span><span class="sxs-lookup"><span data-stu-id="47547-124">Metadata Document</span></span>

<span data-ttu-id="47547-125">Ecco il documento di metadati OData, che mostra l'ereditarietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="47547-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="47547-126">Dal documento di metadati, è possibile notare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="47547-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="47547-127">Il `Shape` tipo complesso è astratto.</span><span class="sxs-lookup"><span data-stu-id="47547-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="47547-128">Il tipo di base di `Rectangle`, `Triangle`e `Circle` è `Shape`.</span><span class="sxs-lookup"><span data-stu-id="47547-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="47547-129">Il tipo di `RoundRectangle` dispone del tipo di base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="47547-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="47547-130">Cast di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="47547-130">Casting Complex Types</span></span>

<span data-ttu-id="47547-131">Il cast sui tipi complessi è ora supportato.</span><span class="sxs-lookup"><span data-stu-id="47547-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="47547-132">Ad esempio, la query seguente esegue il cast di un `Shape` a un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="47547-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="47547-133">Ecco il payload della risposta:</span><span class="sxs-lookup"><span data-stu-id="47547-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
