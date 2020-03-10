---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Dettagli elemento visualizzato | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557323"
---
# <a name="display-item-details"></a><span data-ttu-id="cfe8b-102">Visualizzare i dettagli degli elementi</span><span class="sxs-lookup"><span data-stu-id="cfe8b-102">Display Item Details</span></span>

<span data-ttu-id="cfe8b-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfe8b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cfe8b-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="cfe8b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="cfe8b-105">In questa sezione verrà aggiunta la possibilità di visualizzare i dettagli di ogni libro.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="cfe8b-106">In app. js aggiungere il codice seguente al modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="cfe8b-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="cfe8b-107">In views/Home/index. cshtml aggiungere un elemento di associazione dati al collegamento Dettagli:</span><span class="sxs-lookup"><span data-stu-id="cfe8b-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="cfe8b-108">Questa operazione associa il gestore di clic per la &lt;un elemento&gt; alla funzione `getBookDetail` sul modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="cfe8b-109">Nello stesso file sostituire il contrassegno seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe8b-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="cfe8b-110">con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe8b-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="cfe8b-111">Questo markup crea una tabella con associazione a dati per le proprietà del `detail` osservabile nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="cfe8b-112">La sintassi "&lt;!--Ko--&gt;&quot; consente di includere un'associazione Knockout all'esterno di un elemento DOM.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="cfe8b-113">In questo caso, l'associazione `if` causa la visualizzazione di questa sezione di markup solo quando `details` è non null.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="cfe8b-114">A questo punto, se si esegue l'app e si fa clic su uno dei &quot;dettagli&quot; collegamenti, l'app visualizzerà i dettagli del libro.</span><span class="sxs-lookup"><span data-stu-id="cfe8b-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="cfe8b-115">[Precedente](part-7.md)
> [Successivo](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="cfe8b-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
