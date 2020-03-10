---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Esecuzione di animazioni tramite codice lato client (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Esecuzione dell'animazione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598182"
---
# <a name="executing-animations-using-client-side-code-c"></a>Esecuzione di animazioni tramite codice lato client (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnClick>` per eseguire le animazioni quando l'utente fa clic sul pannello. Aggiungere due animazioni da eseguire in parallelo:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Ai fini della dimostrazione, questa animazione (e qualsiasi altra animazione creata utilizzando il Toolkit di controllo) viene eseguita utilizzando il codice JavaScript, dopo l'esecuzione della pagina. Prima di tutto è necessario accedere al controllo `AnimationExtender`. ASP.NET AJAX Library fornisce la funzione `$find()` per questa attività:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Il controllo `AnimationExtender` espone un'API avanzata, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via. Ad esempio, una chiamata del metodo `OnClick()` esegue l'animazione all'interno dell'elemento `<OnClick>` del controllo `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Ecco il codice JavaScript lato client completo che emula il clic sul pannello dopo che la pagina è stata caricata completamente. si noti che viene usato il nome della funzione `pageLoad()`, che viene chiamato da ASP.NET AJAX una volta che la pagina e tutte le librerie JavaScript incluse sono state caricate.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

[![l'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni complete](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](modifying-animations-from-the-server-side-cs.md)
> [Successivo](changing-an-animation-using-client-side-code-cs.md)
