---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Creazione di caselle di controllo che siC#escludono a vicenda () | Microsoft Docs
author: wenz
description: 'Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606531"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Creazione di caselle di controllo che si escludono a vicenda (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="overview"></a>Panoramica di

Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox. Consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (attributo`Key`). Da tutte le caselle di controllo all'interno dello stesso gruppo, è possibile selezionare una sola volta.

Iniziamo inserendo due caselle di controllo in una nuova pagina ASP.NET. Ci possono essere più, ma due di essi sono sufficienti per illustrare il principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina. Entrambi gli attributi chiave devono avere lo stesso valore, così come gli attributi valore degli elementi del pulsante di opzione HTML devono essere identici per indicare il gruppo a cui appartengono. La proprietà TargetControlID dell'oggetto Extender punta all'ID della casella di controllo.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Includere infine il `ScriptManager` AJAX di ASP.NET, richiesto da tutti gli elementi di ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Salvare ed eseguire la pagina: è possibile selezionare e deselezionare entrambe le caselle di controllo, ma non è possibile selezionare entrambe le caselle di controllo.

[![è possibile selezionare una sola casella di controllo alla volta](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

È possibile controllare una sola casella di controllo alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](creating-mutually-exclusive-checkboxes-vb.md)
