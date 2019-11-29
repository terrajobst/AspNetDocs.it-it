---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modifica delle animazioni dal lato server (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Anche le animazioni possono...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 0594efea9598a6c2461a72f789b5bd5f8ece23da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575162"
---
# <a name="modifying-animations-from-the-server-side-c"></a>Modifica delle animazioni dal lato server (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Il resto del codice viene eseguito sul lato server e non usa il markup; USA invece il codice per creare il controllo `AnimationExtender`:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Tuttavia, il Toolkit di controllo non fornisce attualmente l'accesso all'API per creare le singole animazioni. È tuttavia possibile impostare la proprietà Animations di `AnimationExtender`su una stringa contenente il markup XML utilizzato durante l'assegnazione delle animazioni in modo dichiarativo. Per creare il codice XML che non deve contenere l'elemento `<Animations>` è possibile utilizzare il supporto XML .NET Framework o, come nel codice seguente, fornire solo la stringa:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Infine, aggiungere il controllo `AnimationExtender` alla pagina corrente, all'interno dell'elemento `<form runat="server">`, verificando che l'animazione sia inclusa ed esegua:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]

[![l'animazione viene creata utilizzando il codice/VB C#lato server](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

L'animazione viene creata usando il codice/VB C#lato server ([fare clic per visualizzare l'immagine con dimensioni complete](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](triggering-an-animation-in-another-control-cs.md)
> [Successivo](executing-animations-using-client-side-code-cs.md)
