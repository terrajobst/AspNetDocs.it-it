---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestione dei postback da un ModalPopup (C#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599080"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Gestione dei postback da un controllo ModalPopup (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.

## <a name="overview"></a>Panoramica di

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale usando i mezzi lato client. Quando viene creato un postback dall'interno del popup, è necessario prestare particolare attenzione.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Aggiungere quindi un pannello che funge da popup modale. L'utente può immettere un nome e un indirizzo di posta elettronica. Viene usato un pulsante per chiudere il popup e salvare le informazioni. Si noti che l'attributo `OnClick` è impostato in modo che venga eseguito un postback quando si fa clic su questo pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

La pagina stessa è costituita da due etichette per le stesse informazioni: nome e indirizzo di posta elettronica. Per attivare il popup modale viene usato un pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Per visualizzare la finestra popup, aggiungere il controllo `ModalPopupExtender`. Impostare l'attributo `PopupControlID` sull'ID del pannello e `TargetControlID` sull'ID del pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

A questo punto, ogni volta che si fa clic sul pulsante `Save` all'interno del popup modale, viene eseguito il metodo `SaveData()` sul lato server. In cui è possibile salvare i dati immessi in un archivio dati. Per motivi di semplicità, i nuovi dati vengono semplicemente restituiti nell'etichetta:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Inoltre, i controlli TextBox all'interno del popup modale devono essere riempiti con il nome e l'indirizzo di posta elettronica correnti. Tuttavia, questa operazione è necessaria solo quando si verifica un postback. Se è presente un postback, la funzionalità ViewState ASP.NET compilerà automaticamente le caselle di testo con i valori appropriati.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

[![finestra popup modale causa un postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Il popup modale causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-modalpopup-with-a-repeater-control-cs.md)
> [Successivo](positioning-a-modalpopup-cs.md)
