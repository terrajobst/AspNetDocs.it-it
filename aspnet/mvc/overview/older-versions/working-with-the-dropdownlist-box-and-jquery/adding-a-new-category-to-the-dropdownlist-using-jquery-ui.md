---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList mediante l'interfaccia utente di jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538836"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="f3fb8-102">Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery</span><span class="sxs-lookup"><span data-stu-id="f3fb8-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="f3fb8-103">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3fb8-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f3fb8-104">Il tag HTML `Select` è ideale per presentare un elenco di dati di categoria fissi, ma spesso è necessario aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="f3fb8-105">Si supponga di voler aggiungere il genere "opera" alle categorie nel database?</span><span class="sxs-lookup"><span data-stu-id="f3fb8-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="f3fb8-106">In questa sezione verrà usata l'interfaccia utente di jQuery per aggiungere una finestra di dialogo che è possibile usare per aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="f3fb8-107">Nell'immagine seguente viene illustrato il modo in cui l'interfaccia utente verrà visualizzata nel browser.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="f3fb8-108">Quando un utente seleziona il collegamento **Aggiungi nuovo genere** , viene visualizzata una finestra di dialogo popup in cui viene richiesto all'utente un nuovo nome del genere (e, facoltativamente, una descrizione).</span><span class="sxs-lookup"><span data-stu-id="f3fb8-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="f3fb8-109">Nell'immagine seguente viene mostrata la finestra di dialogo **Aggiungi genere** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="f3fb8-110">Quando viene immesso un nuovo nome di genere e viene effettuato il push del pulsante **Salva** , si verifica quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="f3fb8-111">Una chiamata AJAX invia i dati al metodo create del controller di genere, che salva il nuovo genere nel database e restituisce le nuove informazioni sul genere (nome e ID del genere) come JSON.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="f3fb8-112">JavaScript aggiunge i nuovi dati di genere all'elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="f3fb8-113">JavaScript rende il nuovo genere l'elemento selezionato.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="f3fb8-114">Nell'immagine seguente, **opera** è stato aggiunto al database e selezionato nell'elenco a discesa **genere** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="f3fb8-115">Aprire il file *Views\StoreManager\Create.cshtml* e sostituire il markup del genere con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="f3fb8-116">Il `_ChooseGenre` visualizzazione parziale conterrà tutta la logica per collegare JavaScript e jQuery usati per implementare la funzionalità Aggiungi nuovo genere.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="f3fb8-117">Una volta completato il codice, sarà semplice eseguire la stessa operazione con l'interfaccia utente dell'artista.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="f3fb8-118">In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella *Views\StoreManager* e selezionare **Aggiungi**, quindi **Visualizza**.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="f3fb8-119">Nell'input **nome visualizzazione** immettere `_ChooseGenre` quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="f3fb8-120">Sostituire il markup nel file *Views\StoreManager\\_ChooseGenre. cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="f3fb8-121">La prima riga dichiara che viene passato un `Album` come modello, esattamente la stessa istruzione del modello trovata nella visualizzazione create.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="f3fb8-122">Le righe successive sono il markup dell'helper **Label** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="f3fb8-123">La riga successiva è la chiamata di helper **DropDownList** , esattamente come nella visualizzazione di creazione originale.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="f3fb8-124">La riga successiva aggiunge un collegamento con il nome `Add New Genre`e lo stili come un pulsante.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="f3fb8-125">La riga contenente `ValidationMessageFor` viene copiata direttamente dalla vista Create.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="f3fb8-126">Le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="f3fb8-127">Crea un div nascosto, con l'ID del `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="f3fb8-128">Verrà usato jQuery per collegare la finestra di dialogo **Aggiungi genere** con l'ID `genreDialog` in questo div.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="f3fb8-129">Gli ultimi due tag script contengono i collegamenti ai file JavaScript che si utilizzeranno per implementare la funzionalità Aggiungi nuovo genere.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="f3fb8-130">Il file */Scripts/chooseGenre.js* viene fornito nel progetto, che verrà esaminato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="f3fb8-131">Eseguire l'applicazione e fare clic sul pulsante **Aggiungi nuovo genere** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="f3fb8-132">Nella finestra di dialogo **Aggiungi genere** immettere **opera** nella casella **nome** input.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="f3fb8-133">Fare clic sul pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-133">Click the **Save** button.</span></span> <span data-ttu-id="f3fb8-134">Una chiamata AJAX crea la categoria opera e quindi compila l'elenco a discesa con opera e imposta opera come genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="f3fb8-135">Immettere un artista, un titolo e un prezzo, quindi selezionare il pulsante **Crea** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="f3fb8-136">Se si immette un prezzo inferiore a $8,99, il nuovo album verrà visualizzato nella parte superiore della visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="f3fb8-137">Verificare che la voce del nuovo album sia stata salvata nel database.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="f3fb8-138">Provare a creare un nuovo genere con una sola lettera.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="f3fb8-139">Il codice seguente nel file *Models\Genre.cs* imposta la lunghezza minima e massima del nome del genere.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="f3fb8-140">Report di convalida lato client è necessario immettere una stringa di lunghezza compresa tra 2 e 20 caratteri.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="f3fb8-141">Esame del modo in cui un nuovo genere viene aggiunto al database e all'elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="f3fb8-142">Aprire il file *Scripts\chooseGenre.js* ed esaminare il codice.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="f3fb8-143">La seconda riga usa l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel file *Views\StoreManager\\_ChooseGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="f3fb8-144">La maggior parte dei parametri denominati è intuitiva.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="f3fb8-145">Il parametro `autoOpen` è impostato su false. Se si seleziona il pulsante **Crea genere** , il dialogo verrà aperto in modo esplicito (come descritto in questo secondo caso).</span><span class="sxs-lookup"><span data-stu-id="f3fb8-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="f3fb8-146">Nella finestra di dialogo sono presenti due pulsanti, **Save** e **Cancel**.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="f3fb8-147">Il pulsante **Annulla** chiude la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="f3fb8-148">Nel codice seguente viene illustrata la funzione del pulsante **Salva** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="f3fb8-149">Il `var createGenreForm` viene selezionato dall'ID del `createGenreForm`.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="f3fb8-150">L'ID `createGenreForm` è stato impostato nel codice seguente trovato nel file *Views\Genre\\_CreateGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="f3fb8-151">L'overload dell'helper [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) usato nel file *Views\Genre\\_CreateGenre. cshtml* genera il codice HTML con un attributo di azione contenente l'URL per l'invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="f3fb8-152">Per visualizzarlo, è possibile visualizzare la pagina Crea album in un browser e selezionare Mostra origine nel browser.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="f3fb8-153">Il markup seguente illustra il codice HTML generato contenente il tag form.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="f3fb8-154">La riga di `$.post` jQuery esegue una chiamata AJAX all'attributo Action (`/StoreManager/Create`) e passa i dati dalla finestra di dialogo **Crea genere** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="f3fb8-155">I dati sono costituiti dal nome del nuovo genere e da una descrizione facoltativa.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="f3fb8-156">Se la chiamata AJAX ha esito positivo, il nuovo nome e il valore del genere vengono aggiunti al markup Select e il nuovo genere viene impostato sul valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="f3fb8-157">Poiché si tratta di un markup generato in modo dinamico, non è possibile visualizzare la nuova opzione Select visualizzando l'origine nel browser.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="f3fb8-158">È possibile visualizzare il nuovo codice HTML con gli strumenti di sviluppo F12 di IE 9.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="f3fb8-159">Per visualizzare la nuova opzione Seleziona, in Internet Explorer 9 Premere il tasto F12 per avviare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="f3fb8-160">Passare alla pagina Create (Crea) e aggiungere un nuovo genere, in modo che il nuovo genere sia selezionato nell'elenco di selezione di genere.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="f3fb8-161">Negli strumenti di sviluppo F12:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="f3fb8-162">Selezionare la scheda HTML.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="f3fb8-163">Premere l'icona di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="f3fb8-164">Nella casella di ricerca immettere GenreID.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="f3fb8-165">Utilizzando l'icona avanti,</span><span class="sxs-lookup"><span data-stu-id="f3fb8-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="f3fb8-166">passare al tag SELECT seguente:</span><span class="sxs-lookup"><span data-stu-id="f3fb8-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="f3fb8-167">Espandere l'ultimo valore dell'opzione.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="f3fb8-168">Il codice seguente nel file *Scripts\chooseGenre.js* Mostra come il pulsante **Aggiungi nuovo genere** viene connesso all'evento click e come viene creata la finestra di dialogo **Aggiungi nuovo tipo** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="f3fb8-169">La prima riga crea una funzione click collegata al pulsante **Aggiungi nuovo genere** .</span><span class="sxs-lookup"><span data-stu-id="f3fb8-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="f3fb8-170">Il markup seguente dal file Views\StoreManager\\_ChooseGenre. cshtml Mostra come viene creato il pulsante **Aggiungi nuovo genere** :</span><span class="sxs-lookup"><span data-stu-id="f3fb8-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="f3fb8-171">Il metodo Load crea e apre la finestra di dialogo Aggiungi genere e chiama il metodo jQuery `parse` in modo che la convalida del client venga eseguita sui dati immessi nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="f3fb8-172">In questa sezione si è appreso come creare una finestra di dialogo che può essere usata per aggiungere nuovi dati di categoria a un elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="f3fb8-173">È possibile seguire la stessa procedura per creare un'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione dell'artista.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="f3fb8-174">In questa esercitazione è stata fornita una panoramica dell'uso del **DropDownList**helper HTML ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="f3fb8-175">Per ulteriori informazioni sull'utilizzo del **DropDownList**, vedere la sezione relativa ai riferimenti di addizione riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="f3fb8-176">Indica se questa esercitazione è stata utile.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="f3fb8-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="f3fb8-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="f3fb8-178">Riferimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="f3fb8-178">Additional References</span></span>

- <span data-ttu-id="f3fb8-179">[ASP.NET MVC – elenco a discesa a cascata-esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) di [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="f3fb8-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="f3fb8-180">[Scelta](https://harvesthq.github.com/chosen/) Un plug-in JavaScript che supporta la selezione e il filtraggio.</span><span class="sxs-lookup"><span data-stu-id="f3fb8-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="f3fb8-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="f3fb8-181">Contributors</span></span>

- [<span data-ttu-id="f3fb8-182">Enuca Radu</span><span class="sxs-lookup"><span data-stu-id="f3fb8-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="f3fb8-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="f3fb8-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="f3fb8-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="f3fb8-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="f3fb8-185">Revisori</span><span class="sxs-lookup"><span data-stu-id="f3fb8-185">Reviewers</span></span>

- <span data-ttu-id="f3fb8-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="f3fb8-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="f3fb8-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="f3fb8-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="f3fb8-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="f3fb8-188">Mike Pope</span></span>
- <span data-ttu-id="f3fb8-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="f3fb8-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f3fb8-190">Precedente</span><span class="sxs-lookup"><span data-stu-id="f3fb8-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
