---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carrello acquisti con aggiornamenti Ajax | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 8 illustra il carrello della spesa con aggiornamenti AJAX.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539256"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="8f21a-104">Parte 8: carrello acquisti con aggiornamenti Ajax</span><span class="sxs-lookup"><span data-stu-id="8f21a-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="8f21a-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8f21a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8f21a-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="8f21a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8f21a-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="8f21a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8f21a-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="8f21a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8f21a-109">La parte 8 illustra il carrello della spesa con aggiornamenti AJAX.</span><span class="sxs-lookup"><span data-stu-id="8f21a-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="8f21a-110">Si consentirà agli utenti di inserire gli album nel carrello senza registrarli, ma sarà necessario registrarli come Guest per completare il checkout.</span><span class="sxs-lookup"><span data-stu-id="8f21a-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="8f21a-111">Il processo di acquisti e di estrazione verrà suddiviso in due controller: un controller ShoppingCart che consente l'aggiunta anonima di elementi a un carrello e un controller di checkout che gestisce il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="8f21a-112">Si inizierà con il carrello della spesa in questa sezione, quindi si creerà il processo di estrazione nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="8f21a-113">Aggiunta delle classi del modello cart, Order e OrderDetail</span><span class="sxs-lookup"><span data-stu-id="8f21a-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="8f21a-114">Il carrello acquisti e i processi di estrazione useranno alcune nuove classi.</span><span class="sxs-lookup"><span data-stu-id="8f21a-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="8f21a-115">Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe Cart (Cart.cs) con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="8f21a-116">Questa classe è molto simile a quella usata finora, ad eccezione dell'attributo [key] per la proprietà RecordId.</span><span class="sxs-lookup"><span data-stu-id="8f21a-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="8f21a-117">Gli elementi del carrello avranno un identificatore di stringa denominato CartID per consentire acquisti anonimi, ma la tabella include una chiave primaria integer denominata RecordId.</span><span class="sxs-lookup"><span data-stu-id="8f21a-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="8f21a-118">Per convenzione, il codice Entity Framework prevede innanzitutto che la chiave primaria per una tabella denominata cart sia CartId o ID, ma è possibile eseguire facilmente l'override di tale codice tramite annotazioni o codice, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="8f21a-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="8f21a-119">Questo è un esempio di come è possibile usare le convenzioni semplici in Entity Framework Code-First quando si adattano a Microsoft, ma non è vincolato da loro.</span><span class="sxs-lookup"><span data-stu-id="8f21a-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="8f21a-120">Aggiungere quindi una classe Order (Order.cs) con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="8f21a-121">Questa classe tiene traccia delle informazioni di riepilogo e di recapito per un ordine.</span><span class="sxs-lookup"><span data-stu-id="8f21a-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="8f21a-122">Non verrà **ancora compilato**, perché include una proprietà di navigazione OrderDetails che dipende da una classe che non è ancora stata creata.</span><span class="sxs-lookup"><span data-stu-id="8f21a-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="8f21a-123">Ora è possibile risolvere il problema aggiungendo una classe denominata OrderDetail.cs, aggiungendo il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="8f21a-124">Verrà apportato un ultimo aggiornamento alla classe MusicStoreEntities per includere DbSet che espongono le nuove classi del modello, includendo anche un DbSet&lt;Artist&gt;.</span><span class="sxs-lookup"><span data-stu-id="8f21a-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="8f21a-125">La classe MusicStoreEntities aggiornata viene visualizzata come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8f21a-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="8f21a-126">Gestione della logica di business del carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="8f21a-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="8f21a-127">Successivamente, verrà creata la classe ShoppingCart nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="8f21a-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="8f21a-128">Il modello ShoppingCart gestisce l'accesso ai dati alla tabella del carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="8f21a-129">Inoltre, la logica di business verrà gestita da per aggiungere e rimuovere elementi dal carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="8f21a-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="8f21a-130">Poiché non si desidera richiedere agli utenti di iscriversi a un account solo per aggiungere elementi al carrello acquisti, agli utenti viene assegnato un identificatore univoco temporaneo (usando un GUID o un identificatore univoco globale) quando accedono al carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="8f21a-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="8f21a-131">Questo ID verrà archiviato usando la classe della sessione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8f21a-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="8f21a-132">*Nota: la sessione ASP.NET è una soluzione ideale per archiviare le informazioni specifiche dell'utente che scadranno dopo l'uscita dal sito. Sebbene l'utilizzo improprio dello stato della sessione possa avere implicazioni sulle prestazioni nei siti più grandi, l'uso leggero funzionerà a scopo dimostrativo.*</span><span class="sxs-lookup"><span data-stu-id="8f21a-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="8f21a-133">La classe ShoppingCart espone i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8f21a-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="8f21a-134">**AddToCart** accetta un album come parametro e lo aggiunge al carrello dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="8f21a-135">Poiché la tabella del carrello tiene traccia della quantità per ogni album, include la logica per creare una nuova riga, se necessario, oppure incrementare solo la quantità se l'utente ha già ordinato una copia dell'album.</span><span class="sxs-lookup"><span data-stu-id="8f21a-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="8f21a-136">**RemoveFromCart** accetta un ID album e lo rimuove dal carrello dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="8f21a-137">Se l'utente ha una sola copia dell'album nel carrello, la riga viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="8f21a-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="8f21a-138">**EmptyCart** rimuove tutti gli elementi dal carrello acquisti di un utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="8f21a-139">**GetCartItems** recupera un elenco di CartItems per la visualizzazione o l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="8f21a-140">**GetCount** Recupera il numero totale di album di un utente nel carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="8f21a-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="8f21a-141">**GetTotal** calcola il costo totale di tutti gli elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="8f21a-142">**CreateOrder** converte il carrello acquisti in un ordine durante la fase di estrazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="8f21a-143">**GetCart** è un metodo statico che consente ai controller di ottenere un oggetto del carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="8f21a-144">Usa il metodo **GetCartId** per gestire la lettura del CartId dalla sessione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="8f21a-145">Il metodo GetCartId richiede il HttpContextBase, in modo che possa leggere il CartId dell'utente dalla sessione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="8f21a-146">Di seguito è illustrata la **classe ShoppingCart**completa:</span><span class="sxs-lookup"><span data-stu-id="8f21a-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="8f21a-147">ViewModel</span><span class="sxs-lookup"><span data-stu-id="8f21a-147">ViewModels</span></span>

<span data-ttu-id="8f21a-148">Il controller del carrello acquisti dovrà comunicare alcune informazioni complesse con le visualizzazioni che non sono mappate agli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="8f21a-149">Non è necessario modificare i modelli in base alle visualizzazioni; Le classi del modello devono rappresentare il nostro dominio e non l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="8f21a-150">Una soluzione consiste nel passare le informazioni alle visualizzazioni usando la classe ViewBag, come è stato fatto con le informazioni a discesa di Store Manager, ma il passaggio di una grande quantità di informazioni tramite ViewBag diventa difficile da gestire.</span><span class="sxs-lookup"><span data-stu-id="8f21a-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="8f21a-151">Una soluzione a questo problema consiste nell'usare il modello *ViewModel* .</span><span class="sxs-lookup"><span data-stu-id="8f21a-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="8f21a-152">Quando si usa questo modello, vengono create classi fortemente tipizzate ottimizzate per gli scenari di visualizzazione specifici e che espongono le proprietà per i valori dinamici o il contenuto necessari per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="8f21a-153">Le classi controller possono quindi popolare e passare le classi ottimizzate per la visualizzazione al modello di visualizzazione da usare.</span><span class="sxs-lookup"><span data-stu-id="8f21a-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="8f21a-154">In questo modo è possibile abilitare l'indipendenza dai tipi, il controllo in fase di compilazione e l'editor IntelliSense nei modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="8f21a-155">Verranno creati due modelli di visualizzazione da usare nel nostro controller del carrello della spesa: il ShoppingCartViewModel conterrà il contenuto del carrello acquisti dell'utente e ShoppingCartRemoveViewModel verrà usato per visualizzare le informazioni di conferma quando un utente rimuove qualcosa dal carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="8f21a-156">Viene ora creata una nuova cartella ViewModels nella radice del progetto per organizzare le operazioni.</span><span class="sxs-lookup"><span data-stu-id="8f21a-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="8f21a-157">Fare clic con il pulsante destro del mouse sul progetto e scegliere Aggiungi/nuova cartella.</span><span class="sxs-lookup"><span data-stu-id="8f21a-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="8f21a-158">Denominare la cartella ViewModels.</span><span class="sxs-lookup"><span data-stu-id="8f21a-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="8f21a-159">Successivamente, aggiungere la classe ShoppingCartViewModel nella cartella ViewModels.</span><span class="sxs-lookup"><span data-stu-id="8f21a-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="8f21a-160">Dispone di due proprietà: un elenco di elementi del carrello e un valore decimale che contiene il prezzo totale per tutti gli elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="8f21a-161">Aggiungere ora ShoppingCartRemoveViewModel alla cartella ViewModels, con le quattro proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="8f21a-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="8f21a-162">Il controller del carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="8f21a-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="8f21a-163">Il controller del carrello acquisti ha tre scopi principali: l'aggiunta di elementi a un carrello, la rimozione di elementi dal carrello e la visualizzazione di elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="8f21a-164">Utilizzerà le tre classi appena create: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="8f21a-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="8f21a-165">Come in StoreController e StoreManagerController, si aggiungerà un campo per la detenuta di un'istanza di MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="8f21a-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="8f21a-166">Aggiungere al progetto un nuovo controller del carrello acquisti usando il modello di controller vuoto.</span><span class="sxs-lookup"><span data-stu-id="8f21a-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="8f21a-167">Ecco il controller ShoppingCart completo.</span><span class="sxs-lookup"><span data-stu-id="8f21a-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="8f21a-168">Le azioni index e Add controller dovrebbero avere un aspetto molto familiare.</span><span class="sxs-lookup"><span data-stu-id="8f21a-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="8f21a-169">Le azioni del controller Remove e CartSummary gestiscono due casi speciali, che verranno illustrati nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="8f21a-170">Aggiornamenti AJAX con jQuery</span><span class="sxs-lookup"><span data-stu-id="8f21a-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="8f21a-171">Si creerà quindi una pagina di indice del carrello acquisti fortemente tipizzata per il ShoppingCartViewModel e si userà il modello di visualizzazione elenco usando lo stesso metodo precedente.</span><span class="sxs-lookup"><span data-stu-id="8f21a-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="8f21a-172">Tuttavia, invece di usare un ActionLink HTML per rimuovere gli elementi dal carrello, si userà jQuery per "collegare" l'evento click per tutti i collegamenti in questa visualizzazione con la classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="8f21a-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="8f21a-173">Anziché pubblicare il modulo, questo gestore dell'evento Click effettuerà semplicemente una richiamata AJAX all'azione del controller RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="8f21a-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="8f21a-174">RemoveFromCart restituisce un risultato serializzato JSON, che il callback jQuery analizza ed esegue quattro aggiornamenti rapidi alla pagina usando jQuery:</span><span class="sxs-lookup"><span data-stu-id="8f21a-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="8f21a-175">Rimuove l'album eliminato dall'elenco</span><span class="sxs-lookup"><span data-stu-id="8f21a-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="8f21a-176">Aggiorna il numero di carrelli nell'intestazione</span><span class="sxs-lookup"><span data-stu-id="8f21a-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="8f21a-177">Visualizza un messaggio di aggiornamento all'utente</span><span class="sxs-lookup"><span data-stu-id="8f21a-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="8f21a-178">Aggiorna il prezzo totale del carrello</span><span class="sxs-lookup"><span data-stu-id="8f21a-178">Updates the cart total price</span></span>

<span data-ttu-id="8f21a-179">Poiché lo scenario di rimozione è gestito da un callback Ajax nella visualizzazione dell'indice, non è necessaria una visualizzazione aggiuntiva per l'azione RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="8f21a-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="8f21a-180">Ecco il codice completo per la vista/ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="8f21a-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="8f21a-181">Per eseguire il test, è necessario essere in grado di aggiungere elementi al carrello della spesa.</span><span class="sxs-lookup"><span data-stu-id="8f21a-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="8f21a-182">La visualizzazione dei **Dettagli del negozio** verrà aggiornata in modo da includere un pulsante "Aggiungi al carrello".</span><span class="sxs-lookup"><span data-stu-id="8f21a-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="8f21a-183">In questo articolo, possiamo includere alcune informazioni aggiuntive sull'album aggiunte dopo l'ultimo aggiornamento di questa visualizzazione: genere, artista, prezzo e album.</span><span class="sxs-lookup"><span data-stu-id="8f21a-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="8f21a-184">Il codice di visualizzazione dettagli archivio aggiornato viene visualizzato come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8f21a-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="8f21a-185">A questo punto, è possibile fare clic su Store e testare l'aggiunta e la rimozione di album da e verso il carrello della spesa.</span><span class="sxs-lookup"><span data-stu-id="8f21a-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="8f21a-186">Eseguire l'applicazione e passare all'indice dello Store.</span><span class="sxs-lookup"><span data-stu-id="8f21a-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="8f21a-187">Fare quindi clic su un genere per visualizzare un elenco di album.</span><span class="sxs-lookup"><span data-stu-id="8f21a-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="8f21a-188">Facendo clic sul titolo di un album, viene ora visualizzata la visualizzazione dettagli album aggiornata, incluso il pulsante "Aggiungi al carrello".</span><span class="sxs-lookup"><span data-stu-id="8f21a-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="8f21a-189">Facendo clic sul pulsante "Aggiungi al carrello" viene visualizzata la visualizzazione dell'indice del carrello acquisti con l'elenco di riepilogo della spesa.</span><span class="sxs-lookup"><span data-stu-id="8f21a-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="8f21a-190">Dopo aver caricato il carrello della spesa, è possibile fare clic sul collegamento Rimuovi da carrello per visualizzare l'aggiornamento AJAX per il carrello della spesa.</span><span class="sxs-lookup"><span data-stu-id="8f21a-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="8f21a-191">È stato creato un carrello acquisti funzionante che consente agli utenti non registrati di aggiungere elementi al carrello.</span><span class="sxs-lookup"><span data-stu-id="8f21a-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="8f21a-192">Nella sezione seguente verrà consentito di registrare e completare il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="8f21a-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f21a-193">[Precedente](mvc-music-store-part-7.md)
> [Successivo](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="8f21a-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
