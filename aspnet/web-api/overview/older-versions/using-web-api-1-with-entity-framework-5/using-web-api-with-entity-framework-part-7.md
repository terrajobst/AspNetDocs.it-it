---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: creazione della pagina principale | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599981"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="e126b-102">Parte 7: creazione della pagina principale</span><span class="sxs-lookup"><span data-stu-id="e126b-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="e126b-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e126b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e126b-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="e126b-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="e126b-105">Creazione della pagina principale</span><span class="sxs-lookup"><span data-stu-id="e126b-105">Creating the Main Page</span></span>

<span data-ttu-id="e126b-106">In questa sezione verrà creata la pagina principale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e126b-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="e126b-107">Questa pagina sarà più complessa della pagina di amministrazione, quindi verrà affrontata in diversi passaggi.</span><span class="sxs-lookup"><span data-stu-id="e126b-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="e126b-108">Verranno visualizzate alcune tecniche più avanzate di Knockout. js.</span><span class="sxs-lookup"><span data-stu-id="e126b-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="e126b-109">Ecco il layout di base della pagina:</span><span class="sxs-lookup"><span data-stu-id="e126b-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="e126b-110">"Products" include una matrice di prodotti.</span><span class="sxs-lookup"><span data-stu-id="e126b-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="e126b-111">"Cart" include una matrice di prodotti con quantità.</span><span class="sxs-lookup"><span data-stu-id="e126b-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="e126b-112">Se si fa clic su "Aggiungi al carrello", il carrello viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="e126b-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="e126b-113">"Orders" include una matrice di ID degli ordini.</span><span class="sxs-lookup"><span data-stu-id="e126b-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="e126b-114">"Details" include un dettaglio dell'ordine, ovvero una matrice di elementi (prodotti con quantità)</span><span class="sxs-lookup"><span data-stu-id="e126b-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="e126b-115">Si inizierà definendo un layout di base in HTML, senza data binding o script.</span><span class="sxs-lookup"><span data-stu-id="e126b-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="e126b-116">Aprire il file views/Home/index. cshtml e sostituire tutto il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e126b-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="e126b-117">Successivamente, aggiungere una sezione script e creare un modello di visualizzazione vuoto:</span><span class="sxs-lookup"><span data-stu-id="e126b-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="e126b-118">In base alla progettazione disegnata in precedenza, il modello di visualizzazione necessita di osservabili per prodotti, carrello, ordini e dettagli.</span><span class="sxs-lookup"><span data-stu-id="e126b-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="e126b-119">Aggiungere le variabili seguenti all'oggetto `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="e126b-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="e126b-120">Gli utenti possono aggiungere elementi dall'elenco dei prodotti al carrello e rimuovere gli elementi dal carrello.</span><span class="sxs-lookup"><span data-stu-id="e126b-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="e126b-121">Per incapsulare queste funzioni, verrà creata un'altra classe del modello di visualizzazione che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="e126b-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="e126b-122">Aggiungi il seguente codice a `AppViewModel`.</span><span class="sxs-lookup"><span data-stu-id="e126b-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="e126b-123">La classe `ProductViewModel` contiene due funzioni utilizzate per spostare il prodotto da e verso il carrello: `addItemToCart` aggiunge un'unità del prodotto al carrello e `removeAllFromCart` rimuove tutte le quantità del prodotto.</span><span class="sxs-lookup"><span data-stu-id="e126b-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="e126b-124">Gli utenti possono selezionare un ordine esistente e ottenere i dettagli dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="e126b-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="e126b-125">Questa funzionalità verrà incapsulata in un altro modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="e126b-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="e126b-126">Il `OrderDetailsViewModel` viene inizializzato con un ordine e recupera i dettagli dell'ordine inviando una richiesta AJAX al server.</span><span class="sxs-lookup"><span data-stu-id="e126b-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="e126b-127">Si noti inoltre la proprietà `total` nel `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="e126b-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="e126b-128">Questa proprietà è un tipo speciale di osservabile definito [osservabile calcolato](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="e126b-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="e126b-129">Come suggerisce il nome, un oggetto osservabile calcolato consente di eseguire il binding dei dati a&#8212;un valore calcolato, in questo caso, il costo totale dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="e126b-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="e126b-130">Aggiungere quindi queste funzioni a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="e126b-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="e126b-131">`resetCart` rimuove tutti gli elementi dal carrello.</span><span class="sxs-lookup"><span data-stu-id="e126b-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="e126b-132">`getDetails` ottiene i dettagli per un ordine (eseguendo il push di un nuovo `OrderDetailsViewModel` nell'elenco `details`).</span><span class="sxs-lookup"><span data-stu-id="e126b-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="e126b-133">`createOrder` crea un nuovo ordine e svuota il carrello.</span><span class="sxs-lookup"><span data-stu-id="e126b-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="e126b-134">Infine, inizializzare il modello di visualizzazione eseguendo richieste AJAX per i prodotti e gli ordini:</span><span class="sxs-lookup"><span data-stu-id="e126b-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="e126b-135">OK, si tratta di una grande quantità di codice, ma abbiamo creato una procedura dettagliata, quindi speriamo che la progettazione sia chiara.</span><span class="sxs-lookup"><span data-stu-id="e126b-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="e126b-136">A questo punto è possibile aggiungere alcune associazioni knockout. js al codice HTML.</span><span class="sxs-lookup"><span data-stu-id="e126b-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="e126b-137">**Prodotti**</span><span class="sxs-lookup"><span data-stu-id="e126b-137">**Products**</span></span>

<span data-ttu-id="e126b-138">Ecco le associazioni per l'elenco di prodotti:</span><span class="sxs-lookup"><span data-stu-id="e126b-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="e126b-139">Viene eseguita l'iterazione della matrice Products e vengono visualizzati il nome e il prezzo.</span><span class="sxs-lookup"><span data-stu-id="e126b-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="e126b-140">Il pulsante "Aggiungi a ordine" è visibile solo quando l'utente è connesso.</span><span class="sxs-lookup"><span data-stu-id="e126b-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="e126b-141">Il pulsante "Aggiungi all'ordine" chiama `addItemToCart` nell'istanza `ProductViewModel` per il prodotto.</span><span class="sxs-lookup"><span data-stu-id="e126b-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="e126b-142">Viene illustrata una funzionalità interessante di Knockout. js: quando un modello di visualizzazione contiene altri modelli di visualizzazione, è possibile applicare le associazioni al modello interno.</span><span class="sxs-lookup"><span data-stu-id="e126b-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="e126b-143">In questo esempio, le associazioni all'interno del `foreach` vengono applicate a ognuna delle istanze di `ProductViewModel`.</span><span class="sxs-lookup"><span data-stu-id="e126b-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="e126b-144">Questo approccio è molto più pulito rispetto all'inserimento di tutte le funzionalità in un unico modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e126b-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="e126b-145">**Carrello**</span><span class="sxs-lookup"><span data-stu-id="e126b-145">**Cart**</span></span>

<span data-ttu-id="e126b-146">Ecco i binding per il carrello:</span><span class="sxs-lookup"><span data-stu-id="e126b-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="e126b-147">Viene eseguita l'iterazione sull'array del carrello e vengono visualizzati il nome, il prezzo e la quantità.</span><span class="sxs-lookup"><span data-stu-id="e126b-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="e126b-148">Si noti che il collegamento "Rimuovi" e il pulsante "Crea ordine" sono associati alle funzioni di visualizzazione del modello.</span><span class="sxs-lookup"><span data-stu-id="e126b-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="e126b-149">**Ordini**</span><span class="sxs-lookup"><span data-stu-id="e126b-149">**Orders**</span></span>

<span data-ttu-id="e126b-150">Ecco i binding per l'elenco ordini:</span><span class="sxs-lookup"><span data-stu-id="e126b-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="e126b-151">Viene eseguita l'iterazione degli ordini e viene visualizzato l'ID dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="e126b-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="e126b-152">L'evento click sul collegamento è associato alla funzione `getDetails`.</span><span class="sxs-lookup"><span data-stu-id="e126b-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="e126b-153">**Dettagli ordine**</span><span class="sxs-lookup"><span data-stu-id="e126b-153">**Order Details**</span></span>

<span data-ttu-id="e126b-154">Ecco le associazioni per i dettagli dell'ordine:</span><span class="sxs-lookup"><span data-stu-id="e126b-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="e126b-155">Scorre gli elementi nell'ordine e visualizza il prodotto, il prezzo e la quantità.</span><span class="sxs-lookup"><span data-stu-id="e126b-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="e126b-156">Il div circostante è visibile solo se la matrice di dettagli contiene uno o più elementi.</span><span class="sxs-lookup"><span data-stu-id="e126b-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e126b-157">Conclusione</span><span class="sxs-lookup"><span data-stu-id="e126b-157">Conclusion</span></span>

<span data-ttu-id="e126b-158">In questa esercitazione è stata creata un'applicazione che usa Entity Framework per comunicare con il database e API Web ASP.NET per fornire un'interfaccia pubblica sul livello dati.</span><span class="sxs-lookup"><span data-stu-id="e126b-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="e126b-159">ASP.NET MVC 4 viene usato per eseguire il rendering delle pagine HTML ed knockout. js più jQuery per fornire interazioni dinamiche senza ricaricamenti di pagina.</span><span class="sxs-lookup"><span data-stu-id="e126b-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="e126b-160">Risorse aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="e126b-160">Additional resources:</span></span>

- [<span data-ttu-id="e126b-161">Mappa del contenuto di accesso ai dati di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e126b-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="e126b-162">Centro per sviluppatori Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e126b-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="e126b-163">Precedente</span><span class="sxs-lookup"><span data-stu-id="e126b-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
