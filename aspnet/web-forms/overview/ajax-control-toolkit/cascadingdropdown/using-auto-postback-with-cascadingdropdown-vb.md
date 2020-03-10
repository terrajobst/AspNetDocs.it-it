---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Uso del postback automatico con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535924"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a>Uso del postback automatico con CascadingDropDown (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario). Con alcuni codici JavaScript, questo effetto può essere evitato.

## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario). Con alcuni codici JavaScript, questo effetto può essere evitato.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina, ma all'interno dell'elemento &lt;`form`&gt;:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown, che fornisce l'URL del servizio Web e le informazioni sul metodo:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

Il dispositivo Extender CascadingDropDown chiama quindi in modo asincrono un servizio Web con la firma del metodo seguente:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

Il metodo restituisce una matrice di tipo valore CascadingDropDown. Il costruttore del tipo prevede prima la didascalia della voce dell'elenco e quindi il valore (attributo `value` HTML).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser compilerà l'elenco a discesa con tre fornitori, il secondo preselezionato. Inoltre, ASP.NET definisce il metodo JavaScript `__doPostBack()`. Una volta caricata la pagina, questa chiamata JavaScript viene aggiunta all'elenco a discesa, ma solo se sono presenti elementi. Se nell'elenco non sono presenti elementi, il Toolkit di controllo li sta caricando, in modo che il codice JavaScript usi un timeout e tenti di nuovo tra mezzo secondo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

In questo modo, viene eseguito un postback solo quando sono effettivamente presenti elementi nell'elenco e l'utente seleziona una voce.

[![la selezione di un elemento elenco causa un postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

La selezione di un elemento di elenco causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](presetting-list-entries-with-cascadingdropdown-vb.md)
