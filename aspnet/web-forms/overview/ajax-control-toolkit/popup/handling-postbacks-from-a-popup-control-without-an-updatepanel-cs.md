---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestione dei postback da un controllo popup senza UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback in su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612742"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Gestione dei postback da un controllo Popup senza UpdatePanel (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e nella pagina sono presenti diversi pannelli, è difficile determinare su quale pannello è stato fatto clic.

## <a name="overview"></a>Panoramica

Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e nella pagina sono presenti diversi pannelli, è difficile determinare su quale pannello è stato fatto clic.

## <a name="steps"></a>Passaggi

Quando si usa un `PopupControl` con un postback, ma senza avere un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha attivato il popup che a sua volta ha causato il postback. Un piccolo espediente fornisce tuttavia una soluzione alternativa per questo scenario.

Innanzitutto, di seguito è illustrata la configurazione di base: due caselle di testo che attivano entrambe lo stesso popup, un calendario. Due `PopupControlExtenders` riunire caselle di testo e popup.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

L'idea di base consiste nell'aggiungere un campo del form nascosto nel &lt;`form`elemento &gt; che include la casella di testo che ha avviato il popup:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi a entrambe le caselle di testo: ogni volta che l'utente fa clic su una casella di testo, il nome viene scritto nel campo del form nascosto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Nel codice sul lato server è necessario leggere il valore del campo nascosto. Poiché i campi dei moduli nascosti sono semplici da modificare, è necessario un approccio all'elenco di elementi consentiti per convalidare il valore nascosto. Una volta identificata la casella di testo corretta, viene scritta la data dal calendario.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))

[![facendo clic su una data la inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Se si fa clic su una data, la si inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Successivo](using-multiple-popup-controls-vb.md)
