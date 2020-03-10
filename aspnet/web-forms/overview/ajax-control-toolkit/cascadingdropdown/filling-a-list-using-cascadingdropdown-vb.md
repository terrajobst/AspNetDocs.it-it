---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Compilazione di un elenco tramite CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536008"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Compilazione di un elenco tramite CascadingDropDown (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.

## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown. Invierà una richiesta asincrona a un servizio Web che restituirà un elenco di voci da visualizzare nell'elenco. Per il corretto funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:

- `ServicePath`: URL di un servizio Web che fornisce le voci dell'elenco
- `ServiceMethod`: metodo Web che fornisce le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: informazioni sulla categoria inviate al metodo Web quando viene chiamata
- `PromptText`: testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server

Ecco il markup per l'elemento `CascadingDropDown`. L'unica differenza tra C# e VB è il nome del servizio Web associato:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Il codice JavaScript proveniente dal `CascadingDropDown` Extender chiama un metodo del servizio Web con la firma seguente:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Quindi, l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definita da ASP.NET AJAX Control Toolkit). Nel costruttore di `CascadingDropDownNameValue`, è necessario specificare prima il testo della voce dell'elenco e quindi il relativo valore, così come `<option value="VALUE">NAME</option>` funzionerebbe in HTML. Ecco alcuni dati di esempio:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser attiverà l'elenco per compilarlo con tre fornitori.

[![l'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-auto-postback-with-cascadingdropdown-cs.md)
> [Successivo](using-cascadingdropdown-with-a-database-vb.md)
