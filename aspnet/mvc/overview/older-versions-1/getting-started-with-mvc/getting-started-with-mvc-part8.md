---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543582"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="e945b-104">Aggiunta di una colonna al modello</span><span class="sxs-lookup"><span data-stu-id="e945b-104">Adding a Column to the Model</span></span>

<span data-ttu-id="e945b-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e945b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="e945b-106">Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e945b-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="e945b-107">Verrà creata una semplice applicazione Web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="e945b-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="e945b-108">Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e945b-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="e945b-109">In questa sezione verrà illustrato in dettaglio come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e945b-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="e945b-110">Aggiungere una colonna "rating" alla tabella Movie.</span><span class="sxs-lookup"><span data-stu-id="e945b-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="e945b-111">Tornare all'IDE e fare clic sull'Esplora database.</span><span class="sxs-lookup"><span data-stu-id="e945b-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="e945b-112">Fare clic con il pulsante destro del mouse sulla tabella Movie e selezionare Apri definizione tabella.</span><span class="sxs-lookup"><span data-stu-id="e945b-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="e945b-113">Aggiungere una colonna "classificazione" come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e945b-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="e945b-114">Poiché non sono ora disponibili valutazioni, la colonna può consentire valori null.</span><span class="sxs-lookup"><span data-stu-id="e945b-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="e945b-115">Fare clic su Salva.</span><span class="sxs-lookup"><span data-stu-id="e945b-115">Click Save.</span></span>

<span data-ttu-id="e945b-116">[![la modifica della tabella Movies](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e945b-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="e945b-117">Tornare quindi al Esplora soluzioni e aprire il file Movies. edmx (che si trova nella cartella \Models).</span><span class="sxs-lookup"><span data-stu-id="e945b-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="e945b-118">Fare clic con il pulsante destro del mouse sull'area di progettazione (l'area bianca) e selezionare Aggiorna modello da database.</span><span class="sxs-lookup"><span data-stu-id="e945b-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="e945b-119">[![Movies-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e945b-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="e945b-120">Verrà avviata l'"aggiornamento guidato".</span><span class="sxs-lookup"><span data-stu-id="e945b-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="e945b-121">Fare clic sulla scheda Aggiorna al suo interno e fare clic su fine.</span><span class="sxs-lookup"><span data-stu-id="e945b-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="e945b-122">La classe del modello di film verrà aggiornata con la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="e945b-122">Our Movie model class will then be updated with the new column.</span></span>

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="e945b-124">Dopo aver fatto clic su fine, è possibile vedere che la nuova colonna rating è stata aggiunta all'entità Movie nel modello.</span><span class="sxs-lookup"><span data-stu-id="e945b-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="e945b-125">[![entità Movie](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="e945b-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="e945b-126">È stata aggiunta una colonna nel modello di database, ma le visualizzazioni non lo conoscono.</span><span class="sxs-lookup"><span data-stu-id="e945b-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="e945b-127">Aggiornare le visualizzazioni con modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="e945b-127">Update Views with Model Changes</span></span>

<span data-ttu-id="e945b-128">È possibile aggiornare i modelli di visualizzazione in modo da riflettere la nuova colonna rating.</span><span class="sxs-lookup"><span data-stu-id="e945b-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="e945b-129">Poiché le visualizzazioni sono state create tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarle e ricrearle.</span><span class="sxs-lookup"><span data-stu-id="e945b-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="e945b-130">Tuttavia, in genere le persone hanno già apportato modifiche ai modelli di visualizzazione dalla generazione iniziale con impalcatura e vorranno aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per crea.</span><span class="sxs-lookup"><span data-stu-id="e945b-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="e945b-131">Aprire il modello \Views\Movies\Index.aspx e aggiungere una classificazione &lt;&gt;&lt;/TH&gt; all'inizio della tabella Movie.</span><span class="sxs-lookup"><span data-stu-id="e945b-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="e945b-132">Ho aggiunto il mio dopo Genre.</span><span class="sxs-lookup"><span data-stu-id="e945b-132">I added mine after Genre.</span></span> <span data-ttu-id="e945b-133">Quindi, nella stessa posizione della colonna ma in basso, aggiungere una riga per restituire la nuova classificazione.</span><span class="sxs-lookup"><span data-stu-id="e945b-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="e945b-134">Il modello finale index. aspx sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e945b-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="e945b-135">Aprire quindi il modello \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà rating:</span><span class="sxs-lookup"><span data-stu-id="e945b-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="e945b-136">Il modello di creazione. aspx finale sarà simile al seguente e modificheremo il titolo del browser e il secondario &lt;H2&gt; titolo a qualcosa come "creare un film" mentre ci siamo qui.</span><span class="sxs-lookup"><span data-stu-id="e945b-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="e945b-137">Eseguire l'app e ora è disponibile un nuovo campo nel database che è stato aggiunto alla pagina Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="e945b-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="e945b-138">Aggiungere un nuovo film: questa volta con una valutazione e fare clic su Crea.</span><span class="sxs-lookup"><span data-stu-id="e945b-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="e945b-139">[![creare un film-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="e945b-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="e945b-140">Dopo aver fatto clic su Crea, si viene inviati alla pagina di indice in cui il nuovo film è elencato con la nuova colonna rating nel database</span><span class="sxs-lookup"><span data-stu-id="e945b-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="e945b-141">[Elenco di film ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e945b-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="e945b-142">Questa esercitazione di base è stata avviata per creare i controller, associarli alle visualizzazioni e passare i dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="e945b-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="e945b-143">È stato quindi creato e progettato un database in cui sono stati inseriti alcuni dati.</span><span class="sxs-lookup"><span data-stu-id="e945b-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="e945b-144">I dati sono stati recuperati dal database e visualizzati in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="e945b-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="e945b-145">È stato quindi aggiunto un modulo di creazione che consente all'utente di aggiungere i dati al database dall'interno dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="e945b-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="e945b-146">È stata aggiunta la convalida, quindi la convalida usa JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e945b-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="e945b-147">Infine, è stato modificato il database per includere una nuova colonna di dati, quindi sono state aggiornate le due pagine per creare e visualizzare questi nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="e945b-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="e945b-148">A questo punto, si consiglia di passare all'esercitazione di livello intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", nonché ai numerosi video e risorse disponibili in [https://asp.net/mvc](https://asp.net/mvc) per saperne di più su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e945b-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="e945b-149">Buona lettura.</span><span class="sxs-lookup"><span data-stu-id="e945b-149">Enjoy!</span></span>

- <span data-ttu-id="e945b-150">Scott Hanseln- [http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) su Twitter.</span><span class="sxs-lookup"><span data-stu-id="e945b-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e945b-151">Precedente</span><span class="sxs-lookup"><span data-stu-id="e945b-151">Previous</span></span>](getting-started-with-mvc-part7.md)
