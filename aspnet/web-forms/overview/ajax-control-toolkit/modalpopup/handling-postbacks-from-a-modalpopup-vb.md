---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestione dei postback da un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621527"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a>Gestione dei postback da un controllo ModalPopup (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.

## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Aggiungere quindi un pannello che funge da popup modale. L'utente può immettere un nome e un indirizzo di posta elettronica. Viene usato un pulsante per chiudere il popup e salvare le informazioni. Si noti che l'attributo `OnClick` è impostato in modo che venga eseguito un postback quando si fa clic su questo pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

La pagina stessa è costituita da due etichette per le stesse informazioni: nome e indirizzo di posta elettronica. Per attivare il popup modale viene usato un pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Per visualizzare la finestra popup, aggiungere il controllo `ModalPopupExtender`. Impostare l'attributo `PopupControlID` sull'ID del pannello e `TargetControlID` sull'ID del pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

A questo punto, ogni volta che si fa clic sul pulsante `Save` all'interno del popup modale, viene eseguito il metodo `SaveData()` sul lato server. In cui è possibile salvare i dati immessi in un archivio dati. Per motivi di semplicità, i nuovi dati vengono semplicemente restituiti nell'etichetta:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Inoltre, i controlli TextBox all'interno del popup modale devono essere riempiti con il nome e l'indirizzo di posta elettronica correnti. Tuttavia, questa operazione è necessaria solo quando si verifica un postback. Se è presente un postback, la funzionalità ViewState ASP.NET compilerà automaticamente le caselle di testo con i valori appropriati.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

[![finestra popup modale causa un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Il popup modale causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-modalpopup-with-a-repeater-control-vb.md)
> [Successivo](positioning-a-modalpopup-vb.md)
