---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Avvio di una finestra popup modale dal codice server (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, alcuni scenari richiedono che t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606575"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a>Avvio di una finestra popup modale dal codice server (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.

## <a name="overview"></a>Panoramica di

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Tuttavia, alcuni scenari richiedono che l'apertura del popup modale venga attivata sul lato server.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un controllo Web del pulsante ASP.NET per illustrare il funzionamento del controllo ModalPopup. Aggiungere tale pulsante all'interno dell'elemento &lt;form&gt; in una nuova pagina:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Quindi, è necessario il markup per il popup che si vuole creare. Definirlo come controllo `<asp:Panel>` e verificare che includa un controllo Button. Il controllo ModalPopup offre la funzionalità che consente a un pulsante di chiudere il popup; in caso contrario, non esiste un modo semplice per lasciarlo svanire.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Aggiungere quindi il controllo ModalPopup da ASP.NET AJAX Toolkit alla pagina. Impostare le proprietà per il pulsante che carica il controllo, il pulsante che lo rende scomparire e l'ID del popup effettivo.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Come con tutte le pagine Web basate su ASP.NET AJAX; Gestione script è necessario per caricare le librerie JavaScript necessarie per i diversi browser di destinazione:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Eseguire l'esempio nel browser. Quando si fa clic sul pulsante, viene visualizzata la finestra popup modale. Per ottenere lo stesso effetto usando il codice lato server, è necessario un nuovo pulsante:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Come si può notare, un clic sul pulsante genera un postback ed esegue il metodo `ServerButton_Click()` sul server. In questo metodo, una funzione JavaScript denominata `launchModal()` viene eseguita per essere esatta, la funzione JavaScript verrà eseguita dopo che la pagina è stata caricata:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Il processo di `launchModal()` consiste nel visualizzare il ModalPopup. La funzione `launchModal()` viene eseguita dopo il caricamento della pagina HTML completa. In quel momento, tuttavia, il framework ASP.NET AJAX non è ancora stato completamente caricato. Pertanto, la funzione `launchModal()` imposta semplicemente una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

La funzione `pageLoad()` JavaScript è una funzione speciale che viene eseguita dopo il caricamento completo di ASP.NET AJAX. Quindi, aggiungiamo codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

La funzione `$find()` Cerca un elemento denominato nella pagina e prevede l'ID del lato server come parametro. Pertanto, `$find("mpe")` restituisce la rappresentazione client del controllo ModalPopup. il metodo `show()` consente di visualizzare il popup.

[![finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

La finestra popup modale viene visualizzata quando si fa clic su uno dei pulsanti ([fare clic per visualizzare l'immagine con dimensioni complete](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](positioning-a-modalpopup-cs.md)
> [Successivo](using-modalpopup-with-a-repeater-control-vb.md)
