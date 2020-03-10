---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Creazione di un'azioneC#() | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per l'azione di un metodo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582054"
---
# <a name="creating-an-action-c"></a>Creazione di un'azione (C#)

[Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per l'azione di un metodo.

L'obiettivo di questa esercitazione è spiegare come è possibile creare una nuova azione del controller. Vengono fornite informazioni sui requisiti di un metodo di azione. Si apprenderà anche come impedire che un metodo venga esposto come azione.

## <a name="adding-an-action-to-a-controller"></a>Aggiunta di un'azione a un controller

Per aggiungere una nuova azione a un controller, è necessario aggiungere un nuovo metodo al controller. Il controller nel listato 1, ad esempio, contiene un'azione denominata index () e un'azione denominata SayHello (). Entrambi i metodi sono esposti come azioni.

**Listato 1-Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Per essere esposti all'universo come azione, un metodo deve soddisfare determinati requisiti:

- Il metodo deve essere pubblico.
- Il metodo non può essere un metodo statico.
- Il metodo non può essere un metodo di estensione.
- Il metodo non può essere un costruttore, un getter o un setter.
- Il metodo non può avere tipi generici aperti.
- Il metodo non è un metodo della classe di base del controller.
- Il metodo non può contenere parametri **ref** o **out** .

Si noti che non esistono restrizioni per il tipo restituito di un'azione del controller. Un'azione del controller può restituire una stringa, un valore DateTime, un'istanza della classe casuale o void. Il Framework di MVC ASP.NET converte qualsiasi tipo restituito che non è un risultato di azione in una stringa e ne esegue il rendering nel browser.

Quando si aggiunge un metodo che non viola questi requisiti a un controller, il metodo viene esposto come azione del controller. Prestare attenzione. Un'azione del controller può essere richiamata da chiunque sia connesso a Internet. Non creare, ad esempio, un'azione del controller DeleteMyWebsite ().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedire che un metodo pubblico venga richiamato

Se è necessario creare un metodo pubblico in una classe controller e non si vuole esporre il metodo come un'azione del controller, è possibile impedire che il metodo venga richiamato usando l'attributo [NonAction]. Il controller nel listato 2, ad esempio, contiene un metodo pubblico denominato CompanySecrets () decorato con l'attributo [NonAction].

**Listato 2-Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Se si tenta di richiamare l'azione del controller CompanySecrets () digitando/Work/CompanySecrets nella barra degli indirizzi del browser, si otterrà il messaggio di errore nella figura 1.

[![richiamare un metodo non di azione](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: richiamo di un metodo non di azione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-controller-cs.md)
> [Successivo](asp-net-mvc-routing-overview-vb.md)
