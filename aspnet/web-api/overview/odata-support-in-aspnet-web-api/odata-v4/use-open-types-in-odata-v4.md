---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Tipi aperti in OData V4 con API Web ASP.NET | Microsoft Docs
author: microsoft
description: In OData V4, un tipo aperto è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo. Apri...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622178"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Aprire i tipi in OData V4 con API Web ASP.NET

[Microsoft](https://github.com/microsoft)

> In OData V4, un *tipo aperto* è un tipo strutturato che contiene proprietà dinamiche, oltre a tutte le proprietà dichiarate nella definizione del tipo. I tipi aperti consentono di aggiungere flessibilità ai modelli di dati. Questa esercitazione illustra come usare i tipi aperti in API Web ASP.NET OData.
> 
> In questa esercitazione si presuppone che si sappia già come creare un endpoint OData in API Web ASP.NET. In caso contrario, iniziare leggendo prima [creare un endpoint OData V4](create-an-odata-v4-endpoint.md) .
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web OData 5,3
> - OData v4

Per prima cosa, si tratta di una terminologia OData:

- Tipo di entità: tipo strutturato con una chiave.
- Tipo complesso: tipo strutturato senza chiave.
- Open Type: tipo con proprietà dinamiche. È possibile aprire sia i tipi di entità sia i tipi complessi.

Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione. o una raccolta di questi tipi. Per ulteriori informazioni sui tipi aperti, vedere la [specifica OData V4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installare le librerie OData Web

Usare Gestione pacchetti NuGet per installare le librerie OData dell'API Web più recenti. Dalla finestra console di gestione pacchetti:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definire i tipi CLR

Per iniziare, definire i modelli EDM come tipi CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando viene creato il Entity Data Model (EDM),

- `Category` è un tipo di enumerazione.
- `Address` è un tipo complesso. Non dispone di una chiave, pertanto non è un tipo di entità.
- `Customer` è un tipo di entità. (Contiene una chiave).
- `Press` è un tipo complesso aperto.
- `Book` è un tipo di entità aperto.

Per creare un tipo aperto, il tipo CLR deve avere una proprietà di tipo `IDictionary<string, object>`, che contiene le proprietà dinamiche.

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Se si utilizza **ODataConventionModelBuilder** per creare EDM, `Press` e `Book` vengono aggiunti automaticamente come tipi aperti, in base alla presenza di una proprietà `IDictionary<string, object>`.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

È anche possibile compilare EDM in modo esplicito tramite **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Aggiungere un controller OData

Successivamente, aggiungere un controller OData. Per questa esercitazione verrà usato un controller semplificato che supporta solo richieste GET e POST e usa un elenco in memoria per archiviare le entità.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Si noti che la prima istanza di `Book` non dispone di proprietà dinamiche. La seconda istanza di `Book` presenta le proprietà dinamiche seguenti:

- "Published": tipo primitivo
- "Authors": raccolta di tipi primitivi
- "OtherCategories": raccolta di tipi di enumerazione.

Inoltre, la proprietà `Press` dell'istanza di `Book` presenta le proprietà dinamiche seguenti:

- "Blog": tipo primitivo
- "Address": tipo complesso

## <a name="query-the-metadata"></a>Eseguire una query sui metadati

Per ottenere il documento di metadati OData, inviare una richiesta GET a `~/$metadata`. Il corpo della risposta dovrebbe essere simile al seguente:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Dal documento di metadati, è possibile notare quanto segue:

- Per i tipi `Book` e `Press`, il valore dell'attributo `OpenType` è true. I tipi `Customer` e `Address` non dispongono di questo attributo.
- Il tipo di entità `Book` dispone di tre proprietà dichiarate: ISBN, title e premere. I metadati OData non includono la proprietà `Book.Properties` dalla classe CLR.
- Analogamente, il `Press` tipo complesso dispone solo di due proprietà dichiarate: nome e categoria. I metadati non includono la proprietà `Press.DynamicProperties` dalla classe CLR.

## <a name="query-an-entity"></a>Eseguire una query su un'entità

Per ottenere il libro con ISBN uguale a "978-0-7356-7942-9", inviare una richiesta GET a `~/Books('978-0-7356-7942-9')`. Il corpo della risposta sarà simile al seguente. (Rientrato per renderlo più leggibile).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.

## <a name="post-an-entity"></a>PUBBLICA un'entità

Per aggiungere un'entità Book, inviare una richiesta POST a `~/Books`. Il client può impostare le proprietà dinamiche nel payload della richiesta.

Di seguito è riportato un esempio di richiesta. Prendere nota delle proprietà "price" e "Published".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web ha aggiunto queste proprietà al dizionario `Properties`.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di tipo Open OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
