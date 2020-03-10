---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Disabilitazione delle azioni durante l'C#animazione () | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Supporta inoltre l'azione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536267"
---
# <a name="disabling-actions-during-animation-c"></a>Disabilitazione delle azioni durante l'animazione (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Supporta inoltre azioni, come i clic del mouse. Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Supporta inoltre azioni, come i clic del mouse. Tuttavia, quando un clic del mouse avvia un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pulsante HTML simile al seguente:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Si noti che viene usato un controllo HTML al posto di un controllo Web poiché non si vuole che il pulsante crei un postback; deve semplicemente avviare l'animazione sul lato client per noi.

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

All'interno del nodo `<Animations>` `<OnClick>` è l'elemento corretto per gestire il clic del mouse. Tuttavia, è possibile fare clic sul pulsante anche durante l'animazione. L'elemento `<EnableAction>` può occuparsi di questo. L'impostazione `Enabled="false"` Disabilita il pulsante come parte dell'animazione. Poiché vengono usate diverse animazioni singole (disabilitando il pulsante e le animazioni effettive), l'elemento `<Parallel>` è necessario per incollare le singole animazioni in una sola. Ecco il markup completo per `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

È anche possibile riabilitare il pulsante dopo l'animazione, usando l'elemento XML seguente alla fine dell'elenco:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Tuttavia, nello scenario specificato, questo sarebbe inutile perché il pulsante scompare e non è visibile alla fine dell'animazione.

[![il pulsante è disabilitato non appena viene eseguita l'animazione](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Il pulsante è disabilitato non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni complete](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-in-response-to-user-interaction-cs.md)
> [Successivo](triggering-an-animation-in-another-control-cs.md)
