---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduzione a ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: e71275c93558c0b6ca087a145786e8c846b69721
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540670"
---
# <a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="8201a-103">Introduzione ad ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="8201a-103">Intro to ASP.NET MVC 3 (C#)</span></span>

<span data-ttu-id="8201a-104">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8201a-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="8201a-105">Una versione aggiornata di questa esercitazione è disponibile [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8201a-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="8201a-106">È più sicuro, molto più semplice da seguire e illustra altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="8201a-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="8201a-107">In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8201a-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="8201a-108">Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="8201a-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="8201a-109">È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="8201a-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="8201a-110">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="8201a-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="8201a-111">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="8201a-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="8201a-112">Aggiornamento degli strumenti di ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="8201a-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="8201a-113">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)</span><span class="sxs-lookup"><span data-stu-id="8201a-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="8201a-114">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="8201a-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="8201a-115">Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="8201a-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="8201a-116">[Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="8201a-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="8201a-117">Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="8201a-118">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="8201a-118">What You'll Build</span></span>

<span data-ttu-id="8201a-119">Verrà implementata una semplice applicazione per la visualizzazione di filmati che supporta la creazione, la modifica e l'inserimento di filmati da un database.</span><span class="sxs-lookup"><span data-stu-id="8201a-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="8201a-120">Di seguito sono riportate due schermate dell'applicazione da compilare.</span><span class="sxs-lookup"><span data-stu-id="8201a-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="8201a-121">Include una pagina in cui viene visualizzato un elenco di filmati da un database:</span><span class="sxs-lookup"><span data-stu-id="8201a-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="8201a-123">L'applicazione consente inoltre di aggiungere, modificare ed eliminare filmati, nonché di visualizzare i dettagli relativi a singoli utenti.</span><span class="sxs-lookup"><span data-stu-id="8201a-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="8201a-124">Tutti gli scenari di immissione dei dati includono la convalida per garantire la correttezza dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="8201a-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="8201a-125">Acquisizione di competenze</span><span class="sxs-lookup"><span data-stu-id="8201a-125">Skills You'll Learn</span></span>

<span data-ttu-id="8201a-126">In questa esercitazione si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="8201a-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="8201a-127">Come creare un nuovo progetto MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8201a-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="8201a-128">Come creare controller e visualizzazioni MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8201a-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="8201a-129">Come creare un nuovo database usando il paradigma Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="8201a-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="8201a-130">Come recuperare e visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="8201a-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="8201a-131">Come modificare i dati e abilitare la convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="8201a-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8201a-132">Introduzione</span><span class="sxs-lookup"><span data-stu-id="8201a-132">Getting Started</span></span>

<span data-ttu-id="8201a-133">Per iniziare, eseguire Visual Web Developer 2010 Express ("Visual Web Developer" per brevità) e selezionare **nuovo progetto** nella pagina **iniziale** .</span><span class="sxs-lookup"><span data-stu-id="8201a-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="8201a-134">Visual Web Developer è un IDE o Integrated Development Environment.</span><span class="sxs-lookup"><span data-stu-id="8201a-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="8201a-135">Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="8201a-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="8201a-136">In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le varie opzioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="8201a-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="8201a-137">È anche disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="8201a-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="8201a-138">Ad esempio, invece di selezionare **nuovo progetto** dalla pagina **iniziale** , è possibile usare il menu e selezionare **file** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="8201a-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="8201a-139">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="8201a-139">Creating Your First Application</span></span>

<span data-ttu-id="8201a-140">È possibile creare applicazioni utilizzando Visual Basic o Visual C# come linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="8201a-141">Selezionare oggetto C# visivo a sinistra e quindi selezionare **ASP.NET MVC 3 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="8201a-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="8201a-142">Assegnare al progetto il nome "MvcMovie" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8201a-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="8201a-143">Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="8201a-144">Nella finestra di dialogo **nuovo progetto MVC 3 ASP.NET** selezionare **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="8201a-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="8201a-145">Selezionare **Usa markup HTML5** e lascia **Razor** come motore di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="8201a-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="8201a-146">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8201a-146">Click **OK**.</span></span> <span data-ttu-id="8201a-147">In Visual Web Developer è stato usato un modello predefinito per il progetto MVC ASP.NET appena creato, quindi è disponibile un'applicazione funzionante senza eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="8201a-148">Si tratta di una semplice "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="8201a-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="8201a-149">il progetto è una soluzione ideale per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="8201a-150">Scegliere **Avvia debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="8201a-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="8201a-151">Si noti che il tasto di scelta rapida per avviare il debug è F5.</span><span class="sxs-lookup"><span data-stu-id="8201a-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="8201a-152">F5 fa in modo che Visual Web Developer avvii un server Web di sviluppo ed esegua l'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="8201a-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="8201a-153">Visual Web Developer avvia quindi un browser e apre la home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8201a-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="8201a-154">Si noti che la barra degli indirizzi del browser indica `localhost` e non un elemento come `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8201a-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="8201a-155">Questo perché `localhost` fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena creata.</span><span class="sxs-lookup"><span data-stu-id="8201a-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="8201a-156">Quando Visual Web Developer esegue un progetto Web, viene utilizzata una porta casuale per il server Web.</span><span class="sxs-lookup"><span data-stu-id="8201a-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8201a-157">Nell'immagine seguente il numero di porta casuale è 43246.</span><span class="sxs-lookup"><span data-stu-id="8201a-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="8201a-158">Quando si esegue l'applicazione, è probabile che venga visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="8201a-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="8201a-159">Il modello predefinito fornisce due pagine da visitare e una pagina di accesso di base.</span><span class="sxs-lookup"><span data-stu-id="8201a-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="8201a-160">Il passaggio successivo consiste nel modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC nel processo.</span><span class="sxs-lookup"><span data-stu-id="8201a-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="8201a-161">Chiudere il browser e modificare il codice.</span><span class="sxs-lookup"><span data-stu-id="8201a-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8201a-162">avanti</span><span class="sxs-lookup"><span data-stu-id="8201a-162">Next</span></span>](adding-a-controller.md)
