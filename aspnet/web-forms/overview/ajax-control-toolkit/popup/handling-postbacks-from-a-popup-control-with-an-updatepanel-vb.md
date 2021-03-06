---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Gestione dei postback da un controllo popup con UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. È necessario prestare particolare attenzione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612896"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Gestione dei postback da un controllo Popup con UpdatePanel (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback all'interno di un popup di questo tipo, è necessario prestare particolare attenzione.

## <a name="overview"></a>Panoramica

Il dispositivo Extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare un popup quando viene attivato qualsiasi altro controllo. Quando si verifica un postback all'interno di un popup di questo tipo, è necessario prestare particolare attenzione.

## <a name="steps"></a>Passaggi

Quando si usa un `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento della pagina causato dal postback. Il markup seguente definisce un paio di elementi importanti:

- Un controllo `ScriptManager` in modo che ASP.NET AJAX Control Toolkit funzioni
- Due controlli `TextBox` che attiveranno un popup
- Controllo `Panel` che fungerà da popup
- All'interno del pannello, un controllo `Calendar` viene incorporato all'interno di un controllo `UpdatePanel`
- Due controlli `PopupControlExtender` che assegnano il pannello alle caselle di testo

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Si noti che viene impostato l'attributo `OnSelectionChanged` del controllo `Calendar`. Quindi, quando l'utente seleziona una data nel calendario, viene eseguito un postback e viene eseguito il metodo sul lato server `c1_SelectionChanged()`. All'interno di tale metodo, la data corrente deve essere recuperata e scritta nuovamente nella casella di testo.

La sintassi per è la seguente: prima di tutto, è necessario generare un oggetto proxy per il `PopupControlExtender` nella pagina. ASP.NET AJAX Control Toolkit offre il metodo `GetProxyForCurrentPopup()`. L'oggetto restituito da questo metodo supporta il `Commit()` metodo che invia un valore al controllo che ha attivato il popup (non il controllo che ha attivato la chiamata al metodo). Il codice seguente fornisce la data selezionata come argomento per il metodo `Commit()`, causando il codice per la scrittura della data selezionata nella casella di testo:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

A questo punto, ogni volta che si fa clic su una data del calendario, la data selezionata viene visualizzata nella casella di testo associata, creando un controllo selezione data che attualmente è possibile trovare in molti siti Web.

[![il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella[di testo (fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))

[![facendo clic su una data la inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Se si fa clic su una data, la si inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](using-multiple-popup-controls-vb.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
