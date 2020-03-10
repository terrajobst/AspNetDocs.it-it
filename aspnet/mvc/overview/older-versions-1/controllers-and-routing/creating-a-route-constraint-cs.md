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
# <a name="creating-a-route-constraint-c"></a>Creazione di un vincolo di route (C#)

di [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther illustra come è possibile controllare il modo in cui le richieste del browser corrispondono alle route creando vincoli di route con espressioni regolari.

Usare i vincoli di route per limitare le richieste del browser che corrispondono a una determinata route. È possibile usare un'espressione regolare per specificare un vincolo di route.

Si supponga, ad esempio, di aver definito la route nel listato 1 nel file Global. asax.

**Listato 1-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Il listato 1 contiene una route denominata Product. È possibile usare la route prodotto per eseguire il mapping delle richieste del browser a ProductController contenute nel listato 2.

**Listato 2-Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Si noti che l'azione dettagli () esposta dal controller del prodotto accetta un solo parametro denominato productId. Questo parametro è un parametro integer.

La route definita nel listato 1 corrisponderà a uno qualsiasi degli URL seguenti:

- /Product/23
- /Product/7

Sfortunatamente, la route corrisponderà anche agli URL seguenti:

- /Product/blah
- /Product/apple

Poiché l'azione Details () prevede un parametro Integer, l'esecuzione di una richiesta che contiene un valore diverso da un Integer provocherà un errore. Se ad esempio si digita l'URL/Product/Apple nel browser, si otterrà la pagina di errore nella figura 1.

[![finestra di dialogo nuovo progetto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figura 01**: visualizzazione di una pagina esplosa ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-route-constraint-cs/_static/image2.png))

Ciò che realmente si vuole fare è trovare solo gli URL che contengono un intero productId appropriato. È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route. La route del prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo a numeri interi.

**Listato 3-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

L'espressione regolare \d + corrisponde a uno o più numeri interi. Questo vincolo causa la corrispondenza tra la route del prodotto e gli URL seguenti:

- /Product/3
- /Product/8999

Ma non gli URL seguenti:

- /Product/apple
- /Product

- Queste richieste del browser verranno gestite da un'altra route o, se non sono presenti route corrispondenti, verrà restituito un errore relativo *alla risorsa non trovata* .

> [!div class="step-by-step"]
> [Precedente](creating-custom-routes-cs.md)
> [Successivo](creating-a-custom-route-constraint-cs.md)
