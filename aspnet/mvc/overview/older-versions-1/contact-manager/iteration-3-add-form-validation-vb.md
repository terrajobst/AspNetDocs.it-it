---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iterazione #3-aggiungere la convalida dei form (VB) | Microsoft Docs'
author: microsoft
description: Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È inoltre possibile convalidare emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: b44aaab45f04f736e4171a43a8b24b71aaedca2f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039658"
---
<a name="iteration-3--add-form-validation-vb"></a><span data-ttu-id="65861-105">Iterazione #3-aggiungere la convalida dei form (VB)</span><span class="sxs-lookup"><span data-stu-id="65861-105">Iteration #3 – Add form validation (VB)</span></span>
====================
<span data-ttu-id="65861-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65861-106">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="65861-107">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="65861-107">Download Code</span></span>](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> <span data-ttu-id="65861-108">Nella terza iterazione, aggiungiamo la convalida dei form di base.</span><span class="sxs-lookup"><span data-stu-id="65861-108">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="65861-109">Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto.</span><span class="sxs-lookup"><span data-stu-id="65861-109">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="65861-110">È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="65861-110">We also validate email addresses and phone numbers.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="65861-111">Creazione di un'applicazione di gestione dei contatti ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="65861-111">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="65861-112">In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine.</span><span class="sxs-lookup"><span data-stu-id="65861-112">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="65861-113">L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.</span><span class="sxs-lookup"><span data-stu-id="65861-113">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="65861-114">Si compilerà l'applicazione a varie iterazioni.</span><span class="sxs-lookup"><span data-stu-id="65861-114">We build the application over multiple iterations.</span></span> <span data-ttu-id="65861-115">A ogni iterazione, abbiamo migliorare gradualmente l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65861-115">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="65861-116">L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.</span><span class="sxs-lookup"><span data-stu-id="65861-116">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="65861-117">Iterazione #1 - creare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65861-117">Iteration #1 - Create the application.</span></span> <span data-ttu-id="65861-118">Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile.</span><span class="sxs-lookup"><span data-stu-id="65861-118">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="65861-119">Viene aggiunto il supporto per le operazioni di base dei database: Creare, leggere, aggiornare ed eliminare (CRUD).</span><span class="sxs-lookup"><span data-stu-id="65861-119">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="65861-120">Iterazione #2 - migliorare l'applicazione aspetto interessante.</span><span class="sxs-lookup"><span data-stu-id="65861-120">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="65861-121">In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="65861-121">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="65861-122">Iterazione #3 - aggiungere la convalida dei form.</span><span class="sxs-lookup"><span data-stu-id="65861-122">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="65861-123">Nella terza iterazione, aggiungiamo la convalida dei form di base.</span><span class="sxs-lookup"><span data-stu-id="65861-123">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="65861-124">Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto.</span><span class="sxs-lookup"><span data-stu-id="65861-124">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="65861-125">È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="65861-125">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="65861-126">Iterazione #4 - rendere l'applicazione a regime.</span><span class="sxs-lookup"><span data-stu-id="65861-126">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="65861-127">In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="65861-127">In this fourth iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="65861-128">Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="65861-128">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="65861-129">Iterazione #5 - creare unit test.</span><span class="sxs-lookup"><span data-stu-id="65861-129">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="65861-130">Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test.</span><span class="sxs-lookup"><span data-stu-id="65861-130">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="65861-131">È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-131">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="65861-132">Iterazione #6 - usare lo sviluppo basato su test.</span><span class="sxs-lookup"><span data-stu-id="65861-132">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="65861-133">In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test.</span><span class="sxs-lookup"><span data-stu-id="65861-133">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="65861-134">In questa iterazione, viene aggiunto gruppi di contatti.</span><span class="sxs-lookup"><span data-stu-id="65861-134">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="65861-135">Iterazione #7 - aggiungere funzionalità Ajax.</span><span class="sxs-lookup"><span data-stu-id="65861-135">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="65861-136">Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65861-136">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="this-iteration"></a><span data-ttu-id="65861-137">Questa iterazione</span><span class="sxs-lookup"><span data-stu-id="65861-137">This Iteration</span></span>

<span data-ttu-id="65861-138">In questa seconda iterazione dell'applicazione Contact Manager, viene aggiunto la convalida dei form di base.</span><span class="sxs-lookup"><span data-stu-id="65861-138">In this second iteration of the Contact Manager application, we add basic form validation.</span></span> <span data-ttu-id="65861-139">Si evita che gli utenti dall'invio di un contatto senza dover immettere i valori per i campi modulo necessari.</span><span class="sxs-lookup"><span data-stu-id="65861-139">We prevent people from submitting a contact without entering values for required form fields.</span></span> <span data-ttu-id="65861-140">È inoltre possibile convalidare i numeri di telefono e indirizzi di posta elettronica (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="65861-140">We also validate phone numbers and email addresses (see Figure 1).</span></span>


<span data-ttu-id="65861-141">[![La finestra di dialogo Nuovo progetto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="65861-141">[![The New Project dialog box](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="65861-142">**Figura 01**: Un modulo con la convalida ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-3-add-form-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="65861-142">**Figure 01**: A form with validation ([Click to view full-size image](iteration-3-add-form-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="65861-143">In questa iterazione, la logica di convalida viene aggiunto direttamente per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="65861-143">In this iteration, we add the validation logic directly to the controller actions.</span></span> <span data-ttu-id="65861-144">In generale, non il metodo consigliato per aggiungere la convalida a un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="65861-144">In general, this is not the recommended way to add validation to an ASP.NET MVC application.</span></span> <span data-ttu-id="65861-145">Un approccio migliore consiste nell'inserire una logica di convalida s dell'applicazione in un oggetto separato [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html).</span><span class="sxs-lookup"><span data-stu-id="65861-145">A better approach is to place an application s validation logic in a separate [service layer](http://martinfowler.com/eaaCatalog/serviceLayer.html).</span></span> <span data-ttu-id="65861-146">Nell'iterazione successiva, è eseguire il refactoring all'applicazione Contact Manager di eseguire l'applicazione più facilmente gestibile.</span><span class="sxs-lookup"><span data-stu-id="65861-146">In the next iteration, we refactor the Contact Manager application to make the application more maintainable.</span></span>

<span data-ttu-id="65861-147">In questa iterazione, per semplicità, si scrive tutto il codice di convalida manualmente.</span><span class="sxs-lookup"><span data-stu-id="65861-147">In this iteration, to keep things simple, we write all of the validation code by hand.</span></span> <span data-ttu-id="65861-148">Invece di scrivere il codice di convalida noi stessi, è possibile sfruttare i vantaggi di un framework di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-148">Instead of writing the validation code ourselves, we could take advantage of a validation framework.</span></span> <span data-ttu-id="65861-149">Ad esempio, è possibile utilizzare Microsoft Enterprise Library convalida Application Block (VAB) per implementare la logica di convalida per l'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="65861-149">For example, you can use the Microsoft Enterprise Library Validation Application Block (VAB) to implement the validation logic for your ASP.NET MVC application.</span></span> <span data-ttu-id="65861-150">Per altre informazioni su del blocco applicazione per la convalida, vedere:</span><span class="sxs-lookup"><span data-stu-id="65861-150">To learn more about the Validation Application Block, see:</span></span>

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a><span data-ttu-id="65861-151">Aggiunta della convalida della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="65861-151">Adding Validation to the Create View</span></span>

<span data-ttu-id="65861-152">Let s iniziare aggiungendo la logica di convalida per la visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="65861-152">Let s start by adding validation logic to the Create view.</span></span> <span data-ttu-id="65861-153">Fortunatamente, perché è stato generato della visualizzazione di creazione con Visual Studio, la visualizzazione di creazione contiene già tutta la logica dell'interfaccia utente necessarie per visualizzare i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-153">Fortunately, because we generated the Create view with Visual Studio, the Create view already contains all of the necessary user interface logic to display validation messages.</span></span> <span data-ttu-id="65861-154">Visualizzazione di creazione è contenuta nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="65861-154">The Create view is contained in Listing 1.</span></span>

<span data-ttu-id="65861-155">**Listato 1 - \Views\Contact\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="65861-155">**Listing 1 - \Views\Contact\Create.aspx**</span></span>

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

<span data-ttu-id="65861-156">La classe di errore di convalida campo viene usata per definire lo stile dell'output sottoposto a rendering dall'helper Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="65861-156">The field-validation-error class is used to style the output rendered by the Html.ValidationMessage() helper.</span></span> <span data-ttu-id="65861-157">La classe di errore di convalida input viene utilizzata per definire lo stile casella di testo (input) viene eseguito il rendering dall'helper Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="65861-157">The input-validation-error class is used to style the textbox (input) rendered by the Html.TextBox() helper.</span></span> <span data-ttu-id="65861-158">La classe di errori di riepilogo di convalida viene utilizzata per definire lo stile di elenco non ordinato viene eseguito il rendering dall'helper Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="65861-158">The validation-summary-errors class is used to style the unordered list rendered by the Html.ValidationSummary() helper.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="65861-159">È possibile modificare le classi del foglio di stile descritte in questa sezione per personalizzare l'aspetto dei messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-159">You can modify the style sheet classes described in this section to customize the appearance of validation error messages.</span></span>


## <a name="adding-validation-logic-to-the-create-action"></a><span data-ttu-id="65861-160">Aggiunta della logica di convalida l'azione di creazione</span><span class="sxs-lookup"><span data-stu-id="65861-160">Adding Validation Logic to the Create Action</span></span>

<span data-ttu-id="65861-161">Al momento, la visualizzazione di creazione non visualizza mai i messaggi di errore di convalida perché non è stato scritto la logica per generare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="65861-161">Right now, the Create view never displays validation error messages because we have not written the logic to generate any messages.</span></span> <span data-ttu-id="65861-162">Per visualizzare i messaggi di errore di convalida, è necessario aggiungere i messaggi di errore per ModelState.</span><span class="sxs-lookup"><span data-stu-id="65861-162">In order to display validation error messages, you need to add the error messages to ModelState.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="65861-163">Il metodo UpdateModel() aggiunge messaggi di errore per ModelState automaticamente quando si verifica un errore assegnando il valore di un campo del form a una proprietà.</span><span class="sxs-lookup"><span data-stu-id="65861-163">The UpdateModel() method adds error messages to ModelState automatically when there is an error assigning the value of a form field to a property.</span></span> <span data-ttu-id="65861-164">Ad esempio, se si tenta di assegnare la stringa "apple" a una proprietà data di nascita che accetta i valori DateTime, il metodo UpdateModel() aggiunge un errore per ModelState.</span><span class="sxs-lookup"><span data-stu-id="65861-164">For example, if you attempt to assign the string "apple" to a BirthDate property that accepts DateTime values, then the UpdateModel() method adds an error to ModelState.</span></span>


<span data-ttu-id="65861-165">Il metodo Create () modificato nel listato 2 contiene una nuova sezione che convalida le proprietà della classe contattare prima il nuovo contatto viene inserito nel database.</span><span class="sxs-lookup"><span data-stu-id="65861-165">The modified Create() method in Listing 2 contains a new section that validates the properties of the Contact class before the new contact is inserted into the database.</span></span>

<span data-ttu-id="65861-166">**Listato 2 - Controllers\ContactController.vb (Create con la convalida)**</span><span class="sxs-lookup"><span data-stu-id="65861-166">**Listing 2 - Controllers\ContactController.vb (Create with validation)**</span></span>

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

<span data-ttu-id="65861-167">La sezione Convalida applica quattro regole di convalida distinti:</span><span class="sxs-lookup"><span data-stu-id="65861-167">The validate section enforces four distinct validation rules:</span></span>

- <span data-ttu-id="65861-168">La proprietà FirstName deve avere una lunghezza maggiore di zero (e non può essere composto esclusivamente spazi)</span><span class="sxs-lookup"><span data-stu-id="65861-168">The FirstName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="65861-169">La proprietà LastName deve avere una lunghezza maggiore di zero (e non può essere composto esclusivamente spazi)</span><span class="sxs-lookup"><span data-stu-id="65861-169">The LastName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="65861-170">Se la proprietà Phone ha un valore (ha una lunghezza maggiore di 0), quindi un'espressione regolare deve corrispondere la proprietà di telefono.</span><span class="sxs-lookup"><span data-stu-id="65861-170">If the Phone property has a value (has a length greater than 0) then the Phone property must match a regular expression.</span></span>
- <span data-ttu-id="65861-171">Se la proprietà indirizzo di posta elettronica ha un valore (ha una lunghezza maggiore di 0), quindi un'espressione regolare deve corrispondere la proprietà di messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="65861-171">If the Email property has a value (has a length greater than 0) then the Email property must match a regular expression.</span></span>

<span data-ttu-id="65861-172">Quando si verifica una violazione della regola di convalida, con l'aiuto del metodo AddModelError() ModelState viene aggiunto un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="65861-172">When there is a validation rule violation, an error message is added to ModelState with the help of the AddModelError() method.</span></span> <span data-ttu-id="65861-173">Quando si aggiunge un messaggio a ModelState, si fornisce il nome di una proprietà e il testo di un messaggio di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-173">When you add a message to ModelState, you provide the name of a property and the text of a validation error message.</span></span> <span data-ttu-id="65861-174">Questo messaggio di errore viene visualizzato nella vista dai metodi helper Html.ValidationSummary() e Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="65861-174">This error message is displayed in the view by the Html.ValidationSummary() and Html.ValidationMessage() helper methods.</span></span>

<span data-ttu-id="65861-175">Dopo che vengono eseguite le regole di convalida, viene verificata la proprietà IsValid di ModelState.</span><span class="sxs-lookup"><span data-stu-id="65861-175">After the validation rules are executed, the IsValid property of ModelState is checked.</span></span> <span data-ttu-id="65861-176">La proprietà IsValid restituisce false quando gli eventuali messaggi di errore di convalida sono stati aggiunti per ModelState.</span><span class="sxs-lookup"><span data-stu-id="65861-176">The IsValid property returns false when any validation error messages have been added to ModelState.</span></span> <span data-ttu-id="65861-177">Se la convalida non riesce, il modulo di creazione verrà visualizzata nuovamente con i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="65861-177">If validation fails, the Create form is redisplayed with the error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="65861-178">Ho le espressioni regolari per convalidare l'indirizzo di posta elettronica e numero di telefono dal repository dell'espressione regolare in [*http://regexlib.com*](http://regexlib.com)</span><span class="sxs-lookup"><span data-stu-id="65861-178">I got the regular expressions for validating the phone number and email address from the regular expression repository at [*http://regexlib.com*](http://regexlib.com)</span></span>


## <a name="adding-validation-logic-to-the-edit-action"></a><span data-ttu-id="65861-179">Aggiunta di logica di convalida per l'operazione di modifica</span><span class="sxs-lookup"><span data-stu-id="65861-179">Adding Validation Logic to the Edit Action</span></span>

<span data-ttu-id="65861-180">L'azione di Edit () aggiorna un contatto.</span><span class="sxs-lookup"><span data-stu-id="65861-180">The Edit() action updates a Contact.</span></span> <span data-ttu-id="65861-181">L'azione di Edit () deve seguire esattamente la stessa convalida l'azione create ().</span><span class="sxs-lookup"><span data-stu-id="65861-181">The Edit() action needs to perform exactly the same validation as the Create() action.</span></span> <span data-ttu-id="65861-182">Anziché ripetere lo stesso codice di convalida, è necessario effettuare il refactoring il controller di contattare in modo che le azioni sia create () ed Edit () chiama lo stesso metodo di convalida.</span><span class="sxs-lookup"><span data-stu-id="65861-182">Instead of duplicating the same validation code, we should refactor the Contact controller so that both the Create() and Edit() actions call the same validation method.</span></span>

<span data-ttu-id="65861-183">La classe controller contatto modificata è contenuta nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="65861-183">The modified Contact controller class is contained in Listing 3.</span></span> <span data-ttu-id="65861-184">Questa classe ha un nuovo metodo ValidateContact() che viene chiamato all'interno di azioni sia al metodo Edit ().</span><span class="sxs-lookup"><span data-stu-id="65861-184">This class has a new ValidateContact() method that is called within both the Create() and Edit() actions.</span></span>

<span data-ttu-id="65861-185">**Listing 3 - Controllers\ContactController.vb**</span><span class="sxs-lookup"><span data-stu-id="65861-185">**Listing 3 - Controllers\ContactController.vb**</span></span>

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a><span data-ttu-id="65861-186">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="65861-186">Summary</span></span>

<span data-ttu-id="65861-187">Questa iterazione, abbiamo aggiunto la convalida dei form di base per l'applicazione Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="65861-187">In this iteration, we added basic form validation to our Contact Manager application.</span></span> <span data-ttu-id="65861-188">La logica di convalida impedisce agli utenti di inviare un nuovo contatto o la modifica di un contatto esistente senza fornire valori per le proprietà FirstName e LastName.</span><span class="sxs-lookup"><span data-stu-id="65861-188">Our validation logic prevents users from submitting a new contact or editing an existing contact without supplying values for the FirstName and LastName properties.</span></span> <span data-ttu-id="65861-189">Inoltre, gli utenti devono fornire gli indirizzi di posta elettronica e numeri di telefono valido.</span><span class="sxs-lookup"><span data-stu-id="65861-189">Furthermore, users must supply valid phone numbers and email addresses.</span></span>

<span data-ttu-id="65861-190">Questa iterazione, abbiamo aggiunto la logica di convalida all'applicazione Contact Manager nel modo più semplice possibile.</span><span class="sxs-lookup"><span data-stu-id="65861-190">In this iteration, we added the validation logic to our Contact Manager application in the easiest way possible.</span></span> <span data-ttu-id="65861-191">Tuttavia, combinare la logica di convalida nel nostro logica dei controller creerà i problemi per noi a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="65861-191">However, mixing our validation logic into our controller logic will create problems for us in the long term.</span></span> <span data-ttu-id="65861-192">L'applicazione sarà più difficile da gestire e modificare nel tempo.</span><span class="sxs-lookup"><span data-stu-id="65861-192">Our application will be more difficult to maintain and modify over time.</span></span>

<span data-ttu-id="65861-193">Nell'iterazione successiva, è eseguire il refactoring la logica di convalida e logica di accesso ai database all'esterno ai controller.</span><span class="sxs-lookup"><span data-stu-id="65861-193">In the next iteration, we will refactor our validation logic and database access logic out of our controllers.</span></span> <span data-ttu-id="65861-194">Daremo sfruttare alcuni principi di progettazione di software per abilitare la creazione di un'applicazione più strettamente collegata e più facilmente gestibile.</span><span class="sxs-lookup"><span data-stu-id="65861-194">We'll take advantage of several software design principles to enable us to create a more loosely coupled, and more maintainable, application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="65861-195">[Precedente](iteration-2-make-the-application-look-nice-vb.md)
> [Successivo](iteration-4-make-the-application-loosely-coupled-vb.md)</span><span class="sxs-lookup"><span data-stu-id="65861-195">[Previous](iteration-2-make-the-application-look-nice-vb.md)
[Next](iteration-4-make-the-application-loosely-coupled-vb.md)</span></span>