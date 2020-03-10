---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Creazione di un vincolo di route (VB) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther illustra come è possibile controllare il modo in cui le richieste del browser corrispondono alle route creando vincoli di route con espressioni regolari.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601388"
---
# <a name="creating-a-route-constraint-vb"></a>Creazione di un vincolo di route (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther illustra come è possibile controllare il modo in cui le richieste del browser corrispondono alle route creando vincoli di route con espressioni regolari.

Usare i vincoli di route per limitare le richieste del browser che corrispondono a una determinata route. È possibile usare un'espressione regolare per specificare un vincolo di route.

Si supponga, ad esempio, di aver definito la route nel listato 1 nel file Global. asax.

**Listato 1-Global. asax. vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Il listato 1 contiene una route denominata Product. È possibile usare la route prodotto per eseguire il mapping delle richieste del browser a ProductController contenute nel listato 2.

**Listato 2-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Si noti che l'azione dettagli () esposta dal controller del prodotto accetta un solo parametro denominato productId. Questo parametro è un parametro integer.

La route definita nel listato 1 corrisponderà a uno qualsiasi degli URL seguenti:

- /Product/23
- /Product/7

Sfortunatamente, la route corrisponderà anche agli URL seguenti:

- /Product/blah
- /Product/apple

Poiché l'azione Details () prevede un parametro Integer, l'esecuzione di una richiesta che contiene un valore diverso da un Integer provocherà un errore. Se ad esempio si digita l'URL/Product/Apple nel browser, si otterrà la pagina di errore nella figura 1.

[![finestra di dialogo nuovo progetto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: visualizzazione di una pagina esplosa ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-route-constraint-vb/_static/image2.png))

Ciò che realmente si vuole fare è trovare solo gli URL che contengono un intero productId appropriato. È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route. La route del prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo a numeri interi.

**Listato 3-Global. asax. vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

L'espressione regolare \d + corrisponde a uno o più numeri interi. Questo vincolo causa la corrispondenza tra la route del prodotto e gli URL seguenti:

- /Product/3
- /Product/8999

Ma non gli URL seguenti:

- /Product/apple
- /Product

Queste richieste del browser verranno gestite da un'altra route o, se non sono presenti route corrispondenti, verrà restituito un errore relativo *alla risorsa non trovata* .

> [!div class="step-by-step"]
> [Precedente](creating-custom-routes-vb.md)
> [Successivo](creating-a-custom-route-constraint-vb.md)
