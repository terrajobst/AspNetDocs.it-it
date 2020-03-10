---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: logica di business | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 5 aggiunge una logica di business.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630305"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="3e51f-104">Parte 5: logica di business</span><span class="sxs-lookup"><span data-stu-id="3e51f-104">Part 5: Business Logic</span></span>

<span data-ttu-id="3e51f-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3e51f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3e51f-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="3e51f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3e51f-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="3e51f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3e51f-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="3e51f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3e51f-109">La parte 5 aggiunge una logica di business.</span><span class="sxs-lookup"><span data-stu-id="3e51f-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="3e51f-110">Aggiunta di una logica di business</span><span class="sxs-lookup"><span data-stu-id="3e51f-110">Adding Some Business Logic</span></span>

<span data-ttu-id="3e51f-111">Si vuole che la nostra esperienza di acquisto sia disponibile ogni volta che qualcuno visita il sito Web.</span><span class="sxs-lookup"><span data-stu-id="3e51f-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="3e51f-112">I visitatori saranno in grado di esplorare e aggiungere elementi al carrello acquisti anche se non sono registrati o connessi.</span><span class="sxs-lookup"><span data-stu-id="3e51f-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="3e51f-113">Quando si è pronti per verificare, viene fornita la possibilità di eseguire l'autenticazione e, se non sono ancora membri, saranno in grado di creare un account.</span><span class="sxs-lookup"><span data-stu-id="3e51f-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="3e51f-114">Ciò significa che sarà necessario implementare la logica per convertire il carrello acquisti da uno stato anonimo a uno stato "utente registrato".</span><span class="sxs-lookup"><span data-stu-id="3e51f-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="3e51f-115">Creare una directory denominata "classi", quindi fare clic con il pulsante destro del mouse sulla cartella e creare un nuovo file "class" denominato MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="3e51f-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="3e51f-116">Come indicato in precedenza, si estenderà la classe che implementa la pagina MyShoppingCart. aspx. questa operazione verrà eseguita usando. Costrutto "classe parziale" potente di NET.</span><span class="sxs-lookup"><span data-stu-id="3e51f-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="3e51f-117">La chiamata generata per il file MyShoppingCart.aspx.cf è simile alla seguente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="3e51f-118">Si noti l'uso della parola chiave "partial".</span><span class="sxs-lookup"><span data-stu-id="3e51f-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="3e51f-119">Il file di classe appena generato ha un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="3e51f-120">Le implementazioni vengono unite aggiungendo la parola chiave partial anche a questo file.</span><span class="sxs-lookup"><span data-stu-id="3e51f-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="3e51f-121">Il nuovo file di classe ha ora un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="3e51f-122">Il primo metodo che verrà aggiunto alla classe è il metodo "AddItem".</span><span class="sxs-lookup"><span data-stu-id="3e51f-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="3e51f-123">Si tratta del metodo che verrà infine chiamato quando l'utente fa clic sui collegamenti "Aggiungi all'arte" nelle pagine elenco prodotti e dettagli prodotto.</span><span class="sxs-lookup"><span data-stu-id="3e51f-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="3e51f-124">Aggiungere quanto segue alle istruzioni using nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="3e51f-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="3e51f-125">E aggiungono questo metodo alla classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="3e51f-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="3e51f-126">Si sta usando LINQ to Entities per verificare se l'elemento è già presente nel carrello.</span><span class="sxs-lookup"><span data-stu-id="3e51f-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="3e51f-127">In tal caso, viene aggiornata la quantità dell'ordine dell'elemento. in caso contrario, viene creata una nuova voce per l'elemento selezionato</span><span class="sxs-lookup"><span data-stu-id="3e51f-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="3e51f-128">Per chiamare questo metodo, verrà implementata una pagina AddToCart. aspx che non solo è la classe di questo metodo, ma che ha quindi visualizzato la spesa corrente a = carrello dopo l'aggiunta dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="3e51f-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="3e51f-129">Fare clic con il pulsante destro del mouse sul nome della soluzione in Esplora soluzioni e aggiungere e la nuova pagina denominata AddToCart. aspx, come è stato fatto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3e51f-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="3e51f-130">Sebbene sia possibile usare questa pagina per visualizzare risultati intermedi, ad esempio problemi di magazzino ridotti e così via, nell'implementazione di questa pagina non verrà effettivamente eseguito il rendering, bensì la logica di "aggiunta" e il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="3e51f-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="3e51f-131">A tale scopo, verrà aggiunto il codice seguente alla pagina\_evento Load.</span><span class="sxs-lookup"><span data-stu-id="3e51f-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="3e51f-132">Si noti che viene recuperato il prodotto da aggiungere al carrello acquisti da un parametro QueryString e viene chiamato il metodo AddItem della classe.</span><span class="sxs-lookup"><span data-stu-id="3e51f-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="3e51f-133">Supponendo che non vengano rilevati errori, il controllo viene passato alla pagina SHoppingCart. aspx che verrà implementata completamente successivamente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="3e51f-134">Se si verifica un errore, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3e51f-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="3e51f-135">Attualmente non è stato ancora implementato un gestore degli errori globale in modo che questa eccezione non venga gestita dall'applicazione, ma verrà rimediata a breve.</span><span class="sxs-lookup"><span data-stu-id="3e51f-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="3e51f-136">Si noti inoltre l'utilizzo dell'istruzione debug. Fail () (disponibile tramite `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="3e51f-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="3e51f-137">Se l'applicazione è in esecuzione all'interno del debugger, questo metodo visualizzerà una finestra di dialogo dettagliata con informazioni sullo stato delle applicazioni insieme al messaggio di errore specificato.</span><span class="sxs-lookup"><span data-stu-id="3e51f-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="3e51f-138">Quando è in esecuzione nell'ambiente di produzione, l'istruzione debug. Fail () viene ignorata.</span><span class="sxs-lookup"><span data-stu-id="3e51f-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="3e51f-139">Si noterà che nel codice sopra una chiamata a un metodo nei nomi delle classi del carrello acquisti "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="3e51f-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="3e51f-140">Aggiungere il codice per implementare il metodo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3e51f-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="3e51f-141">Si noti che sono stati anche aggiunti i pulsanti di aggiornamento ed estrazione e un'etichetta in cui è possibile visualizzare il carrello "Total".</span><span class="sxs-lookup"><span data-stu-id="3e51f-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="3e51f-142">È ora possibile aggiungere elementi al carrello della spesa, ma la logica non è stata implementata per visualizzare il carrello dopo l'aggiunta di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="3e51f-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="3e51f-143">Quindi, nella pagina MyShoppingCart. aspx verrà aggiunto un controllo EntityDataSource e un controllo GridVire come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3e51f-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="3e51f-144">Chiamare il form nella finestra di progettazione in modo che sia possibile fare doppio clic sul pulsante Aggiorna carrello e generare il gestore dell'evento Click specificato nella dichiarazione nel markup.</span><span class="sxs-lookup"><span data-stu-id="3e51f-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="3e51f-145">Verranno implementati i dettagli in un secondo momento, ma questa operazione consente di compilare ed eseguire l'applicazione senza errori.</span><span class="sxs-lookup"><span data-stu-id="3e51f-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="3e51f-146">Quando si esegue l'applicazione e si aggiunge un elemento al carrello acquisti, questo verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3e51f-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="3e51f-147">Si noti che la visualizzazione della griglia "predefinita" è stata deviata implementando tre colonne personalizzate.</span><span class="sxs-lookup"><span data-stu-id="3e51f-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="3e51f-148">Il primo è un campo modificabile "associato" per la quantità:</span><span class="sxs-lookup"><span data-stu-id="3e51f-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="3e51f-149">La successiva è una colonna "calcolata" che Visualizza il totale dell'elemento (il costo dell'elemento è la quantità da ordinare):</span><span class="sxs-lookup"><span data-stu-id="3e51f-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="3e51f-150">Infine, abbiamo una colonna personalizzata che contiene un controllo CheckBox che verrà usato dall'utente per indicare che l'elemento deve essere rimosso dal grafico degli acquisti.</span><span class="sxs-lookup"><span data-stu-id="3e51f-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="3e51f-151">Come si può notare, la riga Order Total è vuota, quindi verrà aggiunta una logica per calcolare il totale dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="3e51f-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="3e51f-152">Per prima cosa verrà implementato un metodo "getTotal" per la classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="3e51f-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="3e51f-153">Nel file MyShoppingCart.cs aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="3e51f-154">Quindi, nella pagina\_caricare il gestore eventi, è possibile chiamare il metodo getTotal.</span><span class="sxs-lookup"><span data-stu-id="3e51f-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="3e51f-155">Allo stesso tempo, verrà aggiunto un test per verificare se il carrello acquisti è vuoto e modificare di conseguenza la visualizzazione, se è.</span><span class="sxs-lookup"><span data-stu-id="3e51f-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="3e51f-156">A questo punto, se il carrello acquisti è vuoto, si ottiene quanto segue:</span><span class="sxs-lookup"><span data-stu-id="3e51f-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="3e51f-157">In caso contrario, viene visualizzato il totale.</span><span class="sxs-lookup"><span data-stu-id="3e51f-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="3e51f-158">Tuttavia, questa pagina non è ancora completa.</span><span class="sxs-lookup"><span data-stu-id="3e51f-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="3e51f-159">Sarà necessaria una logica aggiuntiva per ricalcolare il carrello della spesa rimuovendo gli elementi contrassegnati per la rimozione e determinando nuovi valori di quantità, perché alcuni potrebbero essere stati modificati nella griglia dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="3e51f-160">Consente di aggiungere un metodo "RemoveItem" alla classe del carrello della spesa in MyShoppingCart.cs per gestire il caso in cui un utente contrassegna un elemento per la rimozione.</span><span class="sxs-lookup"><span data-stu-id="3e51f-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="3e51f-161">A questo punto, ad un metodo è possibile gestire la circostanza quando un utente modifica semplicemente la qualità da ordinare in GridView.</span><span class="sxs-lookup"><span data-stu-id="3e51f-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="3e51f-162">Con le funzionalità di base di rimozione e aggiornamento, è possibile implementare la logica che aggiorna effettivamente il carrello acquisti nel database.</span><span class="sxs-lookup"><span data-stu-id="3e51f-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="3e51f-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="3e51f-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="3e51f-164">Si noterà che questo metodo prevede due parametri.</span><span class="sxs-lookup"><span data-stu-id="3e51f-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="3e51f-165">Uno è l'ID del carrello acquisti e l'altro è una matrice di oggetti di tipo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3e51f-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="3e51f-166">Per ridurre al minimo la dipendenza della logica sulle specifiche dell'interfaccia utente, è stata definita una struttura di dati che è possibile usare per passare gli elementi del carrello acquisti al codice senza che il metodo debba accedere direttamente al controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="3e51f-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="3e51f-167">Nel file MyShoppingCart.aspx.cs è possibile usare questa struttura nel gestore dell'evento click del pulsante Aggiorna, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3e51f-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="3e51f-168">Si noti che, oltre ad aggiornare il carrello, viene aggiornato anche il totale del carrello.</span><span class="sxs-lookup"><span data-stu-id="3e51f-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="3e51f-169">Si noti che con particolare interesse questa riga di codice:</span><span class="sxs-lookup"><span data-stu-id="3e51f-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="3e51f-170">GetValues () è una funzione di supporto speciale che verrà implementata in MyShoppingCart.aspx.cs, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3e51f-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="3e51f-171">Questo consente di accedere in modo semplice ai valori degli elementi associati nel controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="3e51f-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="3e51f-172">Poiché il controllo CheckBox "Rimuovi elemento" non è associato, sarà possibile accedervi tramite il metodo FindControl ().</span><span class="sxs-lookup"><span data-stu-id="3e51f-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="3e51f-173">In questa fase dello sviluppo del progetto si è pronti per implementare il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="3e51f-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="3e51f-174">Prima di procedere, è possibile usare Visual Studio per generare il database delle appartenenze e aggiungere un utente al repository di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="3e51f-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e51f-175">[Precedente](tailspin-spyworks-part-4.md)
> [Successivo](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3e51f-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
