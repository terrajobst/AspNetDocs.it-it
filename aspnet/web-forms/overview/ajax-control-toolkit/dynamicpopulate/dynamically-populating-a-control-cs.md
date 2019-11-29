---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Popolamento dinamico di un controllo (C#) | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 24f88e44e0f878127314774d4e8846f80133413e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599266"
---
# <a name="dynamically-populating-a-control-c"></a>Popolamento dinamico di un controllo (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.

## <a name="overview"></a>Panoramica di

Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina. Questa esercitazione illustra come impostare questa impostazione.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato da `DynamicPopulate`. La classe del servizio Web richiede l'attributo `ScriptService` definito all'interno `Microsoft.Web.Script.Services`; in caso contrario, ASP.NET AJAX non è in grado di creare il proxy JavaScript sul lato client per il servizio Web, che a sua volta è richiesto da `DynamicPopulate`.

Il metodo Web deve prevedere un argomento di tipo stringa, denominato `contextKey`, perché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web. Il servizio Web seguente restituisce la data corrente in un formato rappresentato dall'argomento `contextKey`:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Il servizio Web viene quindi salvato come `DynamicPopulate.cs.asmx`. In alternativa, è possibile implementare il metodo `getDate()` come metodo di pagina all'interno della pagina ASP.NET effettiva con il controllo `DynamicPopulate`.

Nel passaggio successivo creare un nuovo file ASP.NET. Come sempre, il primo passaggio consiste nell'includere il `ScriptManager` nella pagina corrente per caricare la libreria ASP.NET AJAX e fare in modo che il Toolkit di controllo funzioni:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio utilizzando il controllo HTML con lo stesso nome o il &lt;`asp:Label` /controllo Web &gt;) che in seguito visualizzerà il risultato della chiamata al servizio Web.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Un pulsante HTML (come controllo HTML, dal momento che non è necessario un postback al server) verrà usato per attivare il popolamento dinamico:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Infine, è necessario il controllo `DynamicPopulateExtender` per collegare le cose. Verranno impostati i seguenti attributi (oltre a quelli più evidenti, `ID` e `runat`=`"server"`):

- `TargetControlID` dove inserire il risultato dalla chiamata al servizio Web
- `ServicePath` percorso del servizio Web (omettere se si desidera utilizzare un metodo di pagina)
- nome `ServiceMethod` del metodo Web o del metodo di pagina
- `ContextKey` le informazioni sul contesto da inviare al servizio Web
- `PopulateTriggerControlID` elemento che attiva la chiamata al servizio Web
- `ClearContentsDuringUpdate` se svuotare l'elemento di destinazione durante la chiamata al servizio Web

Come si può notare, il controllo richiede alcune informazioni, ma l'inserimento di tutti gli elementi è piuttosto semplice. Ecco il markup per il controllo `DynamicPopulateExtender` nello scenario corrente:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Eseguire la pagina ASP.NET nel browser e fare clic sul pulsante. si riceverà la data corrente nel formato mese-giorno-anno.

[![un clic sul pulsante Recupera la data dal server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Facendo clic sul pulsante viene recuperata la data dal server ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-populating-a-control-using-javascript-code-cs.md)
