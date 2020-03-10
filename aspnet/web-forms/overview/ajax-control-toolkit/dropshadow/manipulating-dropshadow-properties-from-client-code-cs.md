---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Modifica delle proprietà di DropShadow dal codice clientC#() | Microsoft Docs
author: wenz
description: Personalizzazione dell'interfaccia di modifica di DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613820"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Modifica delle proprietà di DropShadow dal codice client (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.

## <a name="overview"></a>Panoramica

Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. È anche possibile modificare le proprietà di questo Extender usando il codice JavaScript del client.

## <a name="steps"></a>Passaggi

Il codice inizia con un pannello contenente alcune righe di testo:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

La classe CSS associata assegna al pannello un colore di sfondo gradevole:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto ombreggiatura, l'opacità è impostata su 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Quindi, il controllo `ScriptManager` AJAX di ASP.NET consente il funzionamento di Control Toolkit:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Un altro pannello contiene due collegamenti JavaScript per impostare l'opacità dell'ombreggiatura: il collegamento meno diminuisce l'opacità dell'ombreggiatura, il collegamento più lo aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

La funzione JavaScript `changeOpacity()` deve quindi trovare prima di tutto il controllo `DropShadowExtender` nella pagina. ASP.NET AJAX definisce il metodo `$find()` per l'attività stessa. Quindi, il metodo `get_Opacity()` recupera l'opacità corrente, il metodo `set_Opacity()` lo imposta. Il codice JavaScript inserisce quindi il valore di opacità corrente nell'elemento `<label>`:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

[![l'opacità viene modificata sul lato client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

L'opacità viene modificata sul lato client ([fare clic per visualizzare l'immagine con dimensioni complete](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Successivo](adjusting-the-z-index-of-a-dropshadow-vb.md)
