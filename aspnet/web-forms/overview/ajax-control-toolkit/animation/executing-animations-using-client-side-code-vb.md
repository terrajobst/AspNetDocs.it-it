---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Esecuzione di animazioni tramite codice lato client (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Esecuzione dell'animazione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575502"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Esecuzione di animazioni tramite codice lato client (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'esecuzione dell'animazione può anche essere attivata utilizzando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnClick>` per eseguire le animazioni quando l'utente fa clic sul pannello. Aggiungere due animazioni da eseguire in parallelo:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Ai fini della dimostrazione, questa animazione (e qualsiasi altra animazione creata utilizzando il Toolkit di controllo) viene eseguita utilizzando il codice JavaScript, dopo l'esecuzione della pagina. Prima di tutto è necessario accedere al controllo `AnimationExtender`. ASP.NET AJAX Library fornisce la funzione `$find()` per questa attività:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Il controllo `AnimationExtender` espone un'API avanzata, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via. Ad esempio, una chiamata del metodo `OnClick()` esegue l'animazione all'interno dell'elemento `<OnClick>` del controllo `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Ecco il codice JavaScript lato client completo che emula il clic sul pannello dopo che la pagina è stata caricata completamente. si noti che viene usato il nome della funzione `pageLoad()`, che viene chiamato da ASP.NET AJAX una volta che la pagina e tutte le librerie JavaScript incluse sono state caricate.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![l'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni complete](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](modifying-animations-from-the-server-side-vb.md)
> [Successivo](changing-an-animation-using-client-side-code-vb.md)
