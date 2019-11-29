---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: creazione di modelli di dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600365"
---
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="29dfc-102">Parte 2: creazione di modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="29dfc-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="29dfc-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29dfc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="29dfc-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="29dfc-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="29dfc-105">Aggiungi modelli</span><span class="sxs-lookup"><span data-stu-id="29dfc-105">Add Models</span></span>

<span data-ttu-id="29dfc-106">Esistono tre modi per affrontare Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="29dfc-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="29dfc-107">Database-First: si inizia con un database e Entity Framework genera il codice.</span><span class="sxs-lookup"><span data-stu-id="29dfc-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="29dfc-108">Primo modello: si inizia con un modello visivo e Entity Framework genera sia il database che il codice.</span><span class="sxs-lookup"><span data-stu-id="29dfc-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="29dfc-109">Code-First: si inizia con il codice e Entity Framework genera il database.</span><span class="sxs-lookup"><span data-stu-id="29dfc-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="29dfc-110">Viene usato l'approccio Code-First, quindi si inizia definendo gli oggetti di dominio come oggetti POCO (Plain-Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="29dfc-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="29dfc-111">Con l'approccio Code-First, gli oggetti di dominio non necessitano di codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o la persistenza.</span><span class="sxs-lookup"><span data-stu-id="29dfc-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="29dfc-112">In particolare, non è necessario che ereditino dalla classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) . È comunque possibile usare le annotazioni dei dati per controllare il modo in cui Entity Framework crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="29dfc-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="29dfc-113">Poiché i POCO non contengono proprietà aggiuntive che descrivono [lo stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), possono essere facilmente serializzate in formato JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="29dfc-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="29dfc-114">Tuttavia, ciò non significa che sia sempre necessario esporre i modelli di Entity Framework direttamente ai client, come si vedrà più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="29dfc-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="29dfc-115">Si creeranno le seguenti operazioni POCO:</span><span class="sxs-lookup"><span data-stu-id="29dfc-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="29dfc-116">Prodotto</span><span class="sxs-lookup"><span data-stu-id="29dfc-116">Product</span></span>
- <span data-ttu-id="29dfc-117">Order</span><span class="sxs-lookup"><span data-stu-id="29dfc-117">Order</span></span>
- <span data-ttu-id="29dfc-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="29dfc-118">OrderDetail</span></span>

<span data-ttu-id="29dfc-119">Per creare ogni classe, fare clic con il pulsante destro del mouse sulla cartella Models in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="29dfc-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="29dfc-120">Dal menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **classe.**</span><span class="sxs-lookup"><span data-stu-id="29dfc-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="29dfc-121">Aggiungere una classe `Product` con l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="29dfc-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="29dfc-122">Per convenzione, Entity Framework usa la proprietà `Id` come chiave primaria e ne esegue il mapping a una colonna Identity nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="29dfc-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="29dfc-123">Quando si crea una nuova istanza di `Product`, non viene impostato alcun valore per `Id`, perché il database genera il valore.</span><span class="sxs-lookup"><span data-stu-id="29dfc-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="29dfc-124">L'attributo **ScaffoldColumn** indica a ASP.NET MVC di ignorare la proprietà `Id` durante la generazione di un form dell'editor.</span><span class="sxs-lookup"><span data-stu-id="29dfc-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="29dfc-125">L'attributo **required** viene utilizzato per convalidare il modello.</span><span class="sxs-lookup"><span data-stu-id="29dfc-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="29dfc-126">Specifica che la proprietà `Name` deve essere una stringa non vuota.</span><span class="sxs-lookup"><span data-stu-id="29dfc-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="29dfc-127">Aggiungere la classe `Order`:</span><span class="sxs-lookup"><span data-stu-id="29dfc-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="29dfc-128">Aggiungere la classe `OrderDetail`:</span><span class="sxs-lookup"><span data-stu-id="29dfc-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="29dfc-129">Relazioni di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="29dfc-129">Foreign Key Relations</span></span>

<span data-ttu-id="29dfc-130">Un ordine contiene molti dettagli dell'ordine e ogni dettaglio dell'ordine si riferisce a un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="29dfc-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="29dfc-131">Per rappresentare queste relazioni, la classe `OrderDetail` definisce proprietà denominate `OrderId` e `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="29dfc-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="29dfc-132">Entity Framework dedurrà che queste proprietà rappresentano chiavi esterne e aggiungeranno vincoli di chiave esterna al database.</span><span class="sxs-lookup"><span data-stu-id="29dfc-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="29dfc-133">Le classi `Order` e `OrderDetail` includono anche le proprietà "Navigation", che contengono riferimenti agli oggetti correlati.</span><span class="sxs-lookup"><span data-stu-id="29dfc-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="29dfc-134">Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="29dfc-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="29dfc-135">Compilare il progetto ora.</span><span class="sxs-lookup"><span data-stu-id="29dfc-135">Compile the project now.</span></span> <span data-ttu-id="29dfc-136">Entity Framework usa la reflection per individuare le proprietà dei modelli, quindi richiede un assembly compilato per creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="29dfc-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="29dfc-137">Configurare i formattatori del tipo di supporto</span><span class="sxs-lookup"><span data-stu-id="29dfc-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="29dfc-138">Un [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando l'API Web scrive il corpo della risposta http.</span><span class="sxs-lookup"><span data-stu-id="29dfc-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="29dfc-139">I formattatori predefiniti supportano l'output JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="29dfc-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="29dfc-140">Per impostazione predefinita, entrambi i formattatori serializzano tutti gli oggetti in base al valore.</span><span class="sxs-lookup"><span data-stu-id="29dfc-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="29dfc-141">Con la serializzazione per valore viene creato un problema se un oggetto grafico contiene riferimenti circolari.</span><span class="sxs-lookup"><span data-stu-id="29dfc-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="29dfc-142">Questo è esattamente il caso con le classi `Order` e `OrderDetail`, perché ogni oggetto include un riferimento all'altro.</span><span class="sxs-lookup"><span data-stu-id="29dfc-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="29dfc-143">Il formattatore segue i riferimenti, scrive ogni oggetto per valore e passa a cerchi.</span><span class="sxs-lookup"><span data-stu-id="29dfc-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="29dfc-144">Pertanto, è necessario modificare il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="29dfc-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="29dfc-145">In Esplora soluzioni espandere l'app\_cartella di avvio e aprire il file denominato WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="29dfc-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="29dfc-146">Aggiungere il codice seguente alla classe `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="29dfc-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="29dfc-147">Questo codice imposta il formattatore JSON per mantenere i riferimenti agli oggetti e rimuove completamente il formattatore XML dalla pipeline.</span><span class="sxs-lookup"><span data-stu-id="29dfc-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="29dfc-148">È possibile configurare il formattatore XML in modo da mantenere i riferimenti agli oggetti, ma è un po' più lavoro ed è necessario solo JSON per questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="29dfc-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="29dfc-149">Per ulteriori informazioni, vedere [gestione di riferimenti a oggetti circolari](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="29dfc-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="29dfc-150">[Precedente](using-web-api-with-entity-framework-part-1.md)
> [Successivo](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="29dfc-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
