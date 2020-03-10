---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la visualizzazione (interfaccia utente) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557302"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="34526-102">Creare la visualizzazione (interfaccia utente)</span><span class="sxs-lookup"><span data-stu-id="34526-102">Create the View (UI)</span></span>

<span data-ttu-id="34526-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="34526-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="34526-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="34526-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="34526-105">In questa sezione si inizierà a definire il codice HTML per l'app e si aggiungeranno data binding tra il codice HTML e il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34526-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="34526-106">Aprire il file views/Home/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="34526-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="34526-107">Sostituire l'intero contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="34526-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="34526-108">La maggior parte degli elementi `div` è disponibile per lo stile [bootstrap](http://getbootstrap.com/) .</span><span class="sxs-lookup"><span data-stu-id="34526-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="34526-109">Gli elementi importanti sono quelli con attributi `data-bind`.</span><span class="sxs-lookup"><span data-stu-id="34526-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="34526-110">Questo attributo collega il codice HTML al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34526-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="34526-111">Esempio:</span><span class="sxs-lookup"><span data-stu-id="34526-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="34526-112">In questo esempio, il &quot;`text`&quot; binding fa in modo che l'elemento `<p>` visualizzi il valore della proprietà `error` dal modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34526-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="34526-113">Ricordare che `error` è stata dichiarata come `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="34526-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="34526-114">Ogni volta che un nuovo valore viene assegnato a `error`, Knockout aggiorna il testo nell'elemento `<p>`.</span><span class="sxs-lookup"><span data-stu-id="34526-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="34526-115">Il binding `foreach` indica a knockout di scorrere il contenuto della matrice di `books`.</span><span class="sxs-lookup"><span data-stu-id="34526-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="34526-116">Per ogni elemento nella matrice, Knockout crea un nuovo elemento &lt;li&gt;.</span><span class="sxs-lookup"><span data-stu-id="34526-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="34526-117">Le associazioni all'interno del contesto del `foreach` fanno riferimento alle proprietà dell'elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="34526-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="34526-118">Esempio:</span><span class="sxs-lookup"><span data-stu-id="34526-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="34526-119">Qui l'associazione `text` legge la proprietà Author di ogni libro.</span><span class="sxs-lookup"><span data-stu-id="34526-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="34526-120">Se l'applicazione viene eseguita adesso, dovrebbe avere un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="34526-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="34526-121">L'elenco di libri viene caricato in modo asincrono dopo il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="34526-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="34526-122">In questo momento, i collegamenti&quot; &quot;dettagli non funzionano.</span><span class="sxs-lookup"><span data-stu-id="34526-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="34526-123">Questa funzionalità verrà aggiunta nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="34526-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34526-124">[Precedente](part-6.md)
> [Successivo](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="34526-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
