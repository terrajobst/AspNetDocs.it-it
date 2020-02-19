---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Aggiunta di un modelloC#() | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457785"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="ceb23-104">Aggiunta di un modello (C#)</span><span class="sxs-lookup"><span data-stu-id="ceb23-104">Adding a Model (C#)</span></span>

<span data-ttu-id="ceb23-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ceb23-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="ceb23-106">In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ceb23-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="ceb23-107">Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="ceb23-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="ceb23-108">È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ceb23-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="ceb23-109">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ceb23-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="ceb23-110">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="ceb23-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="ceb23-111">Aggiornamento degli strumenti di ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="ceb23-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="ceb23-112">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)</span><span class="sxs-lookup"><span data-stu-id="ceb23-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="ceb23-113">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="ceb23-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="ceb23-114">Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="ceb23-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="ceb23-115">[Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="ceb23-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="ceb23-116">Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/adding-a-model.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ceb23-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="ceb23-117">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="ceb23-117">Adding a Model</span></span>

<span data-ttu-id="ceb23-118">In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="ceb23-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="ceb23-119">Queste classi saranno la parte "modello" dell'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ceb23-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="ceb23-120">Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="ceb23-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="ceb23-121">Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="ceb23-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="ceb23-122">Code First consente di creare oggetti modello scrivendo classi semplici.</span><span class="sxs-lookup"><span data-stu-id="ceb23-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="ceb23-123">Questi sono anche noti come classi POCO, da "Plain-Old CLR Objects". È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="ceb23-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="ceb23-124">Aggiunta di classi di modelli</span><span class="sxs-lookup"><span data-stu-id="ceb23-124">Adding Model Classes</span></span>

<span data-ttu-id="ceb23-125">In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="ceb23-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="ceb23-126">Assegnare alla *classe* il nome "Movie".</span><span class="sxs-lookup"><span data-stu-id="ceb23-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="ceb23-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ceb23-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="ceb23-128">Aggiungere le cinque proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="ceb23-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="ceb23-129">Si userà la classe `Movie` per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="ceb23-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="ceb23-130">Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="ceb23-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="ceb23-131">Nello stesso file aggiungere la classe `MovieDBContext` seguente:</span><span class="sxs-lookup"><span data-stu-id="ceb23-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="ceb23-132">La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database.</span><span class="sxs-lookup"><span data-stu-id="ceb23-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="ceb23-133">Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ceb23-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="ceb23-134">Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti della produttività per il Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="ceb23-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="ceb23-135">Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `using` seguente all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="ceb23-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="ceb23-136">Di seguito è riportato il file *Movie.cs* completo.</span><span class="sxs-lookup"><span data-stu-id="ceb23-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="ceb23-137">Creazione di una stringa di connessione e utilizzo di SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="ceb23-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="ceb23-138">La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database.</span><span class="sxs-lookup"><span data-stu-id="ceb23-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ceb23-139">Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà.</span><span class="sxs-lookup"><span data-stu-id="ceb23-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="ceb23-140">A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ceb23-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="ceb23-141">Aprire il file *Web. config* radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ceb23-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="ceb23-142">(Non il file *Web. config* nella cartella *views* ). L'immagine seguente mostra entrambi i file *Web. config* ; Aprire il file *Web. config* in un cerchio in rosso.</span><span class="sxs-lookup"><span data-stu-id="ceb23-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="ceb23-143">Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="ceb23-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="ceb23-144">Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="ceb23-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="ceb23-145">Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="ceb23-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="ceb23-146">Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.</span><span class="sxs-lookup"><span data-stu-id="ceb23-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ceb23-147">[Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="ceb23-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
