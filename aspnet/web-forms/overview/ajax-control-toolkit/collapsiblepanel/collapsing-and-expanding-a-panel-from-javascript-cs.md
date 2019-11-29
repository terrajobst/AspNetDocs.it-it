---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Compressione ed espansione di un pannello da JavaScript (C#) | Microsoft Docs
author: wenz
description: Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la funzionalità per comprimere il contenuto ed espanderlo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599441"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Compressione ed espansione di un pannello da JavaScript (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la possibilità di comprimerne il contenuto ed espanderlo di nuovo. Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.

## <a name="overview"></a>Panoramica di

Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la possibilità di comprimerne il contenuto ed espanderlo di nuovo. Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, creare una nuova pagina ASP.NET e includere la `ScriptManager` all'interno dell'elemento One `<form>`. Verrà caricata la libreria ASP.NET AJAX richiesta da Control Toolkit:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Quindi, creare un pannello con un testo in modo che l'effetto di compressione/espansione possa essere visualizzato:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Come si può notare, il pannello fa riferimento a una classe CSS visualizzata qui (e fondamentalmente definisce un colore di sfondo e la larghezza del pannello):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Il controllo `CollapsiblePanelExtender` richiede l'attributo `TargetControlID` in modo che il Toolkit conosca il pannello da comprimere o espandere su richiesta:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Sfortunatamente, il dispositivo Extender non espone attualmente un'API specifica per comprimere o espandere il pannello, ma in alcuni metodi non documentati. Prima di tutto, aggiungere tre pulsanti HTML alla pagina che attiverà quindi il codice JavaScript sul lato client per comprimere o espandere il contenuto del pannello:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Nel codice JavaScript lato client (avviato con `<script type="text/javascript">`), è necessario usare il metodo `$find()` per accedere al `CollapsiblePanelExtender`. `$find("cpe")` restituirà un riferimento a tale oggetto. Da quel momento in poi, i metodi specifici risolveranno l'attività.

Il metodo per l'apertura (espansione) del pannello è denominato `_doOpen()`; il codice seguente implementa la funzione `doOpen()` chiamata quando si fa clic sul primo pulsante:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Per la chiusura o la compressione del pannello, è necessario eseguire il `_doClose()` metodo. Quindi, quando l'utente fa clic sul secondo pulsante, viene chiamato il codice JavaScript seguente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Il terzo pulsante consente di visualizzare/disabilitare lo stato del pannello: da compresso a espanso e viceversa. Il `CollapsiblePanelExtender` espone il metodo `toggle()` che esegue esattamente questa operazione: inverte lo stato del pannello. Tuttavia esiste anche un altro approccio (che viene utilizzato internamente dal metodo `toggle()`): il `get_Collapsed()` metodo del `CollapsiblePanelExtender()` indica se il pannello è compresso o meno. A seconda del valore restituito di questa funzione, il pannello viene quindi espanso (`_doOpen()` metodo) o il metodo compresso (`_doClose()`):

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

[![il terzo pulsante cambia lo stato del pannello: da compresso a espanso e viceversa](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Il terzo pulsante modifica lo stato del pannello: da compresso a espanso e viceversa ([fare clic per visualizzare l'immagine con dimensioni complete](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](collapsing-and-expanding-a-panel-from-javascript-vb.md)
