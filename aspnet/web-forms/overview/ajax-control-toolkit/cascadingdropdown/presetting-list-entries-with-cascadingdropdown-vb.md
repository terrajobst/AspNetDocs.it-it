---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Preimpostazione delle voci dell'elenco con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599555"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Preimpostazione delle voci dell'elenco con CascadingDropDown (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Con un po' di codice è possibile che un elemento elenco venga preselezionato dopo che i dati sono stati caricati dinamicamente.

## <a name="overview"></a>Panoramica di

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Con un po' di codice è possibile che un elemento elenco venga preselezionato dopo che i dati sono stati caricati dinamicamente.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown, che fornisce l'URL del servizio Web e le informazioni sul metodo:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Il dispositivo Extender CascadingDropDown chiama quindi in modo asincrono un servizio Web con la firma del metodo seguente:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Il metodo restituisce una matrice di tipo valore CascadingDropDown. Il costruttore del tipo prevede prima la didascalia della voce dell'elenco e quindi il valore (attributo `value` HTML). Se il terzo argomento è impostato su true, l'elemento elenco viene selezionato automaticamente nel browser.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser compilerà l'elenco a discesa con tre fornitori, il secondo preselezionato.

[![l'elenco viene compilato e preselezionato automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

L'elenco viene compilato e preselezionato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-cascadingdropdown-with-a-database-vb.md)
> [Successivo](using-auto-postback-with-cascadingdropdown-vb.md)
