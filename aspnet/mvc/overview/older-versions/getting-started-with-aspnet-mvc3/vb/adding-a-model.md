---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Aggiunta di un modello (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540523"
---
# <a name="adding-a-model-vb"></a><span data-ttu-id="7bb2c-103">Aggiunta di un modello (VB)</span><span class="sxs-lookup"><span data-stu-id="7bb2c-103">Adding a Model (VB)</span></span>

<span data-ttu-id="7bb2c-104">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7bb2c-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="7bb2c-105">In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="7bb2c-106">Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="7bb2c-107">È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7bb2c-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="7bb2c-108">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7bb2c-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="7bb2c-109">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="7bb2c-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="7bb2c-110">Aggiornamento degli strumenti di ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="7bb2c-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="7bb2c-111">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)</span><span class="sxs-lookup"><span data-stu-id="7bb2c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="7bb2c-112">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7bb2c-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="7bb2c-113">Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="7bb2c-114">[Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="7bb2c-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="7bb2c-115">Se si preferisce C#, passare alla [ C# versione](../cs/adding-a-model.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="7bb2c-116">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="7bb2c-116">Adding a Model</span></span>

<span data-ttu-id="7bb2c-117">In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="7bb2c-118">Queste classi saranno la parte "modello" dell'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="7bb2c-119">Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="7bb2c-120">Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="7bb2c-121">Code First consente di creare oggetti modello scrivendo classi semplici.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="7bb2c-122">Questi sono anche noti come classi POCO, da "Plain-Old CLR Objects". È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="7bb2c-123">Aggiunta di classi di modelli</span><span class="sxs-lookup"><span data-stu-id="7bb2c-123">Adding Model Classes</span></span>

<span data-ttu-id="7bb2c-124">In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="7bb2c-125">Assegnare alla classe il nome "Movie".</span><span class="sxs-lookup"><span data-stu-id="7bb2c-125">Name the class "Movie".</span></span>

<span data-ttu-id="7bb2c-126">Aggiungere le cinque proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="7bb2c-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="7bb2c-127">Si userà la classe `Movie` per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="7bb2c-128">Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="7bb2c-129">Nello stesso file aggiungere la classe `MovieDBContext` seguente:</span><span class="sxs-lookup"><span data-stu-id="7bb2c-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="7bb2c-130">La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="7bb2c-131">Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="7bb2c-132">Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti della produttività per il Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="7bb2c-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="7bb2c-133">Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `imports` seguente all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="7bb2c-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="7bb2c-134">Il file completo *Movie. vb* è riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="7bb2c-135">Creazione di una stringa di connessione e utilizzo di SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="7bb2c-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="7bb2c-136">La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="7bb2c-137">Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="7bb2c-138">A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="7bb2c-139">Aprire il file *Web. config* radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="7bb2c-140">(Non il file *Web. config* nella cartella *views* ). L'immagine seguente mostra entrambi i file *Web. config* ; Aprire il file *Web. config* in un cerchio in rosso.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="7bb2c-141">Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="7bb2c-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="7bb2c-142">Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="7bb2c-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="7bb2c-143">Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="7bb2c-144">Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.</span><span class="sxs-lookup"><span data-stu-id="7bb2c-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7bb2c-145">[Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="7bb2c-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
