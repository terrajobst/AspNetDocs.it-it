---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Popolamento dinamico di un controllo tramite codice JavaScript (C#) | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599260"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Popolamento dinamico di un controllo tramite codice JavaScript (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.

## <a name="overview"></a>Panoramica di

Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato dal controllo `DynamicPopulateExtender`. Il servizio Web implementa il metodo `getDate()` che prevede un argomento di tipo stringa, denominato `contextKey`, poiché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web. Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati seguenti:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo sito di ASP.NET e iniziare con il controllo ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio utilizzando il controllo HTML con lo stesso nome o il controllo Web `<asp:Label />`) che in seguito visualizzerà il risultato della chiamata al servizio Web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Includere quindi un controllo `DynamicPopulateExtender` e fornire informazioni sul servizio Web, il controllo di destinazione, ma non il nome del controllo che attiva il popolamento. questa operazione verrà eseguita in un secondo momento usando JavaScript personalizzato.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

A questo punto, la parte relativa a JavaScript. La funzione `$find()`, definita dalla libreria ASP.NET AJAX, restituisce un riferimento a oggetti lato server di ASP.NET AJAX Control Toolkit, ad esempio `DynamicPopulateExtender`. Nel file corrente `$find("dpe")` restituisce un riferimento a un controllo `DynamicPopulateExtender` nella pagina. Espone un metodo denominato `populate()` che attiva il processo di popolamento dinamico. Il metodo `populate()` richiede un argomento: la chiave di contesto che fungerà da argomento per il metodo Web `getDate()`. Quindi, ad esempio, `$find("dpe").populate("format1")` compilerà l'etichetta con la data corrente nel formato mese-giorno-anno.

Per rendere l'esempio leggermente più flessibile, l'utente può ora scegliere tra diversi formati di data. Per ognuno di essi, viene visualizzato un pulsante di opzione. Quando l'utente fa clic su un pulsante di opzione, il codice JavaScript popola in modo dinamico l'etichetta con il formato di data selezionato. Ecco i pulsanti di opzione:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Si noti che all'interno del contesto di un pulsante di opzione, l'espressione JavaScript `this.value` si riferisce al valore del pulsante corrente, che è esattamente le stesse informazioni che il metodo `getDate()` può utilizzare.

[![facendo clic sul pulsante viene recuperata la data dal server, nel formato specificato](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Facendo clic sul pulsante viene recuperata la data dal server, nel formato specificato ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-populating-a-control-cs.md)
> [Successivo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
