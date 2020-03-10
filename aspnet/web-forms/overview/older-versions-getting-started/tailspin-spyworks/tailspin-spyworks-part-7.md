---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: aggiunta di funzionalità | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 7 aggiunge funzionalità aggiuntive, ad esempio Revie account...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641995"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="260cc-104">Parte 7: aggiunta di funzionalità</span><span class="sxs-lookup"><span data-stu-id="260cc-104">Part 7: Adding Features</span></span>

<span data-ttu-id="260cc-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="260cc-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="260cc-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="260cc-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="260cc-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="260cc-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="260cc-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="260cc-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="260cc-109">La parte 7 aggiunge funzionalità aggiuntive, ad esempio la revisione degli account, i revisioni del prodotto e i controlli utente "più diffusi" e "acquistati".</span><span class="sxs-lookup"><span data-stu-id="260cc-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="260cc-110">Aggiunta di funzionalità</span><span class="sxs-lookup"><span data-stu-id="260cc-110">Adding Features</span></span>

<span data-ttu-id="260cc-111">Sebbene gli utenti possano esplorare il catalogo, inserire gli elementi nel carrello della spesa e completare il processo di checkout, sono disponibili diverse funzionalità di supporto che verranno incluse per migliorare il sito.</span><span class="sxs-lookup"><span data-stu-id="260cc-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="260cc-112">Revisione dell'account (elencare gli ordini inseriti e visualizzare i dettagli)</span><span class="sxs-lookup"><span data-stu-id="260cc-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="260cc-113">Aggiungere un contenuto specifico del contesto alla pagina iniziale.</span><span class="sxs-lookup"><span data-stu-id="260cc-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="260cc-114">Aggiungere una funzionalità per consentire agli utenti di esaminare i prodotti nel catalogo.</span><span class="sxs-lookup"><span data-stu-id="260cc-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="260cc-115">Creare un controllo utente per visualizzare gli elementi più diffusi e inserire il controllo nella pagina anteriore.</span><span class="sxs-lookup"><span data-stu-id="260cc-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="260cc-116">Creare un controllo utente "acquistato anche" e aggiungerlo alla pagina dei dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="260cc-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="260cc-117">Aggiungere una pagina di contatto.</span><span class="sxs-lookup"><span data-stu-id="260cc-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="260cc-118">Aggiungere una pagina informazioni su.</span><span class="sxs-lookup"><span data-stu-id="260cc-118">Add an About Page.</span></span>
8. <span data-ttu-id="260cc-119">Errore globale</span><span class="sxs-lookup"><span data-stu-id="260cc-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="260cc-120">Revisione account</span><span class="sxs-lookup"><span data-stu-id="260cc-120">Account Review</span></span>

<span data-ttu-id="260cc-121">Nella cartella "account" creare due pagine. aspx una denominata Order. aspx e l'altra denominata OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="260cc-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="260cc-122">Orders. aspx utilizzerà i controlli GridView e EntityDataSource in modo molto simile a quanto già in precedenza.</span><span class="sxs-lookup"><span data-stu-id="260cc-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="260cc-123">EntityDataSource seleziona i record della tabella Orders filtrata in base al nome utente (vedere il WhereParameter) impostato in una variabile di sessione quando il registro utente è in.</span><span class="sxs-lookup"><span data-stu-id="260cc-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="260cc-124">Si noti anche questi parametri in HyperlinkField di GridView:</span><span class="sxs-lookup"><span data-stu-id="260cc-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="260cc-125">Questi specificano il collegamento alla visualizzazione dei dettagli degli ordini per ogni prodotto che specifica il campo OrderID come parametro QueryString nella pagina OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="260cc-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="260cc-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="260cc-126">OrderDetails.aspx</span></span>

<span data-ttu-id="260cc-127">Si utilizzerà un controllo EntityDataSource per accedere agli ordini e a un controllo FormView per visualizzare i dati degli ordini e un altro oggetto EntityDataSource con GridView per visualizzare tutte le voci dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="260cc-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="260cc-128">Nel file code-behind (OrderDetails.aspx.cs) sono disponibili due piccoli bit di manutenzione.</span><span class="sxs-lookup"><span data-stu-id="260cc-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="260cc-129">Prima di tutto è necessario assicurarsi che OrderDetails ottenga sempre un OrderId.</span><span class="sxs-lookup"><span data-stu-id="260cc-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="260cc-130">È anche necessario calcolare e visualizzare il totale degli ordini dalle voci.</span><span class="sxs-lookup"><span data-stu-id="260cc-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="260cc-131">Pagina iniziale</span><span class="sxs-lookup"><span data-stu-id="260cc-131">The Home Page</span></span>

<span data-ttu-id="260cc-132">Si aggiungerà un contenuto statico alla pagina default. aspx.</span><span class="sxs-lookup"><span data-stu-id="260cc-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="260cc-133">Innanzitutto, creo una cartella "Content" (contenuto) e all'interno di essa una cartella images (e includo un'immagine da usare nel home page).</span><span class="sxs-lookup"><span data-stu-id="260cc-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="260cc-134">Nel segnaposto inferiore della pagina default. aspx aggiungere il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="260cc-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="260cc-135">Revisioni del prodotto</span><span class="sxs-lookup"><span data-stu-id="260cc-135">Product Reviews</span></span>

<span data-ttu-id="260cc-136">Prima di tutto verrà aggiunto un pulsante con un collegamento a un modulo che è possibile usare per immettere una revisione del prodotto.</span><span class="sxs-lookup"><span data-stu-id="260cc-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="260cc-137">Si noti che il ProductID viene passato nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="260cc-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="260cc-138">Quindi, aggiungiamo la pagina denominata ReviewAdd. aspx</span><span class="sxs-lookup"><span data-stu-id="260cc-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="260cc-139">In questa pagina verrà usato ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="260cc-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="260cc-140">Se non è già stato fatto, è possibile scaricarlo da [DevExpress](http://devexpress.com/act) e sono disponibili informazioni aggiuntive sulla configurazione del Toolkit per l'uso con Visual Studio qui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="260cc-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="260cc-141">In modalità progettazione trascinare i controlli e i validator dalla casella degli strumenti e compilare un form come quello riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="260cc-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="260cc-142">Il markup sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="260cc-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="260cc-143">Ora che è possibile immettere le revisioni, visualizzare le revisioni nella pagina del prodotto.</span><span class="sxs-lookup"><span data-stu-id="260cc-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="260cc-144">Aggiungere questo markup alla pagina ProductDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="260cc-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="260cc-145">Eseguendo ora l'applicazione e passando a un prodotto vengono visualizzate le informazioni sul prodotto, incluse le verifiche dei clienti.</span><span class="sxs-lookup"><span data-stu-id="260cc-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="260cc-146">Controllo elementi più diffusi (creazione di controlli utente)</span><span class="sxs-lookup"><span data-stu-id="260cc-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="260cc-147">Per aumentare le vendite nel sito Web, si aggiungeranno alcune funzionalità ai prodotti più diffusi o correlati.</span><span class="sxs-lookup"><span data-stu-id="260cc-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="260cc-148">Il primo di queste funzionalità sarà un elenco dei prodotti più diffusi nel catalogo prodotti.</span><span class="sxs-lookup"><span data-stu-id="260cc-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="260cc-149">Viene creato un "controllo utente" per visualizzare gli articoli più venduti nella home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="260cc-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="260cc-150">Poiché si tratta di un controllo, è possibile usarlo in qualsiasi pagina semplicemente trascinando il controllo nella finestra di progettazione di Visual Studio su qualsiasi pagina desiderata.</span><span class="sxs-lookup"><span data-stu-id="260cc-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="260cc-151">In Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sul nome della soluzione e creare una nuova directory denominata "controlli".</span><span class="sxs-lookup"><span data-stu-id="260cc-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="260cc-152">Sebbene non sia necessario, il progetto verrà organizzato creando tutti i controlli utente nella directory "Controls".</span><span class="sxs-lookup"><span data-stu-id="260cc-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="260cc-153">Fare clic con il pulsante destro del mouse sulla cartella controlli e scegliere "nuovo elemento":</span><span class="sxs-lookup"><span data-stu-id="260cc-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="260cc-154">Specificare un nome per il controllo "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="260cc-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="260cc-155">Si noti che l'estensione di file per i controlli utente è. ascx not. aspx.</span><span class="sxs-lookup"><span data-stu-id="260cc-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="260cc-156">Il controllo utente degli elementi più diffusi verrà definito come segue.</span><span class="sxs-lookup"><span data-stu-id="260cc-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="260cc-157">Qui viene usato un metodo non ancora usato in questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="260cc-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="260cc-158">Si sta usando il controllo Repeater e invece di usare un controllo origine dati il controllo Repeater viene associato ai risultati di una query LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="260cc-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="260cc-159">Nel code-behind del nostro controllo lo facciamo come segue.</span><span class="sxs-lookup"><span data-stu-id="260cc-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="260cc-160">Si noti anche questa linea importante nella parte superiore del markup del controllo.</span><span class="sxs-lookup"><span data-stu-id="260cc-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="260cc-161">Poiché gli elementi più diffusi non verranno modificati in base ai minuti, è possibile aggiungere una direttiva che consente di migliorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="260cc-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="260cc-162">Questa direttiva farà sì che il codice dei controlli venga eseguito solo quando l'output memorizzato nella cache del controllo scade.</span><span class="sxs-lookup"><span data-stu-id="260cc-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="260cc-163">In caso contrario, verrà utilizzata la versione memorizzata nella cache dell'output del controllo.</span><span class="sxs-lookup"><span data-stu-id="260cc-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="260cc-164">A questo punto, è necessario includere il nuovo controllo nella pagina default. aspx.</span><span class="sxs-lookup"><span data-stu-id="260cc-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="260cc-165">Usare il trascinamento della selezione per inserire un'istanza del controllo nella colonna aperta del modulo predefinito.</span><span class="sxs-lookup"><span data-stu-id="260cc-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="260cc-166">A questo punto, quando si esegue l'applicazione, il home page Visualizza gli elementi più diffusi.</span><span class="sxs-lookup"><span data-stu-id="260cc-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="260cc-167">Controllo "also purchased" (controlli utente con parametri)</span><span class="sxs-lookup"><span data-stu-id="260cc-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="260cc-168">Il secondo controllo utente che verrà creato apporterà una vendita indicativa al livello successivo aggiungendo la specificità del contesto.</span><span class="sxs-lookup"><span data-stu-id="260cc-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="260cc-169">La logica per calcolare i primi elementi "acquistati anche" è non semplice.</span><span class="sxs-lookup"><span data-stu-id="260cc-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="260cc-170">Il controllo "anche acquistato" Seleziona i record OrderDetails (precedentemente acquistati) per il ProductID attualmente selezionato e acquisisce gli OrderID per ogni ordine univoco rilevato.</span><span class="sxs-lookup"><span data-stu-id="260cc-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="260cc-171">Verranno quindi selezionati i prodotti di tutti gli ordini e verranno sommate le quantità acquistate.</span><span class="sxs-lookup"><span data-stu-id="260cc-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="260cc-172">Verranno ordinati i prodotti in base a tale quantità somma e verranno visualizzati i primi cinque elementi.</span><span class="sxs-lookup"><span data-stu-id="260cc-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="260cc-173">Data la complessità di questa logica, questo algoritmo verrà implementato come stored procedure.</span><span class="sxs-lookup"><span data-stu-id="260cc-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="260cc-174">Il T-SQL per il stored procedure è il seguente.</span><span class="sxs-lookup"><span data-stu-id="260cc-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="260cc-175">Si noti che questo stored procedure (SelectPurchasedWithProducts) era presente nel database quando è stato incluso nell'applicazione e quando è stato generato il Entity Data Model è stato specificato che, oltre alle tabelle e alle viste necessarie, il Entity Data Model deve includere questa stored procedure.</span><span class="sxs-lookup"><span data-stu-id="260cc-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="260cc-176">Per accedere al stored procedure dalla Entity Data Model è necessario importare la funzione.</span><span class="sxs-lookup"><span data-stu-id="260cc-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="260cc-177">Fare doppio clic sul Entity Data Model in Esplora soluzioni per aprirlo nella finestra di progettazione e aprire il browser modello, quindi fare clic con il pulsante destro del mouse nella finestra di progettazione e scegliere "Aggiungi importazione funzioni".</span><span class="sxs-lookup"><span data-stu-id="260cc-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="260cc-178">In questo modo verrà aperta questa finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="260cc-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="260cc-179">Compilare i campi come indicato sopra, selezionando "SelectPurchasedWithProducts" e usare il nome della procedura per il nome della funzione importata.</span><span class="sxs-lookup"><span data-stu-id="260cc-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="260cc-180">Fare clic su "OK".</span><span class="sxs-lookup"><span data-stu-id="260cc-180">Click "Ok".</span></span>

<span data-ttu-id="260cc-181">A questo punto, è possibile programmare semplicemente sul stored procedure come qualsiasi altro elemento nel modello.</span><span class="sxs-lookup"><span data-stu-id="260cc-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="260cc-182">Quindi, nella cartella "Controls" creare un nuovo controllo utente denominato AlsoPurchased. ascx.</span><span class="sxs-lookup"><span data-stu-id="260cc-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="260cc-183">Il markup per questo controllo riguarderà molto familiare al controllo PopularItems.</span><span class="sxs-lookup"><span data-stu-id="260cc-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="260cc-184">La differenza principale consiste nel fatto che non memorizza nella cache l'output, perché il rendering dell'elemento sarà diverso per il prodotto.</span><span class="sxs-lookup"><span data-stu-id="260cc-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="260cc-185">ProductId sarà una "proprietà" del controllo.</span><span class="sxs-lookup"><span data-stu-id="260cc-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="260cc-186">Nel gestore dell'evento PreRender del controllo, il fondo è a tre fattori.</span><span class="sxs-lookup"><span data-stu-id="260cc-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="260cc-187">Verificare che ProductID sia impostato.</span><span class="sxs-lookup"><span data-stu-id="260cc-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="260cc-188">Verificare se sono presenti prodotti acquistati con quello corrente.</span><span class="sxs-lookup"><span data-stu-id="260cc-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="260cc-189">Restituire alcuni elementi come determinato in #2.</span><span class="sxs-lookup"><span data-stu-id="260cc-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="260cc-190">Si noti quanto sia semplice chiamare il stored procedure tramite il modello.</span><span class="sxs-lookup"><span data-stu-id="260cc-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="260cc-191">Una volta determinato che è stato acquistato anche il metodo "purchased", è sufficiente associare il ripetitore ai risultati restituiti dalla query.</span><span class="sxs-lookup"><span data-stu-id="260cc-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="260cc-192">Se non sono presenti elementi "acquistati anche", verranno semplicemente visualizzati altri elementi comuni del catalogo.</span><span class="sxs-lookup"><span data-stu-id="260cc-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="260cc-193">Per visualizzare gli elementi "acquistati anche", aprire la pagina ProductDetails. aspx e trascinare il controllo AlsoPurchased da Esplora soluzioni in modo che venga visualizzato in questa posizione nel markup.</span><span class="sxs-lookup"><span data-stu-id="260cc-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="260cc-194">In questo modo viene creato un riferimento al controllo nella parte superiore della pagina ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="260cc-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="260cc-195">Poiché il controllo utente AlsoPurchased richiede un numero ProductId, si imposterà la proprietà ProductID del controllo utilizzando un'istruzione eval sull'elemento del modello di dati corrente della pagina.</span><span class="sxs-lookup"><span data-stu-id="260cc-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="260cc-196">Quando si compila e si esegue ora e si passa a un prodotto, vengono visualizzati gli elementi "acquistati anche".</span><span class="sxs-lookup"><span data-stu-id="260cc-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="260cc-197">[Precedente](tailspin-spyworks-part-6.md)
> [Successivo](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="260cc-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
