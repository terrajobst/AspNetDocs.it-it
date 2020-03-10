---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usare Migrazioni Code First per il seeding del database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557456"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="d8113-102">Usare Migrazioni Code First per inizializzare il database</span><span class="sxs-lookup"><span data-stu-id="d8113-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="d8113-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d8113-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d8113-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="d8113-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d8113-105">In questa sezione si userà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) in EF per eseguire il seeding del database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="d8113-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="d8113-106">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="d8113-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d8113-107">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d8113-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="d8113-108">Questo comando aggiunge una cartella denominata Migrations al progetto, oltre a un file di codice denominato Configuration.cs nella cartella Migrations.</span><span class="sxs-lookup"><span data-stu-id="d8113-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="d8113-109">Aprire il file Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="d8113-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="d8113-110">Aggiungere la seguente istruzione **using** .</span><span class="sxs-lookup"><span data-stu-id="d8113-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="d8113-111">Aggiungere quindi il codice seguente al metodo **Configuration. Seed** :</span><span class="sxs-lookup"><span data-stu-id="d8113-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="d8113-112">Nella finestra console di gestione pacchetti digitare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d8113-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="d8113-113">Il primo comando genera il codice che crea il database e il secondo comando esegue tale codice.</span><span class="sxs-lookup"><span data-stu-id="d8113-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="d8113-114">Il database viene creato localmente, utilizzando il database [locale](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8113-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="d8113-115">Esplorare l'API (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="d8113-115">Explore the API (Optional)</span></span>

<span data-ttu-id="d8113-116">Premere F5 per eseguire l'applicazione in modalità debug.</span><span class="sxs-lookup"><span data-stu-id="d8113-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="d8113-117">Visual Studio avvia IIS Express ed esegue l'app Web.</span><span class="sxs-lookup"><span data-stu-id="d8113-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="d8113-118">Visual Studio avvia quindi un browser e apre il home page dell'app.</span><span class="sxs-lookup"><span data-stu-id="d8113-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="d8113-119">Quando Visual Studio esegue un progetto Web, assegna un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="d8113-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="d8113-120">Nell'immagine seguente il numero di porta è 50524.</span><span class="sxs-lookup"><span data-stu-id="d8113-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="d8113-121">Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="d8113-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="d8113-122">Il home page viene implementato con ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d8113-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="d8113-123">Nella parte superiore della pagina è presente un collegamento che indica "API".</span><span class="sxs-lookup"><span data-stu-id="d8113-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="d8113-124">Questo collegamento consente di portare a una pagina della guida generata automaticamente per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="d8113-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="d8113-125">Per informazioni sul modo in cui viene generata questa pagina della guida e su come è possibile aggiungere la propria documentazione alla pagina, vedere [creazione di pagine della Guida per API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md). È possibile fare clic sui collegamenti della pagina della Guida per visualizzare i dettagli relativi all'API, incluso il formato della richiesta e della risposta.</span><span class="sxs-lookup"><span data-stu-id="d8113-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="d8113-126">L'API Abilita le operazioni CRUD sul database.</span><span class="sxs-lookup"><span data-stu-id="d8113-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="d8113-127">Di seguito viene riepilogata l'API.</span><span class="sxs-lookup"><span data-stu-id="d8113-127">The following summarizes the API.</span></span>

| <span data-ttu-id="d8113-128">Autori</span><span class="sxs-lookup"><span data-stu-id="d8113-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="d8113-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="d8113-129">GET api/authors</span></span> | <span data-ttu-id="d8113-130">Ottenere tutti gli autori.</span><span class="sxs-lookup"><span data-stu-id="d8113-130">Get all authors.</span></span> |
| <span data-ttu-id="d8113-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-131">GET api/authors/{id}</span></span> | <span data-ttu-id="d8113-132">Ottenere un autore in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="d8113-132">Get an author by ID.</span></span> |
| <span data-ttu-id="d8113-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="d8113-133">POST /api/authors</span></span> | <span data-ttu-id="d8113-134">Creare un nuovo autore.</span><span class="sxs-lookup"><span data-stu-id="d8113-134">Create a new author.</span></span> |
| <span data-ttu-id="d8113-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="d8113-136">Aggiornare un autore esistente.</span><span class="sxs-lookup"><span data-stu-id="d8113-136">Update an existing author.</span></span> |
| <span data-ttu-id="d8113-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="d8113-138">Eliminare un autore.</span><span class="sxs-lookup"><span data-stu-id="d8113-138">Delete an author.</span></span> |

| <span data-ttu-id="d8113-139">Libri</span><span class="sxs-lookup"><span data-stu-id="d8113-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="d8113-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="d8113-140">GET /api/books</span></span> | <span data-ttu-id="d8113-141">Ottenere tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="d8113-141">Get all books.</span></span> |
| <span data-ttu-id="d8113-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-142">GET /api/books/{id}</span></span> | <span data-ttu-id="d8113-143">Ottenere un libro in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="d8113-143">Get a book by ID.</span></span> |
| <span data-ttu-id="d8113-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="d8113-144">POST /api/books</span></span> | <span data-ttu-id="d8113-145">Creare un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="d8113-145">Create a new book.</span></span> |
| <span data-ttu-id="d8113-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="d8113-147">Aggiornare un libro esistente.</span><span class="sxs-lookup"><span data-stu-id="d8113-147">Update an existing book.</span></span> |
| <span data-ttu-id="d8113-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="d8113-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="d8113-149">Eliminare un libro.</span><span class="sxs-lookup"><span data-stu-id="d8113-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="d8113-150">Visualizzare il database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="d8113-150">View the Database (Optional)</span></span>

<span data-ttu-id="d8113-151">Quando è stato eseguito il comando Update-database, EF ha creato il database e ha chiamato il metodo `Seed`.</span><span class="sxs-lookup"><span data-stu-id="d8113-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="d8113-152">Quando si esegue l'applicazione in locale, EF usa il [database locale](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8113-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="d8113-153">È possibile visualizzare il database in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d8113-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="d8113-154">Dal menu **Visualizza** selezionare **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d8113-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="d8113-155">Nella finestra di dialogo **Connetti al server** , nella casella di modifica **nome server** , digitare "(local DB) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="d8113-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="d8113-156">Lasciare l'opzione di **autenticazione** "autenticazione di Windows".</span><span class="sxs-lookup"><span data-stu-id="d8113-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="d8113-157">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="d8113-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="d8113-158">Visual Studio si connette al database locale e Visualizza i database esistenti nella finestra Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d8113-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="d8113-159">È possibile espandere i nodi per visualizzare le tabelle create da EF.</span><span class="sxs-lookup"><span data-stu-id="d8113-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="d8113-160">Per visualizzare i dati, fare clic con il pulsante destro del mouse su una tabella e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="d8113-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="d8113-161">La schermata seguente mostra i risultati della tabella books.</span><span class="sxs-lookup"><span data-stu-id="d8113-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="d8113-162">Si noti che EF ha popolato il database con i dati di inizializzazione e la tabella contiene la chiave esterna per la tabella authors.</span><span class="sxs-lookup"><span data-stu-id="d8113-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="d8113-163">[Precedente](part-2.md)
> [Successivo](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d8113-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
