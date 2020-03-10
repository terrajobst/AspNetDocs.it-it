---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Aggiunta dinamica di un riquadro Accordion (C#) | Microsoft Docs
author: wenz
description: Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614464"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Aggiunta dinamica di un riquadro Accordion (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.

## <a name="overview"></a>Panoramica

Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.

## <a name="steps"></a>Passaggi

Il controllo Accordion espone tutte le proprietà importanti al codice lato server. Tra le altre cose, la proprietà `Panes` concede l'accesso alla raccolta di riquadri che costituiscono la fisarmonica. Ogni riquadro è di tipo `AccordionPane`. È pertanto semplice creare un riquadro di questo tipo:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

La proprietà `HeaderContainer` di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro. la proprietà `ContentContainer` di `AccordionPane` esegue la stessa operazione per la sezione del contenuto del riquadro. Ciò consente al codice ASP.NET di aggiungere contenuto ai riquadri:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Infine, i riquadri devono essere aggiunti alla raccolta `Panes` della fisarmonica:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Di seguito è riportato un codice completo sul lato server che aggiunge due riquadri a un controllo di Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

L'unico elemento mancante è la fisarmonica, che dipende dalla presenza del controllo `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Per completare l'esempio, le due classi CSS a cui viene fatto riferimento nel controllo di Accordion forniscono informazioni sullo stile per il browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![i dati della fisarmonica sono stati aggiunti dinamicamente dal codice lato server](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

I dati della fisarmonica sono stati aggiunti dinamicamente dal codice sul lato server ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](databinding-to-an-accordion-cs.md)
> [Successivo](databinding-to-an-accordion-vb.md)
