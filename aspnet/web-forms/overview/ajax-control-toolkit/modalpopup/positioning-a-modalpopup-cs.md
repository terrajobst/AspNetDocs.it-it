---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Posizionamento di un ModalPopupC#() | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, il controllo non offre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598999"
---
# <a name="positioning-a-modalpopup-c"></a>Posizionamento di un controllo ModalPopup (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, il controllo non offre una funzionalità incorporata per la posizione del popup.

## <a name="overview"></a>Panoramica di

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, il controllo non offre una funzionalità incorporata per la posizione del popup.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e di Control Toolkit, il `ScriptManager`. il controllo deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Aggiungere quindi un pannello che funge da popup modale. Per chiudere il popup, viene usato un pulsante:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Ogni volta che il popup viene visualizzato, deve essere posizionato in una determinata posizione nella pagina. Per questa attività viene creata una funzione JavaScript sul lato client. Tenta prima di tutto di accedere al pannello. In caso di esito positivo, la posizione del pannello viene impostata usando CSS e JavaScript (modificare la posizione del popup a). Tuttavia, il controllo `ModalPopupExtender` tenta anche di posizionare il popup. Il codice JavaScript posiziona quindi ripetutamente il popup, ogni decimo di secondo.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Come si può notare, il valore restituito del metodo `setTimeout()` JavaScript viene salvato in una variabile globale. In questo modo è possibile arrestare il posizionamento ripetuto del popup su richiesta, usando il metodo `clearTimeout()`:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

A questo punto, tutto ciò che resta da fare è fare in modo che il browser chiami queste funzioni quando appropriato. La funzione `movePanel()` JavaScript deve essere chiamata quando si fa clic sul pulsante che attiva il pannello:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

E la funzione `stopMoving()` entra in gioco quando il popup è chiuso, può essere attivato nel controllo `ModalPopupExtender`:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

[![il popup modale viene visualizzato nella posizione designata](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

La finestra popup modale viene visualizzata nella posizione designata ([fare clic per visualizzare l'immagine con dimensioni complete](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-modalpopup-cs.md)
> [Successivo](launching-a-modal-popup-window-from-server-code-vb.md)
