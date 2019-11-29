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
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="e6346-104">Aggiunta dinamica di un riquadro Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="e6346-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="e6346-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e6346-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e6346-106">[Scarica codice](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e6346-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="e6346-107">Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="e6346-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e6346-108">I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="e6346-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="e6346-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="e6346-109">Overview</span></span>

<span data-ttu-id="e6346-110">Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="e6346-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e6346-111">I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="e6346-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="e6346-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="e6346-112">Steps</span></span>

<span data-ttu-id="e6346-113">Il controllo Accordion espone tutte le proprietà importanti al codice lato server.</span><span class="sxs-lookup"><span data-stu-id="e6346-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="e6346-114">Tra le altre cose, la proprietà `Panes` concede l'accesso alla raccolta di riquadri che costituiscono la fisarmonica.</span><span class="sxs-lookup"><span data-stu-id="e6346-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="e6346-115">Ogni riquadro è di tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="e6346-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="e6346-116">È pertanto semplice creare un riquadro di questo tipo:</span><span class="sxs-lookup"><span data-stu-id="e6346-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="e6346-117">La proprietà `HeaderContainer` di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro. la proprietà `ContentContainer` di `AccordionPane` esegue la stessa operazione per la sezione del contenuto del riquadro.</span><span class="sxs-lookup"><span data-stu-id="e6346-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="e6346-118">Ciò consente al codice ASP.NET di aggiungere contenuto ai riquadri:</span><span class="sxs-lookup"><span data-stu-id="e6346-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="e6346-119">Infine, i riquadri devono essere aggiunti alla raccolta `Panes` della fisarmonica:</span><span class="sxs-lookup"><span data-stu-id="e6346-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="e6346-120">Di seguito è riportato un codice completo sul lato server che aggiunge due riquadri a un controllo di Accordion:</span><span class="sxs-lookup"><span data-stu-id="e6346-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="e6346-121">L'unico elemento mancante è la fisarmonica, che dipende dalla presenza del controllo `ScriptManager` ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e6346-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="e6346-122">Per completare l'esempio, le due classi CSS a cui viene fatto riferimento nel controllo di Accordion forniscono informazioni sullo stile per il browser:</span><span class="sxs-lookup"><span data-stu-id="e6346-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

<span data-ttu-id="e6346-123">[![i dati della fisarmonica sono stati aggiunti dinamicamente dal codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6346-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="e6346-124">I dati della fisarmonica sono stati aggiunti dinamicamente dal codice sul lato server ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e6346-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e6346-125">Precedente</span><span class="sxs-lookup"><span data-stu-id="e6346-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
