---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Uso di DynamicPopulate con un controllo utente e JavaScriptC#() | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613708"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Uso di DynamicPopulate con un controllo utente e JavaScript (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client. Tuttavia è necessario prestare particolare attenzione quando il dispositivo Extender risiede in un controllo utente.

## <a name="overview"></a>Panoramica

Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client. Tuttavia è necessario prestare particolare attenzione quando il dispositivo Extender risiede in un controllo utente.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato dal controllo `DynamicPopulateExtender`. Il servizio Web implementa il metodo `getDate()` che prevede un argomento di tipo stringa, denominato `contextKey`, poiché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web. Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati seguenti:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo controllo utente (file`.ascx`), indicato dalla seguente dichiarazione nella prima riga:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Viene utilizzato un elemento &lt;`label`&gt; per visualizzare i dati provenienti dal server.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Nel file di controllo utente, inoltre, si utilizzeranno tre pulsanti di opzione, ognuno dei quali rappresenta uno dei tre formati di data possibili supportati dal servizio Web. Quando l'utente fa clic su uno dei pulsanti di opzione, il browser eseguirà codice JavaScript simile al seguente:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Questo codice accede al `DynamicPopulateExtender` (non preoccuparsi ancora dell'ID strano, questo verrà trattato in un secondo momento) e attiverà il popolamento dinamico con i dati. Nel contesto del pulsante di opzione corrente, `this.value` fa riferimento al relativo valore `format1`, `format2` o `format3` esattamente ciò che il metodo Web prevede.

L'unico elemento mancante nel controllo utente è ancora il controllo `DynamicPopulateExtender` che collega i pulsanti di opzione al servizio Web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Anche in questo caso, è possibile notare l'ID strano utilizzato nel controllo: `mcd1$myDate` anziché `myDate`. In precedenza, il codice JavaScript usava `mcd1_dpe1` per accedere al `DynamicPopulateExtender` invece che `dpe1`. Questa strategia di denominazione è un requisito speciale quando si utilizza `DynamicPopulateExtender` all'interno di un controllo utente. Inoltre, è necessario incorporare il controllo utente in un modo specifico per eseguire tutto il lavoro. Creare una nuova pagina ASP.NET e registrare un prefisso tag per il controllo utente appena implementato:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Includere quindi il controllo `ScriptManager` AJAX ASP.NET nella nuova pagina:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Infine, aggiungere il controllo utente alla pagina. È necessario impostare solo il relativo attributo `ID` (e `runat="server"`naturalmente), ma è necessario impostarlo anche su un nome specifico: `mcd1` poiché questo è il prefisso utilizzato all'interno del controllo utente per accedervi utilizzando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

L'operazione è ora completata. Il comportamento della pagina è quello previsto: un utente fa clic su uno dei pulsanti di opzione, il controllo nel Toolkit chiama il servizio Web e visualizza la data corrente nel formato desiderato.

[![i pulsanti di opzione si trovano in un controllo utente](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

I pulsanti di opzione si trovano in un controllo utente ([fare clic per visualizzare l'immagine con dimensioni complete](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Successivo](dynamically-populating-a-control-vb.md)
