---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creazione di una stringa di connessione e utilizzo di SQL Server database locale | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d3c6e736c5dcf4a3615e3c72cfc033effc7cc8e6
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519310"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="29514-102">Creazione di una stringa di connessione e uso di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="29514-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="29514-103">di [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="29514-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="29514-104">Creazione di una stringa di connessione e uso di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="29514-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="29514-105">La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database.</span><span class="sxs-lookup"><span data-stu-id="29514-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="29514-106">Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà.</span><span class="sxs-lookup"><span data-stu-id="29514-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="29514-107">In realtà, non è necessario specificare il database da utilizzare, Entity Framework utilizzerà per impostazione predefinita il database [locale](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="29514-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="29514-108">In questa sezione verrà aggiunta in modo esplicito una stringa di connessione nel file *Web. config* dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="29514-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="29514-109">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="29514-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="29514-110">Il [database locale](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera del SQL Server Express motore di database che viene avviato su richiesta e viene eseguito in modalità utente.</span><span class="sxs-lookup"><span data-stu-id="29514-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="29514-111">Il database locale viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di usare i database come file con *estensione MDF* .</span><span class="sxs-lookup"><span data-stu-id="29514-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="29514-112">In genere, i file di database del database locale vengono conservati nella cartella *App\_data* di un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="29514-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="29514-113">Non è consigliabile usare SQL Server Express per le applicazioni Web di produzione.</span><span class="sxs-lookup"><span data-stu-id="29514-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="29514-114">In particolare, il database locale non deve essere usato per la produzione con un'applicazione Web perché non è progettato per funzionare con IIS.</span><span class="sxs-lookup"><span data-stu-id="29514-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="29514-115">È tuttavia possibile eseguire facilmente la migrazione di un database del database locale a SQL Server o SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="29514-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="29514-116">In Visual Studio 2017, il database locale viene installato per impostazione predefinita con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29514-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="29514-117">Per impostazione predefinita, il Entity Framework cerca una stringa di connessione denominata uguale alla classe del contesto dell'oggetto (`MovieDBContext` per questo progetto).</span><span class="sxs-lookup"><span data-stu-id="29514-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="29514-118">Per ulteriori informazioni, vedere [SQL Server stringhe di connessione per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="29514-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="29514-119">Aprire il file *Web. config* radice dell'applicazione mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="29514-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="29514-120">(Non il file *Web. config* nella cartella *views* ).</span><span class="sxs-lookup"><span data-stu-id="29514-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="29514-121">Trovare l'elemento `<connectionStrings>`:</span><span class="sxs-lookup"><span data-stu-id="29514-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="29514-122">Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="29514-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="29514-123">Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="29514-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="29514-124">Le due stringhe di connessione sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="29514-124">The two connection strings are very similar.</span></span> <span data-ttu-id="29514-125">La prima stringa di connessione è denominata `DefaultConnection` e viene utilizzata per il database delle appartenenze per controllare chi può accedere all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="29514-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="29514-126">La stringa di connessione aggiunta specifica un database local DB denominato *Movie. MDF* presente nella cartella *app\_data* .</span><span class="sxs-lookup"><span data-stu-id="29514-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="29514-127">In questa esercitazione non verrà usato il database delle appartenenze. per altre informazioni sull'appartenenza, l'autenticazione e la sicurezza, vedere l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="29514-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="29514-128">Il nome della stringa di connessione deve corrispondere al nome della classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .</span><span class="sxs-lookup"><span data-stu-id="29514-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="29514-129">In realtà non è necessario aggiungere la stringa di connessione `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="29514-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="29514-130">Se non si specifica una stringa di connessione, Entity Framework creerà un database del database locale nella directory Users con il nome completo della classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (in questo caso `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="29514-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="29514-131">È possibile assegnare un nome qualsiasi al database, purché sia presente *.* Suffisso MDF.</span><span class="sxs-lookup"><span data-stu-id="29514-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="29514-132">Ad esempio, è possibile denominare il database *Films. MDF*.</span><span class="sxs-lookup"><span data-stu-id="29514-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="29514-133">Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.</span><span class="sxs-lookup"><span data-stu-id="29514-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="29514-134">[Precedente](adding-a-model.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="29514-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
