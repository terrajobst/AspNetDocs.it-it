---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modifica delle animazioni dal lato server (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Anche le animazioni possono...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575216"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Modifica delle animazioni dal lato server (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Il resto del codice viene eseguito sul lato server e non usa il markup; USA invece il codice per creare il controllo `AnimationExtender`:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Tuttavia, il Toolkit di controllo non fornisce attualmente l'accesso all'API per creare le singole animazioni. È tuttavia possibile impostare la proprietà Animations di `AnimationExtender`su una stringa contenente il markup XML utilizzato durante l'assegnazione delle animazioni in modo dichiarativo. Per creare il codice XML che non deve contenere l'elemento `<Animations>` è possibile utilizzare il supporto XML .NET Framework o, come nel codice seguente, fornire solo la stringa:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Infine, aggiungere il controllo `AnimationExtender` alla pagina corrente, all'interno dell'elemento `<form runat="server">`, verificando che l'animazione sia inclusa ed esegua:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

[![l'animazione viene creata utilizzando il codice/VB C#lato server](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

L'animazione viene creata usando il codice/VB C#lato server ([fare clic per visualizzare l'immagine con dimensioni complete](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](triggering-an-animation-in-another-control-vb.md)
> [Successivo](executing-animations-using-client-side-code-vb.md)
