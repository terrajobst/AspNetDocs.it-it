---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Creazione di caselle di controllo che si escludono a vicenda (VB) | Microsoft Docs
author: wenz
description: 'Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554012"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a>Creazione di caselle di controllo che si escludono a vicenda (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="overview"></a>Panoramica

Quando è possibile selezionare solo un set di opzioni, i pulsanti di opzione vengono in genere utilizzati. Si è verificato un svantaggio, tuttavia: una volta selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo possono essere deselezionate in qualsiasi momento, ma non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox. Consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (attributo`Key`). Da tutte le caselle di controllo all'interno dello stesso gruppo, è possibile selezionare una sola volta.

Iniziamo inserendo due caselle di controllo in una nuova pagina ASP.NET. Ci possono essere più, ma due di essi sono sufficienti per illustrare il principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina. Entrambi gli attributi chiave devono avere lo stesso valore, così come gli attributi valore degli elementi del pulsante di opzione HTML devono essere identici per indicare il gruppo a cui appartengono. La proprietà TargetControlID dell'oggetto Extender punta all'ID della casella di controllo.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Includere infine il `ScriptManager` AJAX di ASP.NET, richiesto da tutti gli elementi di ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salvare ed eseguire la pagina: è possibile selezionare e deselezionare entrambe le caselle di controllo, ma non è possibile selezionare entrambe le caselle di controllo.

[![è possibile selezionare una sola casella di controllo alla volta](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

È possibile controllare una sola casella di controllo alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-mutually-exclusive-checkboxes-cs.md)
