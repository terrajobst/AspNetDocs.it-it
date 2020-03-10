---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Uso dei postback con Reorder (VB) | Microsoft Docs
author: wenz
description: Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un ordine di acquisto...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553739"
---
# <a name="using-postbacks-with-reorderlist-vb"></a>Uso di postback con ReorderList (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Il controllo Reorder list in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.

## <a name="overview"></a>Panoramica

Il controllo `ReorderList` in AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite il trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.

## <a name="steps"></a>Passaggi

Esistono diverse origini dati possibili per il controllo `ReorderList`. Uno consiste nell'utilizzare un controllo `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Per associare il codice XML a un controllo `ReorderList` e abilitare i postback, è necessario impostare i seguenti attributi:

- `DataSourceID`: l'ID dell'origine dati
- `SortOrderField`: Proprietà in base alla quale eseguire l'ordinamento
- `AllowReorder`: indica se consentire all'utente di riordinare gli elementi dell'elenco
- `PostBackOnReorder`: indica se creare un postback ogni volta che l'elenco viene ridisposto

Ecco il markup appropriato per il controllo:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

All'interno del controllo `ReorderList`, i dati specifici dell'origine dati possono essere associati tramite il metodo `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

In una posizione arbitraria della pagina, un'etichetta conterrà le informazioni quando si è verificato l'ultimo riordino:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Questa etichetta viene riempita con testo nel codice sul lato server, gestendo il postback:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Infine, per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, è necessario inserire il controllo `ScriptManager` nella pagina:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

[![ogni riordino attiva un postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Ogni riordino attiva un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](drag-and-drop-via-reorderlist-cs.md)
> [Successivo](drag-and-drop-via-reorderlist-vb.md)
