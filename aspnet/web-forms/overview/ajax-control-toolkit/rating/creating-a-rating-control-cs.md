---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Creazione di un controllo Rating (c#) | Microsoft Docs
author: wenz
description: Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcune attività di codifica, ma non è disponibile il...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fa118b4d733d7848b838f80e9918d62ae60033af
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59378975"
---
# <a name="creating-a-rating-control-c"></a><span data-ttu-id="27f51-104">Creazione di un controllo Rating (C#)</span><span class="sxs-lookup"><span data-stu-id="27f51-104">Creating a Rating Control (C#)</span></span>

<span data-ttu-id="27f51-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="27f51-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="27f51-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="27f51-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="27f51-107">Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi.</span><span class="sxs-lookup"><span data-stu-id="27f51-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="27f51-108">Ciò in genere richiede alcune attività di codifica, ma è disponibile il Toolkit di controllo a disposizione.</span><span class="sxs-lookup"><span data-stu-id="27f51-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="27f51-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="27f51-109">Overview</span></span>

<span data-ttu-id="27f51-110">Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi.</span><span class="sxs-lookup"><span data-stu-id="27f51-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="27f51-111">Ciò in genere richiede alcune attività di codifica, ma è disponibile il Toolkit di controllo a disposizione.</span><span class="sxs-lookup"><span data-stu-id="27f51-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="27f51-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="27f51-112">Steps</span></span>

<span data-ttu-id="27f51-113">Prima di tutto, è necessario (almeno) due tipi di immagini: uno per una piena la voce rating e uno per un elemento vuoto classificazione.</span><span class="sxs-lookup"><span data-stu-id="27f51-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="27f51-114">Un elemento di classificazione è in genere un smile o una stella.</span><span class="sxs-lookup"><span data-stu-id="27f51-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="27f51-115">Per questo scenario, sono disponibili tre file, smiley.png ed empty.png e faccina done.png durante il download del codice sorgente per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="27f51-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="27f51-116">Quindi, creare un nuovo file ASP.NET e iniziare con l'aggiunta di un `ScriptManager` controllo ad esso:</span><span class="sxs-lookup"><span data-stu-id="27f51-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="27f51-117">Aggiungere quindi il `Rating` controllo da ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="27f51-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="27f51-118">Gli attributi seguenti devono essere impostate per questo esempio:</span><span class="sxs-lookup"><span data-stu-id="27f51-118">The following attributes need to be set for this example:</span></span>

- `CurrentRating` <span data-ttu-id="27f51-119">la valutazione iniziale da usare</span><span class="sxs-lookup"><span data-stu-id="27f51-119">the initial rating to be used</span></span>
- `MaxRating` <span data-ttu-id="27f51-120">la classificazione massima</span><span class="sxs-lookup"><span data-stu-id="27f51-120">the maximum rating</span></span>
- `EmptyStarCssClass` <span data-ttu-id="27f51-121">la classe CSS da utilizzare quando un elemento di classificazione (star) è vuota</span><span class="sxs-lookup"><span data-stu-id="27f51-121">the CSS class to use when a rating item ( star ) is empty</span></span>
- `FilledStarCssClass` <span data-ttu-id="27f51-122">la classe CSS da utilizzare quando viene compilato un elemento di classificazione (star)</span><span class="sxs-lookup"><span data-stu-id="27f51-122">the CSS class to use when a rating item ( star ) is filled out</span></span>
- `StarCssClass` <span data-ttu-id="27f51-123">la classe CSS da utilizzare per un stat visibile</span><span class="sxs-lookup"><span data-stu-id="27f51-123">the CSS class to use for a visible stat</span></span>
- `WaitingStarCssClass` <span data-ttu-id="27f51-124">la classe CSS da utilizzare durante una classificazione a stelle viene inviata al server</span><span class="sxs-lookup"><span data-stu-id="27f51-124">the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="27f51-125">Ed ecco il markup che crea un controllo rating con cinque elementi (smileys) di cui non viene compilato inizialmente:</span><span class="sxs-lookup"><span data-stu-id="27f51-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="27f51-126">Le tre classi CSS di cui si fa riferimento a questo punto necessario visualizzare i file di immagine appropriata, che è possibile farlo usando lo stile CSS con facilità:</span><span class="sxs-lookup"><span data-stu-id="27f51-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="27f51-127">Assicurarsi che si fornisce la larghezza e l'altezza delle tre immagini, in caso contrario, la visualizzazione potrebbe avere un aspetto un po' messed backup.</span><span class="sxs-lookup"><span data-stu-id="27f51-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="27f51-128">Infine, il risultato della valutazione dovrebbe essere visualizzato all'utente (o, almeno salvato in un database).</span><span class="sxs-lookup"><span data-stu-id="27f51-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="27f51-129">In questo caso, aggiungere un'etichetta per l'output di un messaggio di testo e un pulsante di invio per inviare nuovamente il form di classificazione nel server:</span><span class="sxs-lookup"><span data-stu-id="27f51-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="27f51-130">Nel codice lato server, accedere al controllo Rating tramite relativi `ID` e quindi accedere ai relativi `CurrentRating` proprietà che è il numero di elementi di classificazione selezionata, nel nostro esempio, un valore compreso tra 0 e 5.</span><span class="sxs-lookup"><span data-stu-id="27f51-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="27f51-131">Salvare la pagina e caricarla nel browser.</span><span class="sxs-lookup"><span data-stu-id="27f51-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="27f51-132">Quando si posiziona gli elementi di classificazione (inizialmente vuota), si verifica un effetto di JavaScript: Le modifiche di classificazione.</span><span class="sxs-lookup"><span data-stu-id="27f51-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="27f51-133">Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente.</span><span class="sxs-lookup"><span data-stu-id="27f51-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="27f51-134">Infine, quando si invia il form, il codice lato server restituisce la classificazione selezionata.</span><span class="sxs-lookup"><span data-stu-id="27f51-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


[![C<span data-ttu-id="27f51-135">un sistema di valutazione con codice minimo reating]</span><span class="sxs-lookup"><span data-stu-id="27f51-135">reating a rating system with minimal code]</span></span>(creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

<span data-ttu-id="27f51-136">Creazione di un sistema di valutazione con quantità minima di codice ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="27f51-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27f51-137">Successivo</span><span class="sxs-lookup"><span data-stu-id="27f51-137">Next</span></span>](creating-a-rating-control-vb.md)
