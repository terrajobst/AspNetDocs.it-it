---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Creazione di un vincolo di routeC#personalizzato () | Microsoft Docs
author: StephenWalther
description: Stephen Walther illustra come è possibile creare un vincolo di route personalizzato. Viene implementato un semplice vincolo personalizzato che impedisce la corrispondenza di una route con...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601451"
---
# <a name="creating-a-custom-route-constraint-c"></a>Creazione di un vincolo di route personalizzato (C#)

di [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther illustra come è possibile creare un vincolo di route personalizzato. Viene implementato un semplice vincolo personalizzato che impedisce la corrispondenza di una route quando viene effettuata una richiesta del browser da un computer remoto.

L'obiettivo di questa esercitazione è illustrare come è possibile creare un vincolo di route personalizzato. Un vincolo di route personalizzato consente di impedire la corrispondenza di una route, a meno che non venga stabilita una corrispondenza per alcune condizioni personalizzate.

In questa esercitazione viene creato un vincolo di route localhost. Il vincolo di route localhost corrisponde solo alle richieste effettuate dal computer locale. Le richieste remote da Internet non corrispondono.

Implementare un vincolo di route personalizzato implementando l'interfaccia IRouteConstraint. Si tratta di un'interfaccia estremamente semplice che descrive un solo metodo:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Il metodo restituisce un valore booleano. Se si restituisce false, la route associata al vincolo non corrisponderà alla richiesta del browser.

Il vincolo localhost è contenuto nel listato 1.

**Listato 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Il vincolo nel listato 1 sfrutta la proprietà setlocale esposta dalla classe HttpRequest. Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta è uguale all'indirizzo IP del server.

Si usa un vincolo personalizzato all'interno di una route definita nel file Global. asax. Il file Global. asax nel listato 2 usa il vincolo localhost per impedire a chiunque di richiedere una pagina di amministrazione, a meno che non effettui la richiesta dal server locale. Ad esempio, una richiesta di/Admin/DeleteAll avrà esito negativo se eseguita da un server remoto.

**Listato 2-Global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Il vincolo localhost viene usato nella definizione della route di amministrazione. Questa route non verrà confrontata con una richiesta del browser remoto. Tenere presente, tuttavia, che altre route definite in Global. asax potrebbero corrispondere alla stessa richiesta. È importante comprendere che un vincolo impedisce a una determinata route di abbinare una richiesta e non tutte le route definite nel file Global. asax.

Si noti che la route predefinita è stata impostata come commento dal file Global. asax nel listato 2. Se si include la route predefinita, la route predefinita corrisponderà alle richieste per il controller di amministrazione. In tal caso, gli utenti remoti possono comunque richiamare le azioni del controller di amministrazione anche se le richieste non corrispondono alla route di amministrazione.

> [!div class="step-by-step"]
> [Precedente](creating-a-route-constraint-cs.md)
> [Successivo](asp-net-mvc-controller-overview-vb.md)
