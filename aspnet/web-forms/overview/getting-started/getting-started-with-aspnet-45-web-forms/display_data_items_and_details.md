---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare elementi di dati e dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641113"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="84966-103">Visualizzazione di elementi di dati e dettagli</span><span class="sxs-lookup"><span data-stu-id="84966-103">Display data items and details</span></span>

<span data-ttu-id="84966-104">di [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="84966-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="84966-105">Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="84966-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="84966-106">In questa esercitazione si apprenderà come visualizzare gli elementi di dati e i dettagli degli elementi di dati con ASP.NET Web Forms e Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="84966-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="84966-107">Questa esercitazione si basa sull'esercitazione "UI and Navigation" precedente come parte della serie di esercitazioni su Wingtip Toy Store.</span><span class="sxs-lookup"><span data-stu-id="84966-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="84966-108">Al termine dell'esercitazione, verranno visualizzati i prodotti nella pagina *Products. aspx* e i dettagli di un prodotto nella pagina *productDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="84966-109">Si imparerà a eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="84966-109">You'll learn how to:</span></span>

- <span data-ttu-id="84966-110">Aggiungere un controllo dati per visualizzare i prodotti dal database</span><span class="sxs-lookup"><span data-stu-id="84966-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="84966-111">Connetti un controllo dati ai dati selezionati</span><span class="sxs-lookup"><span data-stu-id="84966-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="84966-112">Aggiungere un controllo dati per visualizzare i dettagli del prodotto dal database</span><span class="sxs-lookup"><span data-stu-id="84966-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="84966-113">Recuperare un valore dalla stringa di query e usare tale valore per limitare i dati recuperati dal database</span><span class="sxs-lookup"><span data-stu-id="84966-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="84966-114">Funzionalità introdotte in questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="84966-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="84966-115">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="84966-115">Model binding</span></span>
- <span data-ttu-id="84966-116">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="84966-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="84966-117">Aggiungere un controllo dati</span><span class="sxs-lookup"><span data-stu-id="84966-117">Add a data control</span></span>

<span data-ttu-id="84966-118">È possibile utilizzare alcune opzioni diverse per associare i dati a un controllo server.</span><span class="sxs-lookup"><span data-stu-id="84966-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="84966-119">Tra i più comuni:</span><span class="sxs-lookup"><span data-stu-id="84966-119">The most common include:</span></span>

* <span data-ttu-id="84966-120">Aggiunta di un controllo origine dati</span><span class="sxs-lookup"><span data-stu-id="84966-120">Adding a data source control</span></span>
* <span data-ttu-id="84966-121">Aggiunta di codice a mano</span><span class="sxs-lookup"><span data-stu-id="84966-121">Adding code by hand</span></span>
* <span data-ttu-id="84966-122">Uso dell'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="84966-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="84966-123">Usare un controllo origine dati per associare i dati</span><span class="sxs-lookup"><span data-stu-id="84966-123">Use a data source control to bind data</span></span>

<span data-ttu-id="84966-124">L'aggiunta di un controllo origine dati consente di collegare il controllo origine dati al controllo che Visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="84966-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="84966-125">Con questo approccio, è possibile connettere i controlli lato server alle origini dati in modo dichiarativo, anziché a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="84966-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="84966-126">Eseguire manualmente il codice per associare i dati</span><span class="sxs-lookup"><span data-stu-id="84966-126">Code by hand to bind data</span></span>

<span data-ttu-id="84966-127">La codifica manuale prevede:</span><span class="sxs-lookup"><span data-stu-id="84966-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="84966-128">Lettura di un valore</span><span class="sxs-lookup"><span data-stu-id="84966-128">Reading a value</span></span>
2. <span data-ttu-id="84966-129">Verifica della presenza di valori null</span><span class="sxs-lookup"><span data-stu-id="84966-129">Checking if it's null</span></span>
3. <span data-ttu-id="84966-130">Conversione in un tipo appropriato</span><span class="sxs-lookup"><span data-stu-id="84966-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="84966-131">Verifica dell'esito positivo della conversione</span><span class="sxs-lookup"><span data-stu-id="84966-131">Checking conversion success</span></span>
5. <span data-ttu-id="84966-132">Utilizzo del valore nella query</span><span class="sxs-lookup"><span data-stu-id="84966-132">Using the value in the query</span></span> 

<span data-ttu-id="84966-133">Questo approccio consente di avere il controllo completo sulla logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="84966-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="84966-134">Usare l'associazione di modelli per associare i dati</span><span class="sxs-lookup"><span data-stu-id="84966-134">Use model binding to bind data</span></span>

<span data-ttu-id="84966-135">L'associazione di modelli consente di associare i risultati con una quantità di codice molto minore e offre la possibilità di riutilizzare la funzionalità nell'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="84966-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="84966-136">Semplifica l'uso della logica di accesso ai dati incentrata sul codice, offrendo al tempo stesso un Framework completo per l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="84966-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="84966-137">Visualizza prodotti</span><span class="sxs-lookup"><span data-stu-id="84966-137">Display products</span></span>

<span data-ttu-id="84966-138">In questa esercitazione si userà l'associazione di modelli per associare i dati.</span><span class="sxs-lookup"><span data-stu-id="84966-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="84966-139">Per configurare un controllo dati per usare l'associazione di modelli per selezionare i dati, impostare la proprietà `SelectMethod` del controllo su un nome di metodo nel codice della pagina.</span><span class="sxs-lookup"><span data-stu-id="84966-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="84966-140">Il controllo dei dati chiama il metodo nel momento appropriato del ciclo di vita della pagina e associa automaticamente i dati restituiti.</span><span class="sxs-lookup"><span data-stu-id="84966-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="84966-141">Non è necessario chiamare in modo esplicito il metodo `DataBind`.</span><span class="sxs-lookup"><span data-stu-id="84966-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="84966-142">In **Esplora soluzioni**aprire *Production. aspx*.</span><span class="sxs-lookup"><span data-stu-id="84966-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="84966-143">Sostituire il markup esistente con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="84966-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="84966-144">Questo codice usa un controllo **ListView** denominato `productList` per visualizzare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="84966-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="84966-145">Con i modelli e gli stili è possibile definire il modo in cui il controllo **ListView** Visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="84966-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="84966-146">È utile per i dati in qualsiasi struttura ripetuta.</span><span class="sxs-lookup"><span data-stu-id="84966-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="84966-147">Sebbene questo esempio di **ListView** visualizzi semplicemente i dati del database, è anche possibile, senza codice, consentire agli utenti di modificare, inserire ed eliminare i dati e di ordinarli e di paginarli.</span><span class="sxs-lookup"><span data-stu-id="84966-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="84966-148">Impostando la proprietà `ItemType` nel controllo **ListView** , l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="84966-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="84966-149">Come indicato nell'esercitazione precedente, è possibile selezionare Dettagli oggetto elemento con IntelliSense, ad esempio specificando la `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="84966-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Visualizzazione di elementi di dati e dettagli-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="84966-151">Si usa anche l'associazione di modelli per specificare un valore `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="84966-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="84966-152">Questo valore (`GetProducts`) corrisponde al metodo che verrà aggiunto al code-behind per visualizzare i prodotti nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="84966-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="84966-153">Aggiungere codice per visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="84966-153">Add code to display products</span></span>

<span data-ttu-id="84966-154">In questo passaggio verrà aggiunto il codice per popolare il controllo **ListView** con i dati del prodotto del database.</span><span class="sxs-lookup"><span data-stu-id="84966-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="84966-155">Il codice supporta la visualizzazione di tutti i prodotti e dei singoli prodotti di categoria.</span><span class="sxs-lookup"><span data-stu-id="84966-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="84966-156">In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *Production. aspx* , quindi scegliere **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="84966-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="84966-157">Sostituire il codice esistente nel file *ProductList.aspx.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="84966-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="84966-158">Questo codice mostra il metodo `GetProducts` a cui fa riferimento la proprietà `ItemType` del controllo **ListView** nella pagina *Production. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="84966-159">Per limitare i risultati a una categoria di database specifica, il codice imposta il valore `categoryId` dal valore della stringa di query passato alla pagina *Production. aspx* quando si passa alla pagina *Production. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="84966-160">La classe `QueryStringAttribute` nello spazio dei nomi `System.Web.ModelBinding` viene utilizzata per recuperare il valore della variabile della stringa di query `id`.</span><span class="sxs-lookup"><span data-stu-id="84966-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="84966-161">In questo modo si indica all'associazione di modelli di provare a associare un valore dalla stringa di query al parametro `categoryId` in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="84966-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="84966-162">Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati ai prodotti del database che corrispondono al valore `categoryId`.</span><span class="sxs-lookup"><span data-stu-id="84966-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="84966-163">Ad esempio, se l'URL della pagina *Products. aspx* è il seguente:</span><span class="sxs-lookup"><span data-stu-id="84966-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="84966-164">Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.</span><span class="sxs-lookup"><span data-stu-id="84966-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="84966-165">Tutti i prodotti vengono visualizzati se non viene inclusa alcuna stringa di query quando viene chiamata la pagina *Production. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="84966-166">Le origini dei valori per questi metodi sono denominate *provider* di valori (ad esempio *QueryString*) e gli attributi dei parametri che indicano il provider di valori da usare vengono definiti *attributi del provider di valori* , ad esempio `id`.</span><span class="sxs-lookup"><span data-stu-id="84966-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="84966-167">ASP.NET include i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, i cookie, i valori dei moduli, i controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo.</span><span class="sxs-lookup"><span data-stu-id="84966-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="84966-168">È anche possibile scrivere provider di valori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="84966-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="84966-169">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="84966-169">Run the application</span></span>

<span data-ttu-id="84966-170">Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti di una categoria.</span><span class="sxs-lookup"><span data-stu-id="84966-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="84966-171">Premere **F5** in Visual Studio per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="84966-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="84966-172">Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="84966-173">Selezionare **automobili** dal menu di spostamento categoria prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="84966-174">La pagina *Production. aspx* Mostra solo i prodotti di categoria **Cars** .</span><span class="sxs-lookup"><span data-stu-id="84966-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="84966-175">Più avanti in questa esercitazione verranno visualizzati i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Visualizzazione di elementi di dati e dettagli-automobili](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="84966-177">Selezionare **Products** dal menu di navigazione nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="84966-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="84966-178">Anche in questo caso, viene visualizzata la pagina *Production. aspx* , ma questa volta Mostra l'intero elenco dei prodotti.</span><span class="sxs-lookup"><span data-stu-id="84966-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Visualizzazione di elementi di dati e dettagli-prodotti](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="84966-180">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84966-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="84966-181">Aggiungere un controllo dati per visualizzare i dettagli del prodotto</span><span class="sxs-lookup"><span data-stu-id="84966-181">Add a data control to display product details</span></span>

<span data-ttu-id="84966-182">Successivamente, si modificherà il markup nella pagina *productDetails. aspx* aggiunta nell'esercitazione precedente per visualizzare informazioni specifiche sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="84966-183">In **Esplora soluzioni**aprire *productDetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="84966-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="84966-184">Sostituire il markup esistente con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="84966-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="84966-185">Questo codice usa un controllo **FormView** per visualizzare dettagli specifici del prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="84966-186">Questo markup utilizza metodi come i metodi utilizzati per visualizzare i dati nella pagina *Production. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="84966-187">Il controllo **FormView** viene utilizzato per visualizzare un singolo record alla volta da un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="84966-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="84966-188">Quando si utilizza il controllo **FormView** , è possibile creare modelli per visualizzare e modificare i valori associati a dati.</span><span class="sxs-lookup"><span data-stu-id="84966-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="84966-189">Questi modelli contengono controlli, espressioni di associazione e formattazione che definiscono l'aspetto e la funzionalità del modulo.</span><span class="sxs-lookup"><span data-stu-id="84966-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="84966-190">Per connettere il markup precedente al database è necessario codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="84966-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="84966-191">In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *productDetails. aspx* , quindi scegliere **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="84966-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="84966-192">Viene visualizzato il file *productDetails.aspx.cs* .</span><span class="sxs-lookup"><span data-stu-id="84966-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="84966-193">Sostituire il codice esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="84966-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="84966-194">Questo codice controlla la presenza di un valore della stringa di query "`productID`".</span><span class="sxs-lookup"><span data-stu-id="84966-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="84966-195">Se viene trovato un valore di stringa di query valido, viene visualizzato il prodotto corrispondente.</span><span class="sxs-lookup"><span data-stu-id="84966-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="84966-196">Se la stringa di query non viene trovata o il relativo valore non è valido, non viene visualizzato alcun prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="84966-197">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="84966-197">Run the application</span></span>

<span data-ttu-id="84966-198">È ora possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato in base all'ID del prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="84966-199">Premere **F5** in Visual Studio per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="84966-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="84966-200">Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="84966-201">Selezionare **barche** dal menu di spostamento categoria.</span><span class="sxs-lookup"><span data-stu-id="84966-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="84966-202">Verrà visualizzata la pagina *Production. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="84966-203">Selezionare **Paper Boat** dall'elenco dei prodotti.</span><span class="sxs-lookup"><span data-stu-id="84966-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="84966-204">Viene visualizzata la pagina *productDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="84966-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Visualizzazione di elementi di dati e dettagli-prodotti](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="84966-206">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="84966-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84966-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="84966-207">Additional resources</span></span>

[<span data-ttu-id="84966-208">Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form</span><span class="sxs-lookup"><span data-stu-id="84966-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="84966-209">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="84966-209">Next steps</span></span>

<span data-ttu-id="84966-210">In questa esercitazione sono stati aggiunti markup e codice per visualizzare i prodotti e i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="84966-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="84966-211">Sono stati appresi i controlli dati fortemente tipizzati, l'associazione di modelli e i provider di valori.</span><span class="sxs-lookup"><span data-stu-id="84966-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="84966-212">Nell'esercitazione successiva verrà aggiunto un carrello acquisti all'applicazione di esempio Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="84966-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="84966-213">[Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="84966-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
