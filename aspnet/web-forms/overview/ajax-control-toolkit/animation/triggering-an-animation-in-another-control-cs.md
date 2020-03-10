---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Attivazione di un'animazione in un altro controlloC#() | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In generale, l'avvio di...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536078"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Attivazione di un'animazione in un altro controllo (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. In genere, l'avvio di un'animazione viene attivato dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione di un altro controllo.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Per iniziare ad animare il pannello, viene usato un pulsante HTML. Si noti che `<input type="button" />` è favorito da `<asp:Button />` poiché non si desidera un postback quando l'utente fa clic su tale pulsante.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio. È importante impostare `TargetControlID` sull'ID del pulsante (l'elemento che attiva l'animazione), non sull'ID del pannello (l'elemento a cui si sta aggiungendo un'animazione)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

All'interno del nodo `<Animations>` posizionare le animazioni come di consueto. Per fare in modo che modifichino il pannello, non il pulsante, impostare l'attributo `AnimationTarget` per ogni elemento di animazione all'interno `AnimationExtender`. Il valore di `AnimationTarget` è l'ID del pannello, ovviamente. In questo modo, le animazioni avvengono con il pannello, non con il pulsante di attivazione. Ecco il markup `AnimationExtender` per questo scenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni. Prima di tutto, il pulsante viene disattivato dopo l'esecuzione dell'animazione. Poiché nell'elemento `<EnableAction>` non è presente alcun attributo `AnimationTarget`, questa animazione viene applicata al controllo di origine: il pulsante. I due passaggi successivi dell'animazione verranno eseguiti in parallelo (elemento`<Parallel>`). Entrambi gli attributi `AnimationTarget` impostati su `"Panel1"`, animando quindi il pannello, non il pulsante.

[![un clic del mouse sul pulsante viene avviata l'animazione del pannello](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Un clic del mouse sul pulsante avvia l'animazione del pannello ([fare clic per visualizzare l'immagine con dimensioni complete](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](disabling-actions-during-animation-cs.md)
> [Successivo](modifying-animations-from-the-server-side-cs.md)
