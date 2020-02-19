---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457804"
---
# <a name="adding-a-model"></a><span data-ttu-id="ec891-104">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="ec891-104">Adding a Model</span></span>

<span data-ttu-id="ec891-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec891-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ec891-106">Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ec891-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ec891-107">È più sicuro, molto più semplice da seguire e illustra altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ec891-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="ec891-108">In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="ec891-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="ec891-109">Queste classi saranno il modello di &quot;&quot; parte dell'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec891-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="ec891-110">Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) per definire e utilizzare queste classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="ec891-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="ec891-111">Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="ec891-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="ec891-112">Code First consente di creare oggetti modello scrivendo classi semplici.</span><span class="sxs-lookup"><span data-stu-id="ec891-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="ec891-113">Sono note anche come classi POCO, da oggetti CLR &quot;Plain Old.&quot;) È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="ec891-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="ec891-114">Aggiunta di classi di modelli</span><span class="sxs-lookup"><span data-stu-id="ec891-114">Adding Model Classes</span></span>

<span data-ttu-id="ec891-115">In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="ec891-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="ec891-116">Immettere il nome della *classe* &quot;Movie&quot;.</span><span class="sxs-lookup"><span data-stu-id="ec891-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="ec891-117">Aggiungere le cinque proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="ec891-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="ec891-118">Si userà la classe `Movie` per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="ec891-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="ec891-119">Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="ec891-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="ec891-120">Nello stesso file aggiungere la classe `MovieDBContext` seguente:</span><span class="sxs-lookup"><span data-stu-id="ec891-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="ec891-121">La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database.</span><span class="sxs-lookup"><span data-stu-id="ec891-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="ec891-122">Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ec891-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="ec891-123">Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `using` seguente all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="ec891-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="ec891-124">Di seguito è riportato il file *Movie.cs* completo.</span><span class="sxs-lookup"><span data-stu-id="ec891-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="ec891-125">Sono state rimosse diverse istruzioni using non necessarie.</span><span class="sxs-lookup"><span data-stu-id="ec891-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="ec891-126">Creazione di una stringa di connessione e uso di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="ec891-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="ec891-127">La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database.</span><span class="sxs-lookup"><span data-stu-id="ec891-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ec891-128">Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà.</span><span class="sxs-lookup"><span data-stu-id="ec891-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="ec891-129">A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ec891-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="ec891-130">Aprire il file *Web. config* radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ec891-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="ec891-131">(Non il file *Web. config* nella cartella *views* ). Aprire il file *Web. config* indicato in rosso.</span><span class="sxs-lookup"><span data-stu-id="ec891-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="ec891-132">Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ec891-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="ec891-133">Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="ec891-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="ec891-134">Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="ec891-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="ec891-135">Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.</span><span class="sxs-lookup"><span data-stu-id="ec891-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ec891-136">[Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="ec891-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
