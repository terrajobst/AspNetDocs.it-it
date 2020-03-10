---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creare un singleton in OData V4 usando l'API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Questo argomento illustra come definire un singleton in un endpoint OData nell'API Web 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622087"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Creare un singleton in OData V4 usando l'API Web 2,2

di Zoe Luo

> Tradizionalmente, è possibile accedere a un'entità solo se è stata incapsulata all'interno di un set di entità. Tuttavia OData V4 fornisce due opzioni aggiuntive, singleton e contenimento, entrambe supportate da WebAPI 2,2.

Questo articolo illustra come definire un singleton in un endpoint OData nell'API Web 2,2. Per informazioni su un singleton e su come sfruttarlo, vedere [utilizzo di un singleton per definire un'entità speciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Per creare un endpoint OData V4 nell'API Web, vedere [creare un endpoint OData V4 usando API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md). 

Verrà creato un singleton nel progetto API Web usando il modello di dati seguente:

![Modello dati](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton denominato `Umbrella` verrà definito in base al tipo `Company`e verrà definito un set di entità denominato `Employees` in base al tipo `Employee`.

La soluzione usata in questa esercitazione può essere scaricata da [codeplex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generare il modello EDM basato sui tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    `builder.Singleton<Company>("Umbrella")` indica al generatore di modelli di creare un singleton denominato `Umbrella` nel modello EDM.

    I metadati generati appariranno come segue:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Dai metadati è possibile vedere che la proprietà di navigazione `Company` nel set di entità `Employees` è associata al `Umbrella`singleton. L'associazione viene eseguita automaticamente da `ODataConventionModelBuilder`, perché solo `Umbrella` dispone del tipo di `Company`. In caso di ambiguità nel modello, è possibile utilizzare `HasSingletonBinding` per associare in modo esplicito una proprietà di navigazione a un singleton; `HasSingletonBinding` ha lo stesso effetto dell'uso dell'attributo `Singleton` nella definizione del tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definire il controller singleton

Come il controller EntitySet, il controller singleton eredita da `ODataController`e il nome del controller singleton deve essere `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Per gestire diversi tipi di richieste, è necessario che le azioni siano predefinite nel controller. Il **routing degli attributi** è abilitato per impostazione predefinita in WebAPI 2,2. Ad esempio, per definire un'azione per gestire l'esecuzione di query `Revenue` da `Company` tramite il routing degli attributi, usare quanto segue:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Se non si è disposti a definire gli attributi per ogni azione, è sufficiente definire le azioni che seguono le [convenzioni di routing OData](../odata-routing-conventions.md). Poiché una chiave non è necessaria per l'esecuzione di query su un singleton, le azioni definite nel controller Singleton sono leggermente diverse dalle azioni definite nel controller EntitySet.

Per riferimento, le firme del metodo per ogni definizione di azione nel controller Singleton sono elencate di seguito.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Fondamentalmente, questo è tutto ciò che è necessario fare sul lato del servizio. Il [progetto di esempio](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene tutto il codice per la soluzione e il client OData che Mostra come usare il singleton. Il client viene compilato seguendo la procedura descritta in [creare un'app client OData V4](create-an-odata-v4-client-app.md).

. 

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
