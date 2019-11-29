---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Modifica di un'animazione tramite codice lato client (C#) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'animazione può anche...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606967"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Modifica di un'animazione tramite codice lato client (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. È inoltre possibile modificare l'animazione utilizzando codice JavaScript lato client personalizzato.

## <a name="overview"></a>Panoramica di

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. È inoltre possibile modificare l'animazione utilizzando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

L'animazione effettiva viene avviata da un pulsante HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Si noti che non è presente alcun nodo `<Animations>` all'interno del controllo `AnimationExtender`. Il codice JavaScript personalizzato viene usato per fornire le animazioni da usare con il controllo.

Come per l'API server di `AnimationExtender`, non esiste ancora un modo semplice per assegnare un'animazione al dispositivo Extender. Tuttavia, il dispositivo Extender espone diversi metodi per leggere e scrivere animazioni registrate con i vari eventi (`OnClick`, `OnLoad`e così via). Ecco alcuni esempi:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Il formato del valore restituito delle funzioni `get_*()` e il formato dell'argomento per le funzioni `set_*()` è una stringa JSON che fornisce una rappresentazione dell'oggetto del markup XML. Attualmente, non esiste alcun modo per passare un oggetto in, ma è possibile leggere un oggetto da un'animazione specificata (`get_OnXXXBehavior()` metodi).

Ecco una stringa JSON (senza le virgolette di delimitazione e formattata in modo corretto) che rappresenta un'animazione attivata dal pulsante, ma animando il pannello mediante il ridimensionamento e la dissolvenza allo stesso tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Il codice JavaScript seguente assegna questo descrittore JSON all'animazione `OnClick` del dispositivo Extender corrente e lo esegue:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

[![l'animazione viene eseguita immediatamente, senza un clic del mouse (e con un markup molto ridotto)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse (e con un markup molto breve) ([fare clic per visualizzare l'immagine con dimensioni complete](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](executing-animations-using-client-side-code-cs.md)
> [Successivo](animating-an-updatepanel-control-cs.md)
