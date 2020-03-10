---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: aggiornamenti finali per la navigazione e la progettazione del sito, conclusione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 10 illustra gli aggiornamenti finali per la navigazione e...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539368"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="c603a-104">Parte 10: aggiornamenti finali di esplorazione e progettazione del sito, conclusione</span><span class="sxs-lookup"><span data-stu-id="c603a-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="c603a-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c603a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c603a-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="c603a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c603a-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="c603a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c603a-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="c603a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c603a-109">La parte 10 illustra gli aggiornamenti finali per la navigazione e la progettazione del sito, conclusione.</span><span class="sxs-lookup"><span data-stu-id="c603a-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="c603a-110">Sono state completate tutte le funzionalità principali per il sito, ma sono ancora disponibili alcune funzionalità da aggiungere alla navigazione nel sito, alla home page e alla pagina di esplorazione del negozio.</span><span class="sxs-lookup"><span data-stu-id="c603a-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="c603a-111">Creazione della visualizzazione parziale Riepilogo del carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="c603a-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="c603a-112">Si vuole esporre il numero di elementi nel carrello acquisti dell'utente nell'intero sito.</span><span class="sxs-lookup"><span data-stu-id="c603a-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="c603a-113">Questa operazione può essere implementata facilmente creando una visualizzazione parziale aggiunta al sito. master.</span><span class="sxs-lookup"><span data-stu-id="c603a-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="c603a-114">Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="c603a-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="c603a-115">Per creare la visualizzazione parziale CartSummary, fare clic con il pulsante destro del mouse sulla cartella Views/ShoppingCart e scegliere Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c603a-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="c603a-116">Assegnare alla visualizzazione il nome CartSummary e selezionare la casella di controllo "crea una visualizzazione parziale" come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c603a-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="c603a-117">La visualizzazione parziale CartSummary è molto semplice. si tratta semplicemente di un collegamento alla visualizzazione dell'indice ShoppingCart che mostra il numero di elementi presenti nel carrello.</span><span class="sxs-lookup"><span data-stu-id="c603a-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="c603a-118">Il codice completo per CartSummary. cshtml è il seguente:</span><span class="sxs-lookup"><span data-stu-id="c603a-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="c603a-119">È possibile includere una visualizzazione parziale in qualsiasi pagina del sito, incluso il master del sito, usando il metodo HTML. RenderAction.</span><span class="sxs-lookup"><span data-stu-id="c603a-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="c603a-120">Per RenderAction è necessario specificare il nome dell'azione ("CartSummary") e il nome del controller ("ShoppingCart") come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c603a-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="c603a-121">Prima di aggiungerlo al layout del sito, verrà creato anche il menu genre, in modo da poter eseguire tutti gli aggiornamenti del sito. Master in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="c603a-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="c603a-122">Creazione della visualizzazione parziale del menu genre</span><span class="sxs-lookup"><span data-stu-id="c603a-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="c603a-123">Possiamo semplificare molto più facilmente la navigazione degli utenti nell'archivio aggiungendo un menu di genere che elenca tutti i generi disponibili nello Store.</span><span class="sxs-lookup"><span data-stu-id="c603a-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="c603a-124">Si seguiranno anche gli stessi passaggi per creare una visualizzazione parziale GenreMenu, che sarà quindi possibile aggiungere al master del sito.</span><span class="sxs-lookup"><span data-stu-id="c603a-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="c603a-125">Aggiungere prima di tutto la seguente azione del controller GenreMenu a StoreController:</span><span class="sxs-lookup"><span data-stu-id="c603a-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="c603a-126">Questa azione restituisce un elenco di generi che verranno visualizzati dalla visualizzazione parziale, che verrà creata successivamente.</span><span class="sxs-lookup"><span data-stu-id="c603a-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="c603a-127">*Nota: è stato aggiunto l'attributo [ChildActionOnly] a questa azione del controller, che indica che si vuole usare questa azione solo da una visualizzazione parziale. Questo attributo impedisce l'esecuzione dell'azione del controller passando a/Store/GenreMenu. Questa operazione non è necessaria per le visualizzazioni parziali, ma è una procedura consigliata, poiché si vuole assicurarsi che le azioni del controller vengano usate come previsto. Si sta anche restituendo PartialView anziché View, che consente al motore di visualizzazione di tenere presente che non deve usare il layout per questa visualizzazione, perché è incluso in altre visualizzazioni.*</span><span class="sxs-lookup"><span data-stu-id="c603a-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="c603a-128">Fare clic con il pulsante destro del mouse sull'azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu che è fortemente tipizzata usando la classe di dati di genere View, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c603a-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="c603a-129">Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu per visualizzare gli elementi usando un elenco non ordinato, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c603a-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="c603a-130">Aggiornamento del layout del sito per visualizzare le visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="c603a-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="c603a-131">È possibile aggiungere le visualizzazioni parziali al layout del sito (/Views/Shared/\_layout. cshtml) chiamando HTML. RenderAction ().</span><span class="sxs-lookup"><span data-stu-id="c603a-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="c603a-132">Verranno aggiunti entrambi in, oltre ad alcuni markup aggiuntivi per visualizzarli, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c603a-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="c603a-133">A questo punto, quando si esegue l'applicazione, il genere verrà visualizzato nell'area di spostamento a sinistra e nel riepilogo del carrello nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="c603a-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="c603a-134">Aggiornamento alla pagina di esplorazione del negozio</span><span class="sxs-lookup"><span data-stu-id="c603a-134">Update to the Store Browse page</span></span>

<span data-ttu-id="c603a-135">La pagina di esplorazione del negozio è funzionale, ma non ha un aspetto molto valido.</span><span class="sxs-lookup"><span data-stu-id="c603a-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="c603a-136">È possibile aggiornare la pagina per visualizzare gli album in un layout migliore aggiornando il codice di visualizzazione (disponibile in/Views/Store/Browse.cshtml) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c603a-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="c603a-137">Qui viene usato l'URL. Action anziché HTML. ActionLink in modo che sia possibile applicare una formattazione speciale al collegamento per includere la grafica dell'album.</span><span class="sxs-lookup"><span data-stu-id="c603a-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="c603a-138">*Nota: per questi album viene visualizzata una copertura di album generica. Queste informazioni vengono archiviate nel database di e sono modificabili tramite Gestione archivi. È possibile aggiungere un'immagine personalizzata.*</span><span class="sxs-lookup"><span data-stu-id="c603a-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="c603a-139">A questo punto, quando si passa a un genere, verranno visualizzati gli album mostrati in una griglia con la grafica dell'album.</span><span class="sxs-lookup"><span data-stu-id="c603a-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="c603a-140">Aggiornamento della Home page per visualizzare gli album più venduti</span><span class="sxs-lookup"><span data-stu-id="c603a-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="c603a-141">Microsoft vuole avere a che fare con i nostri album più venduti sul home page per aumentare le vendite.</span><span class="sxs-lookup"><span data-stu-id="c603a-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="c603a-142">Verranno apportati alcuni aggiornamenti ai HomeController per gestirli e aggiungere anche altri elementi grafici.</span><span class="sxs-lookup"><span data-stu-id="c603a-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="c603a-143">Prima di tutto, verrà aggiunta una proprietà di navigazione alla classe album in modo che EntityFramework sappia che sono associati.</span><span class="sxs-lookup"><span data-stu-id="c603a-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="c603a-144">Le ultime righe della classe **album** dovrebbero ora essere simili alle seguenti:</span><span class="sxs-lookup"><span data-stu-id="c603a-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="c603a-145">*Nota: è necessario aggiungere un'istruzione using da inserire nello spazio dei nomi System. Collections. Generic.*</span><span class="sxs-lookup"><span data-stu-id="c603a-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="c603a-146">In primo luogo, verranno aggiunti un campo storeDB e MvcMusicStore. Models usando le istruzioni, come negli altri controller.</span><span class="sxs-lookup"><span data-stu-id="c603a-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="c603a-147">Successivamente, verrà aggiunto il metodo seguente a HomeController che esegue una query sul database per trovare gli album più venduti in base a OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="c603a-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="c603a-148">Si tratta di un metodo privato, perché non si vuole renderlo disponibile come azione del controller.</span><span class="sxs-lookup"><span data-stu-id="c603a-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="c603a-149">Per semplicità, viene incluso nella HomeController, ma è consigliabile spostare la logica di business in classi di servizi separate, a seconda dei casi.</span><span class="sxs-lookup"><span data-stu-id="c603a-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="c603a-150">A questo punto, è possibile aggiornare l'azione del controller di indice per eseguire una query sui primi 5 album di vendita e restituirli alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c603a-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="c603a-151">Il codice completo per il HomeController aggiornato è come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c603a-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="c603a-152">Infine, sarà necessario aggiornare la visualizzazione dell'indice Home in modo che sia possibile visualizzare un elenco di album aggiornando il tipo di modello e aggiungendo l'elenco di album alla fine.</span><span class="sxs-lookup"><span data-stu-id="c603a-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="c603a-153">Questa opportunità consente anche di aggiungere una sezione di intestazione e promozione alla pagina.</span><span class="sxs-lookup"><span data-stu-id="c603a-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="c603a-154">A questo punto, quando si esegue l'applicazione, verranno visualizzati i home page aggiornati con i migliori album di vendita e il messaggio promozionale.</span><span class="sxs-lookup"><span data-stu-id="c603a-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="c603a-155">Conclusione</span><span class="sxs-lookup"><span data-stu-id="c603a-155">Conclusion</span></span>

<span data-ttu-id="c603a-156">Abbiamo visto che ASP.NET MVC semplifica la creazione di un sito Web sofisticato con accesso al database, appartenenza, AJAX e così via.</span><span class="sxs-lookup"><span data-stu-id="c603a-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="c603a-157">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="c603a-157">pretty quickly.</span></span> <span data-ttu-id="c603a-158">Speriamo che in questa esercitazione siano stati forniti gli strumenti necessari per iniziare a creare applicazioni MVC ASP.NET personalizzate.</span><span class="sxs-lookup"><span data-stu-id="c603a-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c603a-159">Precedente</span><span class="sxs-lookup"><span data-stu-id="c603a-159">Previous</span></span>](mvc-music-store-part-9.md)
