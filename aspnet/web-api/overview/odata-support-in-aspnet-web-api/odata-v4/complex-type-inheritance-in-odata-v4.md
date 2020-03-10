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
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Ereditarietà dei tipi complessi in OData V4 con API Web ASP.NET

[Microsoft](https://github.com/microsoft)

> In base alla [specifica](http://www.odata.org/documentation/odata-version-4-0/)OData V4, un tipo complesso può ereditare da un altro tipo complesso. Un tipo *complesso* è un tipo strutturato senza chiave. L'API Web OData 5,3 supporta l'ereditarietà di tipi complessi.
> 
> In questo argomento viene illustrato come compilare un modello EDM (Entity Data Model) con tipi di ereditarietà complessi. Per il codice sorgente completo, vedere [esempio di ereditarietà dei tipi complessi OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web OData 5,3
> - OData v4

## <a name="model-hierarchy"></a>Gerarchia del modello

Per illustrare l'ereditarietà dei tipi complessi, verrà usata la gerarchia di classi seguente.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` è un tipo complesso astratto. `Rectangle`, `Triangle`e `Circle` sono tipi complessi derivati da `Shape`e `RoundRectangle` deriva da `Rectangle`. `Window` è un tipo di entità e contiene un'istanza di `Shape`.

Di seguito sono riportate le classi CLR che definiscono questi tipi.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Per creare il modello EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deduce le relazioni di ereditarietà dai tipi CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

È anche possibile compilare EDM in modo esplicito tramite **ODataModelBuilder**. Questa operazione richiede un maggior numero di codice, ma offre un maggiore controllo sull'EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

In questi due esempi viene creato lo stesso schema EDM.

## <a name="metadata-document"></a>Documento di metadati

Ecco il documento di metadati OData, che mostra l'ereditarietà del tipo complesso.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Dal documento di metadati, è possibile notare quanto segue:

- Il `Shape` tipo complesso è astratto.
- Il tipo di base di `Rectangle`, `Triangle`e `Circle` è `Shape`.
- Il tipo di `RoundRectangle` dispone del tipo di base `Rectangle`.

## <a name="casting-complex-types"></a>Cast di tipi complessi

Il cast sui tipi complessi è ora supportato. Ad esempio, la query seguente esegue il cast di un `Shape` a un `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Ecco il payload della risposta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
