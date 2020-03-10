---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: elenco di prodotti | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 4 viene illustrata la lista dei prodotti con il contr GridView...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566983"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="2132a-104">Parte 4: elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="2132a-104">Part 4: Listing Products</span></span>

<span data-ttu-id="2132a-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2132a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2132a-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="2132a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2132a-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="2132a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2132a-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="2132a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2132a-109">Nella parte 4 viene illustrata la lista dei prodotti con il controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="2132a-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="2132a-110">Elenco di prodotti con il controllo GridView</span><span class="sxs-lookup"><span data-stu-id="2132a-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="2132a-111">Iniziamo a implementare la pagina Products. aspx facendo clic con il pulsante destro del mouse sulla soluzione e scegliendo "Aggiungi" e "nuovo elemento".</span><span class="sxs-lookup"><span data-stu-id="2132a-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="2132a-112">Scegliere "Web Form utilizzando la pagina master" e immettere il nome della pagina Products. aspx ".</span><span class="sxs-lookup"><span data-stu-id="2132a-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="2132a-113">Fare clic su "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="2132a-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="2132a-114">Scegliere quindi la cartella "stili" in cui è stata posizionata la pagina Site. master e selezionarla dalla finestra "contenuto della cartella".</span><span class="sxs-lookup"><span data-stu-id="2132a-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="2132a-115">Fare clic su "OK" per creare la pagina.</span><span class="sxs-lookup"><span data-stu-id="2132a-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="2132a-116">Il database viene popolato con i dati del prodotto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2132a-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="2132a-117">Dopo aver creato la pagina, si userà di nuovo un'origine dati dell'entità per accedere ai dati del prodotto, ma in questo caso è necessario selezionare le entità del prodotto ed è necessario limitare gli elementi restituiti solo a quelli per la categoria selezionata.</span><span class="sxs-lookup"><span data-stu-id="2132a-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="2132a-118">A tale scopo, si indica a EntityDataSource di generare automaticamente la clausola WHERE e si specificherà il WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="2132a-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="2132a-119">Si noterà che, quando sono state create le voci di menu nel menu "Product Category", il collegamento è stato compilato dinamicamente aggiungendo CategoryID alla QueryString per ogni collegamento.</span><span class="sxs-lookup"><span data-stu-id="2132a-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="2132a-120">Si indica all'origine dati dell'entità di derivare il parametro WHERE dal parametro QueryString.</span><span class="sxs-lookup"><span data-stu-id="2132a-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="2132a-121">Successivamente, verrà configurato il controllo ListView per visualizzare un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="2132a-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="2132a-122">Per creare un'esperienza di acquisto ottimale, verranno comprimete diverse funzionalità concise in ogni singolo prodotto visualizzato nella ListVew.</span><span class="sxs-lookup"><span data-stu-id="2132a-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="2132a-123">Il nome del prodotto sarà un collegamento alla visualizzazione dei dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="2132a-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="2132a-124">Verrà visualizzato il prezzo del prodotto.</span><span class="sxs-lookup"><span data-stu-id="2132a-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="2132a-125">Verrà visualizzata un'immagine del prodotto che verrà selezionata dinamicamente da una directory di immagini del catalogo nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2132a-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="2132a-126">Si includerà un collegamento per aggiungere immediatamente il prodotto specifico al carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="2132a-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="2132a-127">Ecco il markup per l'istanza del controllo ListView.</span><span class="sxs-lookup"><span data-stu-id="2132a-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="2132a-128">Vengono compilati in modo dinamico diversi collegamenti per ogni prodotto visualizzato.</span><span class="sxs-lookup"><span data-stu-id="2132a-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="2132a-129">Inoltre, prima di testare la nuova pagina, è necessario creare la struttura di directory per le immagini del catalogo dei prodotti come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2132a-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="2132a-130">Una volta accessibili le immagini del prodotto, è possibile testare la pagina dell'elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="2132a-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="2132a-131">Dal home page del sito, fare clic su uno dei collegamenti elenco categorie.</span><span class="sxs-lookup"><span data-stu-id="2132a-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="2132a-132">A questo punto è necessario implementare la pagina ProductDetails. aspx e la funzionalità AddToCart.</span><span class="sxs-lookup"><span data-stu-id="2132a-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="2132a-133">Usare file-&gt;nuovo per creare un nome di pagina ProductDetails. aspx usando la pagina master del sito come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2132a-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="2132a-134">Si utilizzerà di nuovo un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si utilizzerà un controllo ASP.NET FormView per visualizzare i dati del prodotto come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2132a-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="2132a-135">Non preoccuparti se la formattazione sembra un po' divertente.</span><span class="sxs-lookup"><span data-stu-id="2132a-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="2132a-136">Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verranno implementate in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="2132a-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="2132a-137">Il carrello acquisti rappresenterà la logica più complessa nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2132a-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="2132a-138">Per iniziare, usare file-&gt;nuovo per creare una pagina denominata MyShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="2132a-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="2132a-139">Si noti che non si sta scegliendo il nome ShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="2132a-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="2132a-140">Il database contiene una tabella denominata "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="2132a-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="2132a-141">Quando è stato generato un Entity Data Model è stata creata una classe per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="2132a-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="2132a-142">Pertanto, il Entity Data Model ha generato una classe di entità denominata "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="2132a-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="2132a-143">È possibile modificare il modello in modo che sia possibile usare tale nome per l'implementazione del carrello acquisti o estenderlo in base alle esigenze, ma è necessario invece selezionare semplicemente un nome che eviterà il conflitto.</span><span class="sxs-lookup"><span data-stu-id="2132a-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="2132a-144">È anche importante notare che verrà creato un semplice carrello acquisti e verrà incorporata la logica del carrello della spesa con la visualizzazione del carrello.</span><span class="sxs-lookup"><span data-stu-id="2132a-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="2132a-145">Microsoft potrebbe anche scegliere di implementare il carrello acquisti in un livello aziendale completamente separato.</span><span class="sxs-lookup"><span data-stu-id="2132a-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2132a-146">[Precedente](tailspin-spyworks-part-3.md)
> [Successivo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="2132a-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
