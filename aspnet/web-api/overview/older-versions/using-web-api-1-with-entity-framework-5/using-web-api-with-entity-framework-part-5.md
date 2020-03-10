---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Parte 5: creazione di un'interfaccia utente dinamica con Knockout. js | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555993"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="31897-102">Parte 5: creazione di un'interfaccia utente dinamica con Knockout. js</span><span class="sxs-lookup"><span data-stu-id="31897-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="31897-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31897-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="31897-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="31897-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="31897-105">Creazione di un'interfaccia utente dinamica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="31897-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="31897-106">In questa sezione si userà knockout. js per aggiungere funzionalità alla visualizzazione amministratore.</span><span class="sxs-lookup"><span data-stu-id="31897-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="31897-107">[Knockout. js](http://knockoutjs.com/) è una libreria JavaScript che consente di associare facilmente i controlli HTML ai dati.</span><span class="sxs-lookup"><span data-stu-id="31897-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="31897-108">Knockout. js usa il modello MVC (Model-View-ViewModel).</span><span class="sxs-lookup"><span data-stu-id="31897-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="31897-109">Il *modello* è la rappresentazione lato server dei dati nel dominio aziendale (in questo caso, prodotti e ordini).</span><span class="sxs-lookup"><span data-stu-id="31897-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="31897-110">La *visualizzazione* è il livello di presentazione (HTML).</span><span class="sxs-lookup"><span data-stu-id="31897-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="31897-111">Il *modello di visualizzazione* è un oggetto JavaScript che include i dati del modello.</span><span class="sxs-lookup"><span data-stu-id="31897-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="31897-112">Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="31897-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="31897-113">Non conosce la rappresentazione HTML.</span><span class="sxs-lookup"><span data-stu-id="31897-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="31897-114">Rappresenta invece le funzionalità astratte della vista, ad esempio "un elenco di elementi".</span><span class="sxs-lookup"><span data-stu-id="31897-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="31897-115">La vista è associata a dati al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="31897-116">Gli aggiornamenti al modello di visualizzazione vengono riflessi automaticamente nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="31897-117">Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio i clic sui pulsanti, ed esegue operazioni sul modello, ad esempio la creazione di un ordine.</span><span class="sxs-lookup"><span data-stu-id="31897-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="31897-118">Prima verrà definito il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-118">First we'll define the view-model.</span></span> <span data-ttu-id="31897-119">Successivamente, il markup HTML viene associato al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="31897-120">Aggiungere la sezione Razor seguente a admin. cshtml:</span><span class="sxs-lookup"><span data-stu-id="31897-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="31897-121">È possibile aggiungere questa sezione in qualsiasi punto del file.</span><span class="sxs-lookup"><span data-stu-id="31897-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="31897-122">Quando viene eseguito il rendering della visualizzazione, la sezione viene visualizzata nella parte inferiore della pagina HTML, immediatamente prima del tag di chiusura &lt;/Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="31897-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="31897-123">Tutti gli script per questa pagina verranno inseriti nel tag script indicato dal commento:</span><span class="sxs-lookup"><span data-stu-id="31897-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="31897-124">Definire innanzitutto una classe del modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="31897-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="31897-125">**Ko. observableArray** è un tipo speciale di oggetto in Knockout, denominato *osservabile*.</span><span class="sxs-lookup"><span data-stu-id="31897-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="31897-126">Dalla [documentazione di Knockout. js](http://knockoutjs.com/documentation/observables.html): un osservabile è un "oggetto JavaScript che può notificare ai Sottoscrittori le modifiche".</span><span class="sxs-lookup"><span data-stu-id="31897-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="31897-127">Quando il contenuto di un oggetto osservabile cambia, la visualizzazione viene aggiornata automaticamente in modo che corrisponda a.</span><span class="sxs-lookup"><span data-stu-id="31897-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="31897-128">Per popolare la matrice di `products`, effettuare una richiesta AJAX all'API Web.</span><span class="sxs-lookup"><span data-stu-id="31897-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="31897-129">Si tenga presente che l'URI di base per l'API è stato archiviato nel contenitore delle visualizzazioni (vedere la [parte 4](using-web-api-with-entity-framework-part-4.md) dell'esercitazione).</span><span class="sxs-lookup"><span data-stu-id="31897-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="31897-130">Aggiungere quindi funzioni al modello di visualizzazione per creare, aggiornare ed eliminare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="31897-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="31897-131">Queste funzioni inviano chiamate AJAX all'API Web e utilizzano i risultati per aggiornare il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="31897-132">A questo punto, la parte più importante: quando il DOM è riempito, chiamare la funzione **Ko. applyBindings** e passare una nuova istanza della `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="31897-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="31897-133">Il metodo **Ko. applyBindings** attiva Knockout e collega il modello di visualizzazione alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="31897-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="31897-134">Ora che è disponibile un modello di visualizzazione, è possibile creare le associazioni.</span><span class="sxs-lookup"><span data-stu-id="31897-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="31897-135">In Knockout. js questa operazione viene eseguita aggiungendo `data-bind` attributi agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="31897-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="31897-136">Ad esempio, per associare un elenco HTML a una matrice, usare l'associazione `foreach`:</span><span class="sxs-lookup"><span data-stu-id="31897-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="31897-137">L'associazione `foreach` scorre la matrice e crea elementi figlio per ogni oggetto nella matrice.</span><span class="sxs-lookup"><span data-stu-id="31897-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="31897-138">Le associazioni negli elementi figlio possono fare riferimento alle proprietà degli oggetti matrice.</span><span class="sxs-lookup"><span data-stu-id="31897-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="31897-139">Aggiungere le associazioni seguenti all'elenco "Update-Products":</span><span class="sxs-lookup"><span data-stu-id="31897-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="31897-140">L'elemento `<li>` si verifica nell'ambito dell'associazione **foreach** .</span><span class="sxs-lookup"><span data-stu-id="31897-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="31897-141">Questo significa che il rendering dell'elemento viene eseguito una volta per ogni prodotto nell'array `products`.</span><span class="sxs-lookup"><span data-stu-id="31897-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="31897-142">Tutte le associazioni all'interno dell'elemento `<li>` fanno riferimento a tale istanza del prodotto.</span><span class="sxs-lookup"><span data-stu-id="31897-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="31897-143">`$data.Name`, ad esempio, fa riferimento alla proprietà `Name` sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="31897-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="31897-144">Per impostare i valori degli input di testo, usare l'associazione `value`.</span><span class="sxs-lookup"><span data-stu-id="31897-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="31897-145">I pulsanti sono associati alle funzioni nella visualizzazione modello, usando il binding `click`.</span><span class="sxs-lookup"><span data-stu-id="31897-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="31897-146">L'istanza del prodotto viene passata come parametro a ogni funzione.</span><span class="sxs-lookup"><span data-stu-id="31897-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="31897-147">Per ulteriori informazioni, la [documentazione di Knockout. js](http://knockoutjs.com/documentation/observables.html) include una descrizione corretta delle varie associazioni.</span><span class="sxs-lookup"><span data-stu-id="31897-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="31897-148">Successivamente, aggiungere un binding per l'evento **Submit** nel modulo Add Product:</span><span class="sxs-lookup"><span data-stu-id="31897-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="31897-149">Questa associazione chiama la funzione `create` sul modello di visualizzazione per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="31897-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="31897-150">Ecco il codice completo per la visualizzazione amministratore:</span><span class="sxs-lookup"><span data-stu-id="31897-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="31897-151">Eseguire l'applicazione, accedere con l'account amministratore e fare clic sul collegamento "admin".</span><span class="sxs-lookup"><span data-stu-id="31897-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="31897-152">Verrà visualizzato l'elenco dei prodotti e sarà possibile creare, aggiornare o eliminare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="31897-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="31897-153">[Precedente](using-web-api-with-entity-framework-part-4.md)
> [Successivo](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="31897-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
