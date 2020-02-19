---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunta di un nuovo campo al modello di filmato e alla tabella | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457700"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="a3cf0-104">Aggiunta di un nuovo campo al modello di filmato e alla tabella</span><span class="sxs-lookup"><span data-stu-id="a3cf0-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="a3cf0-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3cf0-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="a3cf0-106">Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="a3cf0-107">È più sicuro, molto più semplice da seguire e illustra altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="a3cf0-108">In questa sezione si utilizzerà Migrazioni Code First di Entity Framework per eseguire la migrazione di alcune modifiche alle classi del modello in modo da applicare la modifica al database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="a3cf0-109">Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="a3cf0-110">Se non sono sincronizzati, il Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="a3cf0-111">In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="a3cf0-112">Impostazione Migrazioni Code First per le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="a3cf0-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="a3cf0-113">Se si usa Visual Studio 2012, fare doppio clic sul file *Movies. MDF* da Esplora soluzioni per aprire lo strumento database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="a3cf0-114">Visual Studio Express per il Web mostrerà Esplora database, Visual Studio 2012 visualizzerà Esplora server.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="a3cf0-115">Se si usa Visual Studio 2010, usare Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="a3cf0-116">Nello strumento database (Esplora database, Esplora server o Esplora oggetti di SQL Server), fare clic con il pulsante destro del mouse su `MovieDBContext` e selezionare **Elimina** per eliminare il database Movies.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="a3cf0-117">Tornare a Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="a3cf0-118">Fare clic con il pulsante destro del mouse sul file *Movies. MDF* e selezionare **Elimina** per rimuovere il database Movies.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="a3cf0-119">Compilare l'applicazione e verificare che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="a3cf0-120">Nel menu **Strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Aggiungi Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="a3cf0-122">Nella finestra **console di gestione pacchetti** al prompt dei `PM>` immettere "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="a3cf0-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="a3cf0-123">Il comando **Enable-Migrations** (illustrato in precedenza) consente di creare un file *Configuration.cs* in una nuova cartella *Migrations* .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="a3cf0-124">Visual Studio apre il file *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="a3cf0-125">Sostituire il metodo `Seed` nel file *Configuration.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="a3cf0-126">Fare clic con il pulsante destro del mouse sulla riga rossa ondulata sotto `Movie` e selezionare **Risolvi** e quindi **usare** **MvcMovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="a3cf0-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="a3cf0-127">Questa operazione consente di aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="a3cf0-128">Migrazioni Code First chiama il metodo `Seed` dopo ogni migrazione (ovvero, chiamando **Update-database** nella console di gestione pacchetti) e questo metodo aggiorna le righe che sono già state inserite oppure le inserisce se non esistono ancora.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="a3cf0-129">**Premere CTRL + MAIUSC + B per compilare il progetto.** I passaggi seguenti avranno esito negativo se non si compila a questo punto.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="a3cf0-130">Il passaggio successivo consiste nel creare una classe `DbMigration` per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="a3cf0-131">Questa migrazione a crea un nuovo database, motivo per cui è stato eliminato il file *Movie. MDF* in un passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="a3cf0-132">Nella finestra **console di gestione pacchetti** immettere il comando "Aggiungi-migrazione iniziale" per creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="a3cf0-133">Il nome "Initial" è arbitrario e viene usato per assegnare un nome al file di migrazione creato.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="a3cf0-134">Migrazioni Code First crea un altro file di classe nella cartella *Migrations* (denominato *{DateStamp}\_Initial.cs* ) e questa classe contiene il codice che crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="a3cf0-135">Il nome del file di migrazione è preceduto da un timestamp per facilitare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="a3cf0-136">Esaminare il file *{datestamp}\_Initial.cs* , che contiene le istruzioni per creare la tabella Movies per il database di film.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="a3cf0-137">Quando si aggiorna il database nelle istruzioni seguenti, questo file di *{datestamp}\_Initial.cs* verrà eseguito e creerà lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="a3cf0-138">Viene quindi eseguito il metodo **Seed** per popolare il database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="a3cf0-139">Nella **console di gestione pacchetti**immettere il comando "Update-database" per creare il database ed eseguire il metodo **Seed** .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="a3cf0-140">Se viene generato un errore che indica che una tabella esiste già e non può essere creata, è probabile che l'applicazione sia stata eseguita dopo l'eliminazione del database e prima dell'esecuzione `update-database`.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="a3cf0-141">In tal caso, eliminare nuovamente il file *Movies. MDF* , quindi riprovare a `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="a3cf0-142">Se viene comunque ricevuto un errore, eliminare la cartella migrazioni e il contenuto, quindi iniziare con le istruzioni nella parte superiore di questa pagina, ovvero eliminare il file *Movies. MDF* , quindi passare a Enable-Migrations.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="a3cf0-143">Eseguire l'applicazione e passare all'URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a3cf0-144">Vengono visualizzati i dati di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="a3cf0-145">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="a3cf0-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="a3cf0-146">Per iniziare, aggiungere una nuova proprietà `Rating` alla classe `Movie` esistente.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="a3cf0-147">Aprire il file *Models\Movie.cs* e aggiungere la proprietà `Rating` come questa:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="a3cf0-148">La classe `Movie` completa ora è simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="a3cf0-149">Compilare l'applicazione usando il comando di menu **compila** &gt;**Compila film** oppure premendo CTRL + MAIUSC + B.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="a3cf0-150">Ora che è stata aggiornata la classe `Model`, è necessario aggiornare anche i modelli di visualizzazione *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* per visualizzare la nuova proprietà `Rating` nella visualizzazione del browser.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="a3cf0-151">Aprire il file<em>\Views\Movies\Index.cshtml</em> e aggiungere un `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna <strong>Price</strong> .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="a3cf0-152">Aggiungere quindi una colonna `<td>` alla fine del modello per eseguire il rendering del valore `@item.Rating`.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="a3cf0-153">Di seguito è riportato il modello di visualizzazione <em>index. cshtml</em> aggiornato:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="a3cf0-154">Successivamente, aprire il file *\Views\Movies\Create.cshtml* e aggiungere il markup seguente alla fine del modulo.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="a3cf0-155">Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="a3cf0-156">A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova proprietà `Rating`.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="a3cf0-157">A questo punto, eseguire l'applicazione e passare all'URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a3cf0-158">Quando si esegue questa operazione, tuttavia, verrà visualizzato uno degli errori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="a3cf0-159">Questo errore viene visualizzato perché la classe del modello di `Movie` aggiornata nell'applicazione è ora diversa dallo schema della tabella `Movie` del database esistente.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="a3cf0-160">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="a3cf0-161">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="a3cf0-162">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="a3cf0-163">Questo approccio è molto utile quando si esegue lo sviluppo attivo su un database di prova. consente di sviluppare rapidamente lo schema del modello e del database insieme.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="a3cf0-164">Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="a3cf0-165">L'utilizzo di un inizializzatore per eseguire automaticamente il seeding di un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="a3cf0-166">Per altre informazioni sugli inizializzatori di database Entity Framework, vedere l' [esercitazione ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="a3cf0-167">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="a3cf0-168">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="a3cf0-169">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="a3cf0-170">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="a3cf0-171">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="a3cf0-172">Aggiornare il metodo seed in modo da fornire un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="a3cf0-173">Aprire il file Migrations\Configuration.cs e aggiungere un campo rating a ogni oggetto Movie.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="a3cf0-174">Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="a3cf0-175">Il `add-migration` comando indica al Framework di migrazione di esaminare il modello di film corrente con lo schema del database del film corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="a3cf0-176">AddRatingMig è arbitrario e viene usato per assegnare un nome al file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="a3cf0-177">È utile usare un nome significativo per il passaggio di migrazione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="a3cf0-178">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe derivata `DbMigration` e nel metodo `Up` è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="a3cf0-179">Compilare la soluzione, quindi immettere il comando "Update-database" nella finestra **console di gestione pacchetti** .</span><span class="sxs-lookup"><span data-stu-id="a3cf0-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="a3cf0-180">Nell'immagine seguente viene illustrato l'output nella finestra della **console di gestione pacchetti** (il timbro di data anteposto AddRatingMig sarà diverso).</span><span class="sxs-lookup"><span data-stu-id="a3cf0-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="a3cf0-181">Eseguire di nuovo l'applicazione e passare all'URL/Movies.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="a3cf0-182">È possibile visualizzare il nuovo campo rating.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="a3cf0-183">Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="a3cf0-184">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="a3cf0-186">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-186">Click **Create**.</span></span> <span data-ttu-id="a3cf0-187">Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:</span><span class="sxs-lookup"><span data-stu-id="a3cf0-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="a3cf0-189">È anche necessario aggiungere il campo `Rating` ai modelli di vista edit, Details e SearchIndex.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="a3cf0-190">È possibile immettere di nuovo il comando "Update-database" nella finestra della **console di gestione pacchetti** e non verrà apportata alcuna modifica, perché lo schema corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="a3cf0-191">In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="a3cf0-192">È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="a3cf0-193">Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.</span><span class="sxs-lookup"><span data-stu-id="a3cf0-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3cf0-194">[Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="a3cf0-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
