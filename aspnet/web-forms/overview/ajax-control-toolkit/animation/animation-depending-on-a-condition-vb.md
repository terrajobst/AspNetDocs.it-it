---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animazione in base a una condizione (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Indica se un'animazione è...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614345"
---
# <a name="animation-depending-on-a-condition-vb"></a>Animazione in base a una condizione (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Se un'animazione viene eseguita o meno può dipendere anche da una condizione sotto forma di codice JavaScript.

## <a name="overview"></a>Panoramica

Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Se un'animazione viene eseguita o meno può dipendere anche da una condizione sotto forma di codice JavaScript.

## <a name="steps"></a>Passaggi

Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

L'animazione verrà applicata a un pannello di testo simile al seguente:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Aggiungere quindi il `AnimationExtender` alla pagina, specificando un `ID`, l'attributo `TargetControlID` e il `runat="server":` obbligatorio

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

All'interno del nodo `<Animations>` usare `<OnLoad>` per eseguire le animazioni dopo che la pagina è stata caricata completamente. Anziché una delle animazioni regolari, l'elemento `<Condition>` entra in gioco. Il codice JavaScript fornito come valore dell'attributo `ConditionScript` viene eseguito in fase di esecuzione. Se restituisce true, l'animazione viene eseguita; in caso contrario,. Il markup seguente fornisce due animazioni, ognuna delle quali viene eseguita nel 50% dei case in modo casuale. Poiché può essere presente solo un'animazione all'interno `<OnLoad>`, le due animazioni `<Condition>` sono unite in join tramite l'elemento `<Sequence>`:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Si noti che il segno di minore di (`<`) nell'attributo `ConditionScript` deve essere preceduto da un carattere di escape (). Quando si esegue questo script, non viene eseguita alcuna animazione o uno dei due esegue o entrambi.

[![il pannello si dissolve senza ridimensionare, quindi viene eseguita la seconda animazione, la prima non è stata](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Il pannello si dissolve senza ridimensionare, quindi la seconda animazione viene eseguita, la prima non è stata eseguita ([fare clic per visualizzare l'immagine con dimensioni complete](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](executing-several-animations-after-each-other-vb.md)
> [Successivo](picking-one-animation-out-of-a-list-vb.md)
