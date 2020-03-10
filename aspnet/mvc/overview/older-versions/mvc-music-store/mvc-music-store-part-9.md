---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registrazione ed estrazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 9 illustra la registrazione e l'estrazione.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559535"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="a753a-104">Parte 9: registrazione ed estrazione</span><span class="sxs-lookup"><span data-stu-id="a753a-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="a753a-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a753a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a753a-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="a753a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a753a-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="a753a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a753a-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a753a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a753a-109">La parte 9 illustra la registrazione e l'estrazione.</span><span class="sxs-lookup"><span data-stu-id="a753a-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="a753a-110">In questa sezione verrà creata una CheckoutController che raccoglierà l'indirizzo e le informazioni di pagamento del cliente.</span><span class="sxs-lookup"><span data-stu-id="a753a-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="a753a-111">Prima di eseguire il checkout, è necessario che gli utenti si registrino con il sito, pertanto il controller richiederà l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a753a-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="a753a-112">Gli utenti accederanno al processo di estrazione dal carrello acquisti facendo clic sul pulsante "Estrai".</span><span class="sxs-lookup"><span data-stu-id="a753a-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="a753a-113">Se l'utente non ha eseguito l'accesso, verrà richiesto di farlo.</span><span class="sxs-lookup"><span data-stu-id="a753a-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="a753a-114">Al completamento dell'accesso, l'utente viene quindi visualizzato l'indirizzo e la visualizzazione dei pagamenti.</span><span class="sxs-lookup"><span data-stu-id="a753a-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="a753a-115">Una volta compilato il modulo e inviato l'ordine, verrà visualizzata la schermata di conferma dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="a753a-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="a753a-116">Se si tenta di visualizzare un ordine inesistente o un ordine che non appartiene a, verrà visualizzata la visualizzazione degli errori.</span><span class="sxs-lookup"><span data-stu-id="a753a-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="a753a-117">Migrazione del carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="a753a-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="a753a-118">Mentre il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante Checkout, sarà necessario registrarsi e accedere a.</span><span class="sxs-lookup"><span data-stu-id="a753a-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="a753a-119">Gli utenti si aspettano che le informazioni del carrello acquisti vengano mantenute tra le visite, pertanto sarà necessario associare le informazioni del carrello acquisti a un utente quando completano la registrazione o l'accesso.</span><span class="sxs-lookup"><span data-stu-id="a753a-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="a753a-120">Questa operazione è molto semplice, in quanto la classe ShoppingCart dispone già di un metodo che associa tutti gli elementi nel carrello corrente a un nome utente.</span><span class="sxs-lookup"><span data-stu-id="a753a-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="a753a-121">Quando un utente completa la registrazione o l'accesso, è sufficiente chiamare questo metodo.</span><span class="sxs-lookup"><span data-stu-id="a753a-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="a753a-122">Aprire la classe **AccountController** aggiunta durante la configurazione dell'appartenenza e dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a753a-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="a753a-123">Aggiungere un'istruzione using che fa riferimento a MvcMusicStore. Models, quindi aggiungere il metodo MigrateShoppingCart seguente:</span><span class="sxs-lookup"><span data-stu-id="a753a-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="a753a-124">Modificare quindi l'azione post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="a753a-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="a753a-125">Apportare la stessa modifica all'azione registra post, immediatamente dopo la creazione dell'account utente:</span><span class="sxs-lookup"><span data-stu-id="a753a-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="a753a-126">È ora possibile trasferire automaticamente un carrello acquisti anonimo a un account utente al termine della registrazione o dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="a753a-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="a753a-127">Creazione di CheckoutController</span><span class="sxs-lookup"><span data-stu-id="a753a-127">Creating the CheckoutController</span></span>

<span data-ttu-id="a753a-128">Fare clic con il pulsante destro del mouse sulla cartella Controllers e aggiungere un nuovo controller al progetto denominato CheckoutController usando il modello di controller vuoto.</span><span class="sxs-lookup"><span data-stu-id="a753a-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="a753a-129">Aggiungere prima di tutto l'attributo autorizza sopra la dichiarazione della classe controller per richiedere agli utenti di registrarsi prima del checkout:</span><span class="sxs-lookup"><span data-stu-id="a753a-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="a753a-130">*Nota: questa operazione è simile alla modifica apportata in precedenza a StoreManagerController, ma in questo caso l'attributo di autorizzazione richiede che l'utente sia in un ruolo di amministratore. Nel controller di checkout è necessario che l'utente sia connesso, ma non richiede che siano amministratori.*</span><span class="sxs-lookup"><span data-stu-id="a753a-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="a753a-131">Per motivi di semplicità, in questa esercitazione non verranno trattate le informazioni di pagamento.</span><span class="sxs-lookup"><span data-stu-id="a753a-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="a753a-132">Al contrario, si consente agli utenti di consultare l'uso di un codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="a753a-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="a753a-133">Questo codice promozionale verrà archiviato usando una costante denominata promozione.</span><span class="sxs-lookup"><span data-stu-id="a753a-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="a753a-134">Come in StoreController, si dichiara un campo per includere un'istanza della classe MusicStoreEntities, denominata storeDB.</span><span class="sxs-lookup"><span data-stu-id="a753a-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="a753a-135">Per poter usare la classe MusicStoreEntities, è necessario aggiungere un'istruzione using per lo spazio dei nomi MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="a753a-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="a753a-136">Di seguito viene visualizzata la parte superiore del controller di estrazione.</span><span class="sxs-lookup"><span data-stu-id="a753a-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="a753a-137">Il CheckoutController avrà le seguenti azioni del controller:</span><span class="sxs-lookup"><span data-stu-id="a753a-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="a753a-138">**AddressAndPayment (Get Method)** visualizzerà un modulo per consentire all'utente di immettere le informazioni.</span><span class="sxs-lookup"><span data-stu-id="a753a-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="a753a-139">**AddressAndPayment (metodo post)** consente di convalidare l'input ed elaborare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="a753a-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="a753a-140">Il **completamento** verrà visualizzato dopo che un utente ha completato il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="a753a-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="a753a-141">Questa visualizzazione includerà il numero di ordine dell'utente, come conferma.</span><span class="sxs-lookup"><span data-stu-id="a753a-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="a753a-142">Rinominare prima di tutto l'azione del controller di indice, che è stata generata quando è stato creato il controller, in AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="a753a-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="a753a-143">Questa azione del controller Visualizza solo il modulo di checkout, quindi non richiede informazioni sul modello.</span><span class="sxs-lookup"><span data-stu-id="a753a-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="a753a-144">Il metodo POST di AddressAndPayment seguirà lo stesso modello usato in StoreManagerController: tenterà di accettare l'invio del modulo e completerà l'ordine e visualizzerà nuovamente il modulo in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="a753a-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="a753a-145">Dopo aver convalidato l'input del modulo soddisfa i requisiti di convalida per un ordine, si verificherà direttamente il valore del modulo Promozione.</span><span class="sxs-lookup"><span data-stu-id="a753a-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="a753a-146">Supponendo che tutto sia corretto, le informazioni aggiornate vengono salvate con l'ordine, si indica all'oggetto ShoppingCart di completare il processo di ordine e si reindirizza all'azione completa.</span><span class="sxs-lookup"><span data-stu-id="a753a-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="a753a-147">Al termine del processo di checkout, gli utenti verranno reindirizzati all'azione completa del controller.</span><span class="sxs-lookup"><span data-stu-id="a753a-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="a753a-148">Questa azione consente di eseguire un semplice controllo per verificare che l'ordine appartenga effettivamente all'utente connesso prima di visualizzare il numero di ordine come una conferma.</span><span class="sxs-lookup"><span data-stu-id="a753a-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="a753a-149">*Nota: la visualizzazione degli errori è stata creata automaticamente nella cartella/Views/Shared. quando è stato avviato il progetto.*</span><span class="sxs-lookup"><span data-stu-id="a753a-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="a753a-150">Il codice CheckoutController completo è il seguente:</span><span class="sxs-lookup"><span data-stu-id="a753a-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="a753a-151">Aggiunta della visualizzazione AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="a753a-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="a753a-152">A questo punto, creare la visualizzazione AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="a753a-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="a753a-153">Fare clic con il pulsante destro del mouse su una delle azioni del controller AddressAndPayment e aggiungere una vista denominata AddressAndPayment che è fortemente tipizzata come ordine e usa il modello di modifica, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a753a-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="a753a-154">Questa visualizzazione consente di usare due delle tecniche analizzate durante la creazione della vista StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="a753a-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="a753a-155">Si userà HTML. EditorForModel () per visualizzare i campi del modulo per il modello Order</span><span class="sxs-lookup"><span data-stu-id="a753a-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="a753a-156">Le regole di convalida vengono sfruttate usando una classe Order con attributi di convalida</span><span class="sxs-lookup"><span data-stu-id="a753a-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="a753a-157">Si inizierà con l'aggiornamento del codice del modulo per usare HTML. EditorForModel (), seguito da una casella di testo aggiuntiva per il codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="a753a-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="a753a-158">Di seguito è riportato il codice completo per la visualizzazione AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="a753a-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="a753a-159">Definizione delle regole di convalida per l'ordine</span><span class="sxs-lookup"><span data-stu-id="a753a-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="a753a-160">Ora che la visualizzazione è configurata, verranno configurate le regole di convalida per il modello Order come in precedenza per il modello di album.</span><span class="sxs-lookup"><span data-stu-id="a753a-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="a753a-161">Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe denominata Order.</span><span class="sxs-lookup"><span data-stu-id="a753a-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="a753a-162">Oltre agli attributi di convalida usati in precedenza per l'album, si utilizzerà anche un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a753a-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="a753a-163">Se si tenta di inviare il modulo con informazioni mancanti o non valide, verrà visualizzato un messaggio di errore con la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="a753a-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="a753a-164">È stata eseguita la maggior parte delle operazioni necessarie per il processo di checkout; Ci sono solo alcune probabilità e termina per terminare.</span><span class="sxs-lookup"><span data-stu-id="a753a-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="a753a-165">È necessario aggiungere due visualizzazioni semplici ed è necessario prestare attenzione alla consegna delle informazioni del carrello durante il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="a753a-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="a753a-166">Aggiunta della visualizzazione completamento estrazione</span><span class="sxs-lookup"><span data-stu-id="a753a-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="a753a-167">La visualizzazione completamento estrazione è piuttosto semplice, in quanto deve solo visualizzare l'ID dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="a753a-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="a753a-168">Fare clic con il pulsante destro del mouse sull'azione completa controller e aggiungere una vista denominata completa che è fortemente tipizzata come int.</span><span class="sxs-lookup"><span data-stu-id="a753a-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="a753a-169">A questo punto, verrà aggiornato il codice di visualizzazione per visualizzare l'ID dell'ordine, come mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a753a-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="a753a-170">Aggiornamento della visualizzazione degli errori</span><span class="sxs-lookup"><span data-stu-id="a753a-170">Updating The Error view</span></span>

<span data-ttu-id="a753a-171">Il modello predefinito include una visualizzazione degli errori nella cartella delle visualizzazioni condivise, in modo che possa essere riutilizzata altrove nel sito.</span><span class="sxs-lookup"><span data-stu-id="a753a-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="a753a-172">Questa visualizzazione degli errori contiene un errore molto semplice e non usa il layout del sito, quindi verrà aggiornata.</span><span class="sxs-lookup"><span data-stu-id="a753a-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="a753a-173">Poiché si tratta di una pagina di errore generica, il contenuto è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="a753a-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="a753a-174">Si includerà un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente vuole riprovare a eseguire l'azione.</span><span class="sxs-lookup"><span data-stu-id="a753a-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="a753a-175">[Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="a753a-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
