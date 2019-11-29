---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Aggiunta dinamica di un riquadro Accordion (VB) | Microsoft Docs
author: wenz
description: Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607183"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Aggiunta dinamica di un riquadro Accordion (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.

## <a name="overview"></a>Panoramica di

Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.

## <a name="steps"></a>Passaggi

Il controllo Accordion espone tutte le proprietà importanti al codice lato server. Tra le altre cose, la proprietà `Panes` concede l'accesso alla raccolta di riquadri che costituiscono la fisarmonica. Ogni riquadro è di tipo `AccordionPane`. È pertanto semplice creare un riquadro di questo tipo:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

La proprietà `HeaderContainer` di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro. la proprietà `ContentContainer` di `AccordionPane` esegue la stessa operazione per la sezione del contenuto del riquadro. Ciò consente al codice ASP.NET di aggiungere contenuto ai riquadri:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Infine, i riquadri devono essere aggiunti alla raccolta `Panes` della fisarmonica:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Di seguito è riportato un codice completo sul lato server che aggiunge due riquadri a un controllo di Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

L'unico elemento mancante è la fisarmonica, che dipende dalla presenza del controllo `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Per completare l'esempio, le due classi CSS a cui viene fatto riferimento nel controllo di Accordion forniscono informazioni sullo stile per il browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

[![i dati della fisarmonica sono stati aggiunti dinamicamente dal codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

I dati della fisarmonica sono stati aggiunti dinamicamente dal codice sul lato server ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](databinding-to-an-accordion-vb.md)
