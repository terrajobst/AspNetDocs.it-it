---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Analisi del modo in cui ASP.NET MVC supporta l'helper DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457609"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="ed778-102">Analisi della modalità di scaffolding dell'helper DropDownList in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ed778-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="ed778-103">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed778-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed778-104">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* e quindi scegliere **Aggiungi controller**.</span><span class="sxs-lookup"><span data-stu-id="ed778-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="ed778-105">Assegnare al controller il nome **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="ed778-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="ed778-106">Impostare le opzioni per la finestra di dialogo **Aggiungi controller** , come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="ed778-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="ed778-107">Modificare la visualizzazione *StoreManager\Index.cshtml* e rimuovere `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="ed778-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="ed778-108">La rimozione di `AlbumArtUrl` renderà la presentazione più leggibile.</span><span class="sxs-lookup"><span data-stu-id="ed778-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="ed778-109">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ed778-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="ed778-110">Aprire il file *Controllers\StoreManagerController.cs* e trovare il metodo `Index`.</span><span class="sxs-lookup"><span data-stu-id="ed778-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="ed778-111">Aggiungere la clausola `OrderBy` in modo che gli album verranno ordinati in base al prezzo.</span><span class="sxs-lookup"><span data-stu-id="ed778-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="ed778-112">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ed778-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="ed778-113">L'ordinamento in base al prezzo renderà più semplice testare le modifiche apportate al database.</span><span class="sxs-lookup"><span data-stu-id="ed778-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="ed778-114">Quando si esegue il test dei metodi di modifica e creazione, è possibile utilizzare un prezzo ridotto in modo che i dati salvati vengano visualizzati per primi.</span><span class="sxs-lookup"><span data-stu-id="ed778-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="ed778-115">Aprire il file *StoreManager\Edit.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ed778-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="ed778-116">Aggiungere la riga seguente subito dopo il tag della legenda.</span><span class="sxs-lookup"><span data-stu-id="ed778-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="ed778-117">Il codice seguente illustra il contesto di questa modifica:</span><span class="sxs-lookup"><span data-stu-id="ed778-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="ed778-118">Il `AlbumId` è necessario per apportare modifiche a un record di album.</span><span class="sxs-lookup"><span data-stu-id="ed778-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="ed778-119">Premere CTRL+F5 per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ed778-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="ed778-120">Selezionare il collegamento **amministratore** , quindi selezionare il collegamento **Crea nuovo** per creare un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="ed778-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="ed778-121">Verificare che le informazioni sull'album siano state salvate.</span><span class="sxs-lookup"><span data-stu-id="ed778-121">Verify the album information was saved.</span></span> <span data-ttu-id="ed778-122">Modificare un album e verificare che le modifiche apportate siano rese permanente.</span><span class="sxs-lookup"><span data-stu-id="ed778-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="ed778-123">Schema album</span><span class="sxs-lookup"><span data-stu-id="ed778-123">The Album Schema</span></span>

<span data-ttu-id="ed778-124">Il controller `StoreManager` creato dal meccanismo di impalcatura MVC consente l'accesso CRUD (creazione, lettura, aggiornamento, eliminazione) agli album nel database di Music Store.</span><span class="sxs-lookup"><span data-stu-id="ed778-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="ed778-125">Lo schema per le informazioni sugli album è riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed778-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="ed778-126">La tabella `Albums` non archivia il genere e la descrizione dell'album, quindi archivia una chiave esterna nella tabella `Genres`.</span><span class="sxs-lookup"><span data-stu-id="ed778-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="ed778-127">La tabella `Genres` contiene il nome del genere e la descrizione.</span><span class="sxs-lookup"><span data-stu-id="ed778-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="ed778-128">Analogamente, la tabella `Albums` non contiene il nome degli artisti degli album, ma una chiave esterna per la tabella `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ed778-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ed778-129">La tabella `Artists` contiene il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="ed778-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="ed778-130">Se si esaminano i dati nella tabella `Albums`, è possibile visualizzare ogni riga contenente una chiave esterna per la tabella `Genres` e una chiave esterna per la tabella `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ed778-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ed778-131">Nell'immagine seguente vengono illustrati alcuni dati della tabella della tabella `Albums`.</span><span class="sxs-lookup"><span data-stu-id="ed778-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="ed778-132">Tag di selezione HTML</span><span class="sxs-lookup"><span data-stu-id="ed778-132">The HTML Select Tag</span></span>

<span data-ttu-id="ed778-133">L'elemento HTML `<select>` (creato dall'helper [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) HTML) viene usato per visualizzare un elenco completo di valori, ad esempio l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="ed778-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="ed778-134">Per i moduli di modifica, quando il valore corrente è noto, l'elenco di selezione può visualizzare il valore corrente.</span><span class="sxs-lookup"><span data-stu-id="ed778-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="ed778-135">Questa operazione è stata illustrata in precedenza quando si imposta il valore selezionato su **commedia**.</span><span class="sxs-lookup"><span data-stu-id="ed778-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="ed778-136">L'elenco di selezione è ideale per la visualizzazione di dati di categoria o di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="ed778-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="ed778-137">L'elemento `<select>` per la chiave esterna genre Visualizza l'elenco dei nomi di genere possibili, ma quando si salva il modulo la proprietà Genre viene aggiornata con il valore di chiave esterna del genere e non con il nome del genere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="ed778-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="ed778-138">Nell'immagine seguente il genere selezionato è **discoteca** e l'artista è **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="ed778-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="ed778-139">Esame del codice con impalcature MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ed778-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="ed778-140">Aprire il file *Controllers\StoreManagerController.cs* e trovare il metodo `HTTP GET Create`.</span><span class="sxs-lookup"><span data-stu-id="ed778-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="ed778-141">Il metodo `Create` aggiunge due oggetti [selectin](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) al `ViewBag`, uno per contenere le informazioni sul genere e uno per contenere le informazioni sull'artista.</span><span class="sxs-lookup"><span data-stu-id="ed778-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="ed778-142">L'overload del costruttore [Selects](https://msdn.microsoft.com/library/dd505286.aspx) usato in precedenza accetta tre argomenti:</span><span class="sxs-lookup"><span data-stu-id="ed778-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="ed778-143">*Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenente gli elementi dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="ed778-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="ed778-144">Nell'esempio precedente, l'elenco dei generi restituiti da `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="ed778-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="ed778-145">*dataValueField*: nome della proprietà nell'elenco **IEnumerable** che contiene il valore della chiave.</span><span class="sxs-lookup"><span data-stu-id="ed778-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="ed778-146">Nell'esempio precedente, `GenreId` e `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="ed778-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="ed778-147">*TextField*: nome della proprietà nell'elenco **IEnumerable** che contiene le informazioni da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="ed778-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="ed778-148">Nella tabella Artists e genre viene usato il campo `name`.</span><span class="sxs-lookup"><span data-stu-id="ed778-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="ed778-149">Aprire il file *Views\StoreManager\Create.cshtml* ed esaminare il markup dell'helper `Html.DropDownList` per il campo Genre.</span><span class="sxs-lookup"><span data-stu-id="ed778-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="ed778-150">La prima riga indica che la visualizzazione crea accetta un modello di `Album`.</span><span class="sxs-lookup"><span data-stu-id="ed778-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="ed778-151">Nel metodo `Create` illustrato in precedenza, non è stato passato alcun modello, quindi la vista ottiene un modello di `Album` **null** .</span><span class="sxs-lookup"><span data-stu-id="ed778-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="ed778-152">A questo punto si sta creando un nuovo album, quindi non sono disponibili dati `Album`.</span><span class="sxs-lookup"><span data-stu-id="ed778-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="ed778-153">L'overload [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) illustrato in precedenza accetta il nome del campo da associare al modello.</span><span class="sxs-lookup"><span data-stu-id="ed778-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="ed778-154">USA inoltre questo nome per cercare un oggetto **ViewBag** contenente un oggetto [SELECT](https://msdn.microsoft.com/library/dd505286.aspx) .</span><span class="sxs-lookup"><span data-stu-id="ed778-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="ed778-155">Utilizzando questo overload, è necessario assegnare un nome all'oggetto **ViewBag selecting** `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="ed778-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="ed778-156">Il secondo parametro (`String.Empty`) è il testo da visualizzare quando non è selezionato alcun elemento.</span><span class="sxs-lookup"><span data-stu-id="ed778-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="ed778-157">Questo è esattamente ciò che si desidera quando si crea un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="ed778-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="ed778-158">Se il secondo parametro è stato rimosso ed è stato usato il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ed778-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="ed778-159">Per impostazione predefinita, l'elenco di selezione è il primo elemento o rock nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="ed778-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="ed778-160">Esame del metodo `HTTP POST Create`.</span><span class="sxs-lookup"><span data-stu-id="ed778-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="ed778-161">Questo overload del metodo `Create` accetta un oggetto `album`, creato dal sistema di associazione di modelli MVC ASP.NET dai valori del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="ed778-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="ed778-162">Quando si invia un nuovo album, se lo stato del modello è valido e non sono presenti errori di database, il nuovo album viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="ed778-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="ed778-163">Nell'immagine seguente viene illustrata la creazione di un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="ed778-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="ed778-164">È possibile utilizzare lo [strumento Fiddler](http://www.fiddler2.com/fiddler2/) per esaminare i valori del modulo inviati utilizzati dall'associazione di modelli MVC ASP.NET per creare l'oggetto album.</span><span class="sxs-lookup"><span data-stu-id="ed778-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="ed778-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ed778-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="ed778-166">Refactoring della creazione di ViewBag Select</span><span class="sxs-lookup"><span data-stu-id="ed778-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="ed778-167">Entrambi i metodi `Edit` e il metodo `HTTP POST Create` hanno codice identico per impostare l'oggetto **Select** nell'oggetto **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ed778-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ed778-168">In questo [spirito, si](http://en.wikipedia.org/wiki/Don't_repeat_yourself)effettuerà il refactoring di questo codice.</span><span class="sxs-lookup"><span data-stu-id="ed778-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="ed778-169">Il codice sottoposto a refactoring verrà usato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="ed778-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="ed778-170">Creare un nuovo metodo per aggiungere un tipo di oggetto **selezionato** per genere e artista a **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ed778-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="ed778-171">Sostituire le due righe impostando il `ViewBag` in ognuno dei metodi `Create` e `Edit` con una chiamata al metodo `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ed778-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="ed778-172">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ed778-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="ed778-173">Creare un nuovo album e modificare un album per verificare il corretto funzionamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="ed778-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="ed778-174">Passaggio esplicito dell'oggetto Select all'oggetto DropDownList</span><span class="sxs-lookup"><span data-stu-id="ed778-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="ed778-175">Per creare e modificare le visualizzazioni create dall'impalcatura MVC ASP.NET, usare l'overload di **DropDownList** seguente:</span><span class="sxs-lookup"><span data-stu-id="ed778-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="ed778-176">Di seguito è riportato il markup `DropDownList` per la vista Create.</span><span class="sxs-lookup"><span data-stu-id="ed778-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="ed778-177">Poiché la proprietà `ViewBag` per l'`SelectList` è denominata `GenreId`, l'helper **DropDownList** utilizzerà il `GenreId`**SELECT** nell'oggetto **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ed778-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ed778-178">Nell'overload di **DropDownList** seguente il `SelectList` viene passato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="ed778-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="ed778-179">Aprire il file *Views\StoreManager\Edit.cshtml* e modificare la chiamata a **DropDownList** per passare in modo esplicito all'oggetto **Select**, usando l'overload riportato sopra.</span><span class="sxs-lookup"><span data-stu-id="ed778-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="ed778-180">Eseguire questa operazione per la categoria Genre.</span><span class="sxs-lookup"><span data-stu-id="ed778-180">Do this for the Genre category.</span></span> <span data-ttu-id="ed778-181">Il codice completato è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed778-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="ed778-182">Eseguire l'applicazione e fare clic sul collegamento **admin** , quindi passare a un album jazz e selezionare il collegamento **Edit (modifica** ).</span><span class="sxs-lookup"><span data-stu-id="ed778-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="ed778-183">Invece di visualizzare jazz come il genere attualmente selezionato, viene visualizzato Rock.</span><span class="sxs-lookup"><span data-stu-id="ed778-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="ed778-184">Quando l'argomento di stringa (la proprietà da associare) e l'oggetto **selector** hanno lo stesso nome, il valore selezionato non viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="ed778-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="ed778-185">Quando non viene specificato alcun valore selezionato, per impostazione predefinita i browser sono il primo elemento dell'oggetto **Select**(che è **Rock** nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="ed778-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="ed778-186">Si tratta di un limite noto dell'helper **DropDownList** .</span><span class="sxs-lookup"><span data-stu-id="ed778-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="ed778-187">Aprire il file *Controllers\StoreManagerController.cs* e modificare i nomi degli oggetti **selezionati** in `Genres` e `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ed778-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="ed778-188">Il codice completato è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed778-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="ed778-189">I nomi generici e artisti sono nomi migliori per le categorie, perché contengono più di un ID di ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="ed778-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="ed778-190">Il refactoring è stato precedentemente pagato.</span><span class="sxs-lookup"><span data-stu-id="ed778-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="ed778-191">Anziché modificare il **ViewBag** in quattro metodi, le modifiche sono state isolate con il metodo `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ed778-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="ed778-192">Modificare la chiamata **DropDownList** nelle viste create e Edit per utilizzare i nuovi nomi di **selezione** .</span><span class="sxs-lookup"><span data-stu-id="ed778-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="ed778-193">Il nuovo markup per la visualizzazione di modifica è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed778-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="ed778-194">La vista Create richiede una stringa vuota per impedire la visualizzazione del primo elemento dell'oggetto Select.</span><span class="sxs-lookup"><span data-stu-id="ed778-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="ed778-195">Creare un nuovo album e modificare un album per verificare il corretto funzionamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="ed778-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="ed778-196">Testare il codice di modifica selezionando un album con un genere diverso da Rock.</span><span class="sxs-lookup"><span data-stu-id="ed778-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="ed778-197">Uso di un modello di visualizzazione con l'helper DropDownList</span><span class="sxs-lookup"><span data-stu-id="ed778-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="ed778-198">Creare una nuova classe nella cartella ViewModels denominata `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ed778-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="ed778-199">Sostituire il codice nella classe `AlbumSelectListViewModel` con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ed778-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="ed778-200">Il costruttore `AlbumSelectListViewModel` accetta un album, un elenco di artisti e generi e crea un oggetto contenente l'album e un `SelectList` per i generi e gli artisti.</span><span class="sxs-lookup"><span data-stu-id="ed778-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="ed778-201">Compilare il progetto in modo che il `AlbumSelectListViewModel` sia disponibile quando si crea una vista nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="ed778-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="ed778-202">Aggiungere un metodo di `EditVM` al `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="ed778-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="ed778-203">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ed778-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="ed778-204">Fare clic con il pulsante destro del mouse su `AlbumSelectListViewModel`, scegliere **Risolvi**e quindi **usare MvcMusicStore. ViewModels;** .</span><span class="sxs-lookup"><span data-stu-id="ed778-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="ed778-205">In alternativa, è possibile aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="ed778-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="ed778-206">Fare clic con il pulsante destro del mouse `EditVM` e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="ed778-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="ed778-207">Usare le opzioni illustrate di seguito.</span><span class="sxs-lookup"><span data-stu-id="ed778-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="ed778-208">Selezionare **Aggiungi**, quindi sostituire il contenuto del file *Views\StoreManager\EditVM.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ed778-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="ed778-209">Il markup `EditVM` è molto simile al markup di `Edit` originale con le eccezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="ed778-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="ed778-210">Le proprietà del modello nella vista `Edit` sono nel formato `model.property`, ad esempio `model.Title`.</span><span class="sxs-lookup"><span data-stu-id="ed778-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="ed778-211">Le proprietà del modello nella vista `EditVm` sono nel formato `model.Album.property`, ad esempio `model.Album.Title`.</span><span class="sxs-lookup"><span data-stu-id="ed778-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="ed778-212">Questo perché alla vista `EditVM` viene passato un contenitore per un `Album`, non un `Album` come nella visualizzazione `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ed778-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="ed778-213">Il secondo parametro **DropDownList** deriva dal modello di visualizzazione, non da **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ed778-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="ed778-214">L'helper **BeginForm** nella visualizzazione `EditVM` esegue in modo esplicito il postback al metodo di azione `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ed778-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="ed778-215">Eseguendo il postback all'azione `Edit`, non è necessario scrivere un'azione `HTTP POST EditVM` ed è possibile riutilizzare la `HTTP POST` azione `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ed778-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="ed778-216">Eseguire l'applicazione e modificare un album.</span><span class="sxs-lookup"><span data-stu-id="ed778-216">Run the application and edit an album.</span></span> <span data-ttu-id="ed778-217">Modificare l'URL per utilizzare `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="ed778-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="ed778-218">Modificare un campo e premere il pulsante **Salva** per verificare il corretto funzionamento del codice.</span><span class="sxs-lookup"><span data-stu-id="ed778-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="ed778-219">Quale approccio utilizzare?</span><span class="sxs-lookup"><span data-stu-id="ed778-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="ed778-220">Tutti e tre gli approcci illustrati sono accettabili.</span><span class="sxs-lookup"><span data-stu-id="ed778-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="ed778-221">Molti sviluppatori preferiscono passare esplicitamente il `SelectList` al `DropDownList` utilizzando il `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ed778-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="ed778-222">Questo approccio offre l'ulteriore vantaggio di offrire la flessibilità di usare un nome più appropriato per la raccolta.</span><span class="sxs-lookup"><span data-stu-id="ed778-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="ed778-223">Un avvertimento è che non è possibile denominare l'oggetto `ViewBag SelectList` con lo stesso nome della proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="ed778-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="ed778-224">Alcuni sviluppatori preferiscono l'approccio ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ed778-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="ed778-225">Altri considerano il markup più dettagliato e il codice HTML generato dell'approccio ViewModel uno svantaggio.</span><span class="sxs-lookup"><span data-stu-id="ed778-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="ed778-226">In questa sezione sono stati appresi tre approcci all'uso di **DropDownList** con i dati di categoria.</span><span class="sxs-lookup"><span data-stu-id="ed778-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="ed778-227">Nella sezione successiva verrà illustrato come aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="ed778-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed778-228">[Precedente](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Successivo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="ed778-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
