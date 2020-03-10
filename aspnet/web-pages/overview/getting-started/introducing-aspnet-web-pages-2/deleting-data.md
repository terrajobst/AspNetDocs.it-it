---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introduzione a Pagine Web ASP.NET-eliminazione dei dati del database | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione viene illustrato come eliminare una singola voce di database. Si presuppone che la serie sia stata completata tramite l'aggiornamento dei dati del database in ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629038"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="dda65-104">Introduzione a Pagine Web ASP.NET-eliminazione dei dati del database</span><span class="sxs-lookup"><span data-stu-id="dda65-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="dda65-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dda65-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dda65-106">In questa esercitazione viene illustrato come eliminare una singola voce di database.</span><span class="sxs-lookup"><span data-stu-id="dda65-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="dda65-107">Si presuppone che la serie sia stata completata tramite l' [aggiornamento dei dati del database in pagine Web ASP.NET](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="dda65-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="dda65-108">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="dda65-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="dda65-109">Come selezionare un singolo record da un elenco di record.</span><span class="sxs-lookup"><span data-stu-id="dda65-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="dda65-110">Come eliminare un singolo record da un database.</span><span class="sxs-lookup"><span data-stu-id="dda65-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="dda65-111">Come verificare che sia stato fatto clic su un pulsante specifico in un modulo.</span><span class="sxs-lookup"><span data-stu-id="dda65-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="dda65-112">Funzionalità/tecnologie discusse:</span><span class="sxs-lookup"><span data-stu-id="dda65-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="dda65-113">Helper `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="dda65-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="dda65-114">Comando SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="dda65-115">Metodo `Database.Execute` per eseguire un comando SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="dda65-116">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="dda65-116">What You'll Build</span></span>

<span data-ttu-id="dda65-117">Nell'esercitazione precedente si è appreso come aggiornare un record di database esistente.</span><span class="sxs-lookup"><span data-stu-id="dda65-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="dda65-118">Questa esercitazione è simile, ad eccezione del fatto che anziché aggiornare il record, sarà possibile eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="dda65-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="dda65-119">I processi sono molto uguali, ad eccezione del fatto che l'eliminazione è più semplice, quindi questa esercitazione sarà breve.</span><span class="sxs-lookup"><span data-stu-id="dda65-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="dda65-120">Nella pagina *Movies* si aggiornerà l'helper `WebGrid` in modo che visualizzi un collegamento **Delete** accanto a ogni film per accompagnare il collegamento di **modifica** aggiunto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="dda65-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Pagina dei film che mostra un collegamento Delete per ogni film](deleting-data/_static/image1.png)

<span data-ttu-id="dda65-122">Come per la modifica, quando si fa clic sul collegamento **Elimina** si passa a una pagina diversa, in cui le informazioni sul film sono già in un formato:</span><span class="sxs-lookup"><span data-stu-id="dda65-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Elimina la pagina di film con un film visualizzato](deleting-data/_static/image2.png)

<span data-ttu-id="dda65-124">È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.</span><span class="sxs-lookup"><span data-stu-id="dda65-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="dda65-125">Aggiunta di un collegamento Delete all'elenco di film</span><span class="sxs-lookup"><span data-stu-id="dda65-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="dda65-126">Per iniziare, aggiungere un collegamento **Delete** all'helper `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="dda65-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="dda65-127">Questo collegamento è simile al collegamento **Edit** aggiunto in un'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="dda65-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="dda65-128">Aprire il file *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="dda65-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="dda65-129">Modificare il markup `WebGrid` nel corpo della pagina aggiungendo una colonna.</span><span class="sxs-lookup"><span data-stu-id="dda65-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="dda65-130">Ecco il markup modificato:</span><span class="sxs-lookup"><span data-stu-id="dda65-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="dda65-131">La nuova colonna è la seguente:</span><span class="sxs-lookup"><span data-stu-id="dda65-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="dda65-132">Il modo in cui la griglia è configurata, la colonna di **modifica** è più a sinistra nella griglia e la colonna **Delete** è più a destra.</span><span class="sxs-lookup"><span data-stu-id="dda65-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="dda65-133">(Dopo la `Year` colonna è presente una virgola, nel caso in cui non sia stato notato). Non c'è niente di speciale sulla posizione in cui le colonne di collegamento sono disponibili ed è possibile inserirle facilmente tra loro.</span><span class="sxs-lookup"><span data-stu-id="dda65-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="dda65-134">In questo caso, sono separate per renderle più difficili da combinare.</span><span class="sxs-lookup"><span data-stu-id="dda65-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Pagina dei filmati con collegamenti di modifica e dettagli contrassegnati per indicare che non sono vicini tra loro](deleting-data/_static/image3.png)

<span data-ttu-id="dda65-136">La nuova colonna Mostra un collegamento (`<a>` elemento) il cui testo indica "Delete".</span><span class="sxs-lookup"><span data-stu-id="dda65-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="dda65-137">La destinazione del collegamento (il relativo attributo `href`) è il codice che in definitiva viene risolto in un elemento come questo URL, con il valore `id` diverso per ogni film:</span><span class="sxs-lookup"><span data-stu-id="dda65-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="dda65-138">Questo collegamento richiama una pagina denominata *DeleteMovie* e la passa all'ID del film selezionato.</span><span class="sxs-lookup"><span data-stu-id="dda65-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="dda65-139">In questa esercitazione non verrà illustrato in dettaglio come viene costruito questo collegamento, perché è quasi identico al collegamento **Edit** dell'esercitazione precedente ([aggiornamento dei dati del database in pagine Web ASP.NET](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="dda65-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="dda65-140">Creazione della pagina Elimina</span><span class="sxs-lookup"><span data-stu-id="dda65-140">Creating the Delete Page</span></span>

<span data-ttu-id="dda65-141">A questo punto è possibile creare la pagina che sarà la destinazione per il collegamento **Elimina** nella griglia.</span><span class="sxs-lookup"><span data-stu-id="dda65-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="dda65-142">**Importante** La tecnica di selezionare prima un record da eliminare e quindi usare una pagina e un pulsante separati per confermare che il processo è estremamente importante per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="dda65-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="dda65-143">Come è stato letto nelle esercitazioni precedenti, l'esecuzione di *qualsiasi* modifica al sito web dovrebbe essere *sempre* eseguita usando un modulo &mdash;, ovvero usando un'operazione HTTP post.</span><span class="sxs-lookup"><span data-stu-id="dda65-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="dda65-144">Se è possibile modificare il sito facendo semplicemente clic su un collegamento (ovvero usando un'operazione GET), è possibile che gli utenti possano effettuare richieste semplici al sito ed eliminare i dati.</span><span class="sxs-lookup"><span data-stu-id="dda65-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="dda65-145">Anche un crawler del motore di ricerca che indicizza il sito potrebbe eliminare inavvertitamente i dati solo con i collegamenti seguenti.</span><span class="sxs-lookup"><span data-stu-id="dda65-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="dda65-146">Quando l'app consente agli utenti di modificare un record, è comunque necessario presentare il record all'utente per la modifica.</span><span class="sxs-lookup"><span data-stu-id="dda65-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="dda65-147">Tuttavia, si potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record.</span><span class="sxs-lookup"><span data-stu-id="dda65-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="dda65-148">Tuttavia, non ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="dda65-148">Don't skip that step, though.</span></span> <span data-ttu-id="dda65-149">È anche utile per gli utenti visualizzare il record e confermare che sta eliminando il record che ha voluto.</span><span class="sxs-lookup"><span data-stu-id="dda65-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="dda65-150">In un set di esercitazioni successivo si vedrà come aggiungere la funzionalità di accesso per consentire a un utente di accedere prima di eliminare un record.</span><span class="sxs-lookup"><span data-stu-id="dda65-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="dda65-151">Creare una pagina denominata *DeleteMovie. cshtml* e sostituire gli elementi del file con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="dda65-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="dda65-152">Questo markup è analogo alle pagine *EditMovie* , ad eccezione del fatto che anziché usare caselle di testo (`<input type="text">`), il markup include `<span>` elementi.</span><span class="sxs-lookup"><span data-stu-id="dda65-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="dda65-153">Nessun elemento da modificare.</span><span class="sxs-lookup"><span data-stu-id="dda65-153">There's nothing here to edit.</span></span> <span data-ttu-id="dda65-154">È sufficiente visualizzare i dettagli dei film in modo che gli utenti possano assicurarsi che stiano eliminando il film appropriato.</span><span class="sxs-lookup"><span data-stu-id="dda65-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="dda65-155">Il markup contiene già un collegamento che consente all'utente di tornare alla pagina dell'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="dda65-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="dda65-156">Come nella pagina *EditMovie* , l'ID del film selezionato viene archiviato in un campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="dda65-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="dda65-157">Viene passato nella pagina nella prima posizione come valore della stringa di query. Esiste una chiamata `Html.ValidationSummary` che visualizzerà gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="dda65-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="dda65-158">In questo caso, l'errore potrebbe essere che nessun ID film è stato passato alla pagina o che l'ID film non è valido.</span><span class="sxs-lookup"><span data-stu-id="dda65-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="dda65-159">Questa situazione può verificarsi se qualcuno ha eseguito questa pagina senza prima selezionare un film nella pagina dei *filmati* .</span><span class="sxs-lookup"><span data-stu-id="dda65-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="dda65-160">La didascalia del pulsante è **Delete Movie**e il relativo attributo name è impostato su `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="dda65-161">L'attributo `name` verrà usato nel codice per identificare il pulsante che ha inviato il form.</span><span class="sxs-lookup"><span data-stu-id="dda65-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="dda65-162">È necessario scrivere il codice su 1) leggere i dettagli dei film quando la pagina viene visualizzata per la prima volta e 2) eliminare effettivamente il film quando l'utente fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="dda65-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="dda65-163">Aggiunta di codice per la lettura di un singolo film</span><span class="sxs-lookup"><span data-stu-id="dda65-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="dda65-164">Nella parte superiore della pagina *DeleteMovie. cshtml* aggiungere il blocco di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dda65-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="dda65-165">Questo markup corrisponde al codice corrispondente nella pagina *EditMovie* .</span><span class="sxs-lookup"><span data-stu-id="dda65-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="dda65-166">Ottiene l'ID del film dalla stringa di query e usa l'ID per leggere un record dal database.</span><span class="sxs-lookup"><span data-stu-id="dda65-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="dda65-167">Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID film passato alla pagina sia valido.</span><span class="sxs-lookup"><span data-stu-id="dda65-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="dda65-168">Tenere presente che questo codice deve essere eseguito solo la prima volta che la pagina viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="dda65-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="dda65-169">Non si vuole rileggere il record del film dal database quando l'utente fa clic sul pulsante **Elimina film** .</span><span class="sxs-lookup"><span data-stu-id="dda65-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="dda65-170">Pertanto, il codice per leggere il film si trova all'interno di un test che afferma `if(!IsPost)` &mdash;, *se la richiesta non è un'operazione post (invio del modulo)* .</span><span class="sxs-lookup"><span data-stu-id="dda65-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="dda65-171">Aggiunta del codice per eliminare il film selezionato</span><span class="sxs-lookup"><span data-stu-id="dda65-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="dda65-172">Per eliminare il film quando l'utente fa clic sul pulsante, aggiungere il codice seguente solo all'interno della parentesi graffa di chiusura del blocco di `@`:</span><span class="sxs-lookup"><span data-stu-id="dda65-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="dda65-173">Questo codice è simile al codice per l'aggiornamento di un record esistente, ma più semplice.</span><span class="sxs-lookup"><span data-stu-id="dda65-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="dda65-174">Il codice esegue fondamentalmente un'istruzione SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="dda65-175">Come nella pagina *EditMovie* , il codice si trova in un blocco `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="dda65-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="dda65-176">Questa volta, la condizione `if()` è leggermente più complessa:</span><span class="sxs-lookup"><span data-stu-id="dda65-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="dda65-177">Esistono due condizioni.</span><span class="sxs-lookup"><span data-stu-id="dda65-177">There are two conditions here.</span></span> <span data-ttu-id="dda65-178">Il primo consiste nel fatto che la pagina viene inviata, come si è visto prima &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="dda65-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="dda65-179">La seconda condizione è `!Request["buttonDelete"].IsEmpty()`, ovvero la richiesta dispone di un oggetto denominato `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="dda65-180">Si tratta di un modo indiretto per testare il pulsante che ha inviato il modulo.</span><span class="sxs-lookup"><span data-stu-id="dda65-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="dda65-181">Se un form contiene più pulsanti Submit, nella richiesta viene visualizzato solo il nome del pulsante su cui è stato fatto clic.</span><span class="sxs-lookup"><span data-stu-id="dda65-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="dda65-182">Pertanto, logicamente, se il nome di un pulsante particolare viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuoto &mdash; che è il pulsante che ha inviato il form.</span><span class="sxs-lookup"><span data-stu-id="dda65-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="dda65-183">L'operatore `&&` significa "and" (AND logico).</span><span class="sxs-lookup"><span data-stu-id="dda65-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="dda65-184">Quindi, l'intera condizione di `if` è...</span><span class="sxs-lookup"><span data-stu-id="dda65-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="dda65-185">*Questa richiesta è post (non è una richiesta per la prima volta)*</span><span class="sxs-lookup"><span data-stu-id="dda65-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="dda65-186">AND</span><span class="sxs-lookup"><span data-stu-id="dda65-186">AND</span></span>  
  
<span data-ttu-id="dda65-187">*Il* *pulsante `buttonDelete`è il pulsante che ha inviato il form.*</span><span class="sxs-lookup"><span data-stu-id="dda65-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="dda65-188">Questo modulo (infatti, questa pagina) contiene un solo pulsante, quindi il test aggiuntivo per `buttonDelete` non è tecnicamente necessario.</span><span class="sxs-lookup"><span data-stu-id="dda65-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="dda65-189">Tuttavia, si sta per eseguire un'operazione che eliminerà definitivamente i dati.</span><span class="sxs-lookup"><span data-stu-id="dda65-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="dda65-190">Quindi, si vuole essere sicuri che si sta eseguendo l'operazione solo quando l'utente lo ha richiesto in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="dda65-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="dda65-191">Si supponga, ad esempio, che la pagina sia stata espansa in un secondo momento e che siano stati aggiunti altri pulsanti.</span><span class="sxs-lookup"><span data-stu-id="dda65-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="dda65-192">Anche in questo caso, il codice che elimina il film verrà eseguito solo se è stato fatto clic sul pulsante `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="dda65-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="dda65-193">Come nella pagina *EditMovie* , si ottiene l'ID dal campo nascosto, quindi si esegue il comando SQL.</span><span class="sxs-lookup"><span data-stu-id="dda65-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="dda65-194">La sintassi per l'istruzione `Delete` è la seguente:</span><span class="sxs-lookup"><span data-stu-id="dda65-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="dda65-195">È essenziale includere la clausola `WHERE` e l'ID.</span><span class="sxs-lookup"><span data-stu-id="dda65-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="dda65-196">Se si omette la clausola WHERE, *verranno eliminati tutti i record della tabella*.</span><span class="sxs-lookup"><span data-stu-id="dda65-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="dda65-197">Come si è visto, il valore ID viene passato al comando SQL utilizzando un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="dda65-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="dda65-198">Test del processo di eliminazione film</span><span class="sxs-lookup"><span data-stu-id="dda65-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="dda65-199">A questo punto è possibile eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="dda65-199">Now you can test.</span></span> <span data-ttu-id="dda65-200">Eseguire la pagina *Movies* , quindi fare clic su **Delete** accanto a un film.</span><span class="sxs-lookup"><span data-stu-id="dda65-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="dda65-201">Quando viene visualizzata la pagina *DeleteMovie* , fare clic su **Elimina filmato**.</span><span class="sxs-lookup"><span data-stu-id="dda65-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Elimina pagina film con pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

<span data-ttu-id="dda65-203">Quando si fa clic sul pulsante, il codice elimina i film e torna all'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="dda65-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="dda65-204">Qui è possibile cercare il film eliminato e verificare che sia stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="dda65-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="dda65-205">Prossimi</span><span class="sxs-lookup"><span data-stu-id="dda65-205">Coming Up Next</span></span>

<span data-ttu-id="dda65-206">L'esercitazione successiva illustra come assegnare a tutte le pagine del sito un aspetto e un layout comuni.</span><span class="sxs-lookup"><span data-stu-id="dda65-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="dda65-207">Elenco completo per la pagina di film (aggiornato con i collegamenti di eliminazione)</span><span class="sxs-lookup"><span data-stu-id="dda65-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="dda65-208">Elenco completo per la pagina DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="dda65-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="dda65-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dda65-209">Additional Resources</span></span>

- [<span data-ttu-id="dda65-210">Introduzione alla programmazione Web di ASP.NET tramite la sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="dda65-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="dda65-211">[Istruzione SQL DELETE](http://www.w3schools.com/sql/sql_delete.asp) nel sito di W3Schools</span><span class="sxs-lookup"><span data-stu-id="dda65-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dda65-212">[Precedente](updating-data.md)
> [Successivo](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="dda65-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
