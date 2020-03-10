---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622346"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="ba23a-102">Creare il client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ba23a-102">Create the JavaScript Client</span></span>

<span data-ttu-id="ba23a-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ba23a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ba23a-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="ba23a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ba23a-105">In questa sezione si creerà il client per l'applicazione, usando HTML, JavaScript e la libreria [knockout. js](http://knockoutjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="ba23a-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="ba23a-106">L'app client verrà compilata in fasi:</span><span class="sxs-lookup"><span data-stu-id="ba23a-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="ba23a-107">Visualizzazione di un elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="ba23a-107">Showing a list of books.</span></span>
- <span data-ttu-id="ba23a-108">Visualizzazione di un dettaglio del libro.</span><span class="sxs-lookup"><span data-stu-id="ba23a-108">Showing a book detail.</span></span>
- <span data-ttu-id="ba23a-109">Aggiunta di un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="ba23a-109">Adding a new book.</span></span>

<span data-ttu-id="ba23a-110">La libreria Knockout usa il modello MVC (Model-View-ViewModel):</span><span class="sxs-lookup"><span data-stu-id="ba23a-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="ba23a-111">Il **modello** è la rappresentazione lato server dei dati nel dominio aziendale (in questo caso, Books and Authors).</span><span class="sxs-lookup"><span data-stu-id="ba23a-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="ba23a-112">La **visualizzazione** è il livello di presentazione (HTML).</span><span class="sxs-lookup"><span data-stu-id="ba23a-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="ba23a-113">Il **modello di visualizzazione** è un oggetto JavaScript che include i modelli.</span><span class="sxs-lookup"><span data-stu-id="ba23a-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="ba23a-114">Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ba23a-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="ba23a-115">Non conosce la rappresentazione HTML.</span><span class="sxs-lookup"><span data-stu-id="ba23a-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="ba23a-116">Rappresenta invece le funzionalità astratte della vista, ad esempio &quot;un elenco di libri&quot;.</span><span class="sxs-lookup"><span data-stu-id="ba23a-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="ba23a-117">La vista è associata a dati al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ba23a-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="ba23a-118">Gli aggiornamenti al modello di visualizzazione vengono riflessi automaticamente nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ba23a-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="ba23a-119">Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio i clic sui pulsanti.</span><span class="sxs-lookup"><span data-stu-id="ba23a-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="ba23a-120">Questo approccio semplifica la modifica del layout e dell'interfaccia utente dell'app, perché è possibile modificare i binding senza riscrivere il codice.</span><span class="sxs-lookup"><span data-stu-id="ba23a-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="ba23a-121">Ad esempio, è possibile visualizzare un elenco di elementi come `<ul>`, quindi modificarlo in un secondo momento in una tabella.</span><span class="sxs-lookup"><span data-stu-id="ba23a-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="ba23a-122">Aggiungere la libreria Knockout</span><span class="sxs-lookup"><span data-stu-id="ba23a-122">Add the Knockout Library</span></span>

<span data-ttu-id="ba23a-123">In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** .</span><span class="sxs-lookup"><span data-stu-id="ba23a-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="ba23a-124">Selezionare quindi **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="ba23a-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="ba23a-125">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ba23a-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="ba23a-126">Questo comando aggiunge i file Knockout alla cartella Scripts.</span><span class="sxs-lookup"><span data-stu-id="ba23a-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="ba23a-127">Creare il modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="ba23a-127">Create the View Model</span></span>

<span data-ttu-id="ba23a-128">Aggiungere un file JavaScript denominato app. js alla cartella Scripts.</span><span class="sxs-lookup"><span data-stu-id="ba23a-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="ba23a-129">(In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella script, scegliere **Aggiungi**, quindi selezionare **file JavaScript**). Incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ba23a-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="ba23a-130">In Knockout la classe `observable` consente l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="ba23a-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="ba23a-131">Quando il contenuto di una modifica osservabile, l'osservabile Invia notifiche a tutti i controlli con associazione a dati, in modo che possano aggiornarsi.</span><span class="sxs-lookup"><span data-stu-id="ba23a-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="ba23a-132">La classe `observableArray` è la versione della matrice di *osservabile*. Per iniziare, il modello di visualizzazione presenta due osservabili:</span><span class="sxs-lookup"><span data-stu-id="ba23a-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="ba23a-133">`books` include l'elenco dei libri.</span><span class="sxs-lookup"><span data-stu-id="ba23a-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="ba23a-134">`error` contiene un messaggio di errore in caso di esito negativo di una chiamata AJAX.</span><span class="sxs-lookup"><span data-stu-id="ba23a-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="ba23a-135">Il metodo `getAllBooks` esegue una chiamata AJAX per ottenere l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="ba23a-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="ba23a-136">Quindi inserisce il risultato nella matrice `books`.</span><span class="sxs-lookup"><span data-stu-id="ba23a-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="ba23a-137">Il `ko.applyBindings` metodo fa parte della libreria knockout.</span><span class="sxs-lookup"><span data-stu-id="ba23a-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="ba23a-138">Accetta il modello di visualizzazione come parametro e imposta il data binding.</span><span class="sxs-lookup"><span data-stu-id="ba23a-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="ba23a-139">Aggiungere un bundle di script</span><span class="sxs-lookup"><span data-stu-id="ba23a-139">Add a Script Bundle</span></span>

<span data-ttu-id="ba23a-140">La creazione di bundle è una funzionalità di ASP.NET 4,5 che semplifica la combinazione o l'aggregazione di più file in un unico file.</span><span class="sxs-lookup"><span data-stu-id="ba23a-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="ba23a-141">La creazione di bundle riduce il numero di richieste al server, che può migliorare il tempo di caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="ba23a-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="ba23a-142">Aprire l'app file\_Start/BundleConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="ba23a-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="ba23a-143">Aggiungere il codice seguente al metodo RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="ba23a-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="ba23a-144">[Precedente](part-5.md)
> [Successivo](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="ba23a-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
