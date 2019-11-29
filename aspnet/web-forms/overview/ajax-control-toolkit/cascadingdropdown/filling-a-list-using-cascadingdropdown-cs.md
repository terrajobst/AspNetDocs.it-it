---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Compilazione di un elenco tramite CascadingDropDownC#() | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574817"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a>Compilazione di un elenco tramite CascadingDropDown (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.

## <a name="overview"></a>Panoramica di

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Quindi, è necessario un controllo DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown. Invierà una richiesta asincrona a un servizio Web che restituirà un elenco di voci da visualizzare nell'elenco. Per il corretto funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:

- `ServicePath`: URL di un servizio Web che fornisce le voci dell'elenco
- `ServiceMethod`: metodo Web che fornisce le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: informazioni sulla categoria inviate al metodo Web quando viene chiamata
- `PromptText`: testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server

Ecco il markup per l'elemento `CascadingDropDown`. L'unica differenza tra C# e VB è il nome del servizio Web associato:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Il codice JavaScript proveniente dal `CascadingDropDown` Extender chiama un metodo del servizio Web con la firma seguente:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Quindi, l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definita da ASP.NET AJAX Control Toolkit). Nel costruttore di `CascadingDropDownNameValue`, è necessario specificare prima il testo della voce dell'elenco e quindi il relativo valore, così come `<option value="VALUE">NAME</option>` funzionerebbe in HTML. Ecco alcuni dati di esempio:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Il caricamento della pagina nel browser attiverà l'elenco per compilarlo con tre fornitori.

[![l'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-cascadingdropdown-with-a-database-cs.md)
