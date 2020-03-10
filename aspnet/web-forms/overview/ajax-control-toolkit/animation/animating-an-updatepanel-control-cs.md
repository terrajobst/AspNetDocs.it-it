---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animazione di un controllo UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536379"
---
# <a name="animating-an-updatepanel-control-c"></a>Animazione di un controllo UpdatePanel (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un UpdatePanel, esiste una speciale estensione che si basa principalmente sul Framework di animazione: UpdatePanelAnimation. Questa esercitazione illustra come configurare tale animazione per un UpdatePanel.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Per il contenuto di un `UpdatePanel`, esiste una speciale estensione che si basa principalmente sul Framework di animazione: `UpdatePanelAnimation`. Questa esercitazione illustra come configurare tale animazione per un `UpdatePanel`.

## <a name="steps"></a>Passaggi

Il primo passaggio è il solito includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX venga caricata ed è possibile usare il Toolkit di controllo:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a una ASP.NET `Wizard` controllo Web che risiede in una `UpdatePanel`. Tre passaggi (arbitrari) forniscono le opzioni necessarie per attivare i postback:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

Il markup necessario per il controllo `UpdatePanelAnimationExtender` è molto simile al markup utilizzato per il `AnimationExtender`. Nell'attributo `TargetControlID` viene fornito l'`ID` della `UpdatePanel` a cui aggiungere un'animazione; all'interno del controllo `UpdatePanelAnimationExtender`, l'elemento `<Animations>` include il markup XML per le animazioni. Tuttavia esiste una differenza: la quantità di eventi e gestori di eventi è limitata rispetto a `AnimationExtender`. Per `UpdatePanels`, ne esistono solo due:

- `<OnUpdated>` quando UpdatePanel è stato aggiornato
- `<OnUpdating>` all'avvio dell'aggiornamento di UpdatePanel

In questo scenario, il nuovo contenuto del `UpdatePanel` (dopo il postback) deve dissolversi. Questo è il markup necessario per questo:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

A questo punto, ogni volta che si verifica un postback all'interno di UpdatePanel, il nuovo contenuto del pannello si dissolve in modo graduale.

[![passaggio successivo della procedura guidata sta svanendo](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

Il passaggio successivo della procedura guidata è in dissolvenza ([fare clic per visualizzare l'immagine con dimensioni complete](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](changing-an-animation-using-client-side-code-cs.md)
> [Successivo](dynamically-controlling-updatepanel-animations-cs.md)
