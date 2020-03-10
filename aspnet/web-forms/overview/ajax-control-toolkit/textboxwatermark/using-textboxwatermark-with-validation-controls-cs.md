---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilizzo di TextBoxWatermark con i controlliC#di convalida () | Microsoft Docs
author: wenz
description: Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577840"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a>Uso di TextBoxWatermark con i controlli di convalida (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, viene svuotato. Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato. Questo potrebbe essere in conflitto con i controlli di convalida di ASP.NET nella stessa pagina, ma questi problemi possono essere superati.

## <a name="overview"></a>Panoramica

Il controllo `TextBoxWatermark` in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, viene svuotato. Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato. Questo potrebbe essere in conflitto con i controlli di convalida di ASP.NET nella stessa pagina, ma questi problemi possono essere superati.

## <a name="steps"></a>Passaggi

La configurazione di base dell'esempio è la seguente: un controllo `TextBox` è filigranato usando un controllo `TextBoxWatermarkExtender`. Un pulsante attiva un postback e verrà usato in un secondo momento per attivare i controlli di convalida nella pagina. Inoltre, è necessario un controllo `ScriptManager` per inizializzare ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

A questo punto, aggiungere un controllo `RequiredFieldValidator` che controlla se è presente un testo nel campo quando viene inviato il modulo. È necessario impostare la proprietà `InitialValue` del validator sullo stesso valore usato nel controllo `TextBoxWatermarkExtender`: quando il modulo viene inviato, il valore di una casella di testo non modificato è il valore della filigrana al suo interno:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Tuttavia, esiste un problema con questo approccio: se il client Disabilita JavaScript, il campo di testo non viene precompilato con il testo della filigrana, quindi la `RequiredFieldValidator` non attiva un messaggio di errore. Pertanto, è necessario un secondo controllo `RequiredFieldValidator` che controlla la presenza di una casella di testo vuota (omettendo l'attributo `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Poiché entrambi i validator utilizzano `Display`=`"Dynamic"`, l'utente finale non può distinguere dall'aspetto visivo quale dei due validator è stato generato; sembra che sia presente solo uno di essi.

Infine, aggiungere parte del codice sul lato server per restituire il testo nel campo se nessun validator ha emesso un messaggio di errore:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

[![il validator indica che non è presente alcun testo nel campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Il validator indica che non è presente alcun testo nel campo ([fare clic per visualizzare l'immagine con dimensioni complete](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-textboxwatermark-in-a-formview-cs.md)
> [Successivo](using-textboxwatermark-in-a-formview-vb.md)
