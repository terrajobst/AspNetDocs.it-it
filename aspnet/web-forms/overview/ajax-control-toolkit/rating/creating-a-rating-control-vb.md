---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Creazione di un controllo Rating (VB) | Microsoft Docs
author: wenz
description: Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli. Questa operazione richiede in genere un po' di codice, ma il...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611536"
---
# <a name="creating-a-rating-control-vb"></a><span data-ttu-id="b051c-104">Creazione di un controllo Rating (VB)</span><span class="sxs-lookup"><span data-stu-id="b051c-104">Creating a Rating Control (VB)</span></span>

<span data-ttu-id="b051c-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b051c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b051c-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b051c-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="b051c-107">Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli.</span><span class="sxs-lookup"><span data-stu-id="b051c-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="b051c-108">Questa operazione richiede in genere una certa attività di scrittura del codice, ma il Toolkit di controllo è a disposizione.</span><span class="sxs-lookup"><span data-stu-id="b051c-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="b051c-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="b051c-109">Overview</span></span>

<span data-ttu-id="b051c-110">Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli.</span><span class="sxs-lookup"><span data-stu-id="b051c-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="b051c-111">Questa operazione richiede in genere una certa attività di scrittura del codice, ma il Toolkit di controllo è a disposizione.</span><span class="sxs-lookup"><span data-stu-id="b051c-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="b051c-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="b051c-112">Steps</span></span>

<span data-ttu-id="b051c-113">Prima di tutto, sono necessari almeno due tipi di immagini: uno per un elemento di classificazione compilato e uno per un elemento di classificazione vuoto.</span><span class="sxs-lookup"><span data-stu-id="b051c-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="b051c-114">Un elemento di classificazione è in genere una stella o uno smiley.</span><span class="sxs-lookup"><span data-stu-id="b051c-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="b051c-115">Per questo scenario, sono disponibili tre file, smiley. png e Empty. png e smiley-done. png come parte del download del codice sorgente per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b051c-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="b051c-116">Quindi, creare un nuovo file ASP.NET e iniziare con l'aggiunta di un controllo `ScriptManager`:</span><span class="sxs-lookup"><span data-stu-id="b051c-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="b051c-117">Aggiungere quindi il controllo `Rating` da ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="b051c-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="b051c-118">Per questo esempio è necessario impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b051c-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="b051c-119">`CurrentRating` della classificazione iniziale da usare</span><span class="sxs-lookup"><span data-stu-id="b051c-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="b051c-120">`MaxRating` la classificazione massima</span><span class="sxs-lookup"><span data-stu-id="b051c-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="b051c-121">`EmptyStarCssClass` la classe CSS da utilizzare quando un elemento della classificazione (stella) è vuoto</span><span class="sxs-lookup"><span data-stu-id="b051c-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="b051c-122">`FilledStarCssClass` la classe CSS da utilizzare quando un elemento della classificazione (stella) viene compilato</span><span class="sxs-lookup"><span data-stu-id="b051c-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="b051c-123">`StarCssClass` la classe CSS da utilizzare per una stat visibile</span><span class="sxs-lookup"><span data-stu-id="b051c-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="b051c-124">`WaitingStarCssClass` la classe CSS da usare mentre una classificazione a stelle viene restituita al server</span><span class="sxs-lookup"><span data-stu-id="b051c-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="b051c-125">Di seguito è riportato il markup che consente di creare un controllo Rating con cinque elementi (smiley) di cui non è stato compilato inizialmente alcuno:</span><span class="sxs-lookup"><span data-stu-id="b051c-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="b051c-126">Le tre classi CSS a cui viene fatto riferimento devono ora visualizzare i file di immagine appropriati, operazione semplice con CSS:</span><span class="sxs-lookup"><span data-stu-id="b051c-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="b051c-127">Assicurarsi di specificare la larghezza e l'altezza delle tre immagini; in caso contrario, la visualizzazione potrebbe sembrare un po' complicata.</span><span class="sxs-lookup"><span data-stu-id="b051c-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="b051c-128">Infine, il risultato della classificazione deve essere visualizzato all'utente (o almeno salvato in un database).</span><span class="sxs-lookup"><span data-stu-id="b051c-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="b051c-129">Aggiungere quindi un'etichetta per l'output di un messaggio di testo e un pulsante Submit (Invia) per eseguire il postback del modulo rating al server:</span><span class="sxs-lookup"><span data-stu-id="b051c-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="b051c-130">Nel codice sul lato server accedere al controllo Rating tramite la relativa `ID` e quindi accedere alla relativa proprietà `CurrentRating`, ovvero il numero degli elementi di classificazione selezionati, nell'esempio un valore compreso tra 0 e 5.</span><span class="sxs-lookup"><span data-stu-id="b051c-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="b051c-131">Salvare la pagina e caricarla nel browser.</span><span class="sxs-lookup"><span data-stu-id="b051c-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="b051c-132">Quando si passa il mouse sugli elementi della classificazione (inizialmente vuoti), si verifica un effetto JavaScript: la classificazione viene modificata.</span><span class="sxs-lookup"><span data-stu-id="b051c-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="b051c-133">Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente.</span><span class="sxs-lookup"><span data-stu-id="b051c-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="b051c-134">Infine, quando si invia il modulo, il codice lato server restituisce la classificazione selezionata.</span><span class="sxs-lookup"><span data-stu-id="b051c-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="b051c-135">[![la creazione di un sistema di classificazione con codice minimo](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b051c-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="b051c-136">Creazione di un sistema di classificazione con codice minimo ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b051c-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b051c-137">Precedente</span><span class="sxs-lookup"><span data-stu-id="b051c-137">Previous</span></span>](creating-a-rating-control-cs.md)
