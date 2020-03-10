---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581585"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="3699a-104">Introduzione ad ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3699a-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="3699a-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3699a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="3699a-106">Una versione aggiornata se questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="3699a-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="3699a-107">La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3699a-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="3699a-108">Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3699a-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3699a-109">Verrà creata una semplice applicazione Web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="3699a-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3699a-110">Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3699a-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="3699a-111">È ora necessario creare la prima applicazione Web MVC ASP.NET con [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="3699a-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="3699a-112">Verrà creata un'applicazione di elenco di film che ci consentirà di creare ed elencare i film.</span><span class="sxs-lookup"><span data-stu-id="3699a-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="3699a-113">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="3699a-113">What You'll Build</span></span>

<span data-ttu-id="3699a-114">Ecco due screenshot dell'applicazione che verrà compilata.</span><span class="sxs-lookup"><span data-stu-id="3699a-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="3699a-115">Si disporrà di una semplice tabella di filmati con varie colonne.</span><span class="sxs-lookup"><span data-stu-id="3699a-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="3699a-116">[Elenco di film ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="3699a-117">Ed è possibile creare un modulo per aggiungere film all'elenco.</span><span class="sxs-lookup"><span data-stu-id="3699a-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="3699a-118">[![creare un film-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="3699a-119">Acquisizione di competenze</span><span class="sxs-lookup"><span data-stu-id="3699a-119">Skills You'll Learn</span></span>

<span data-ttu-id="3699a-120">Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web MVC ASP.NET con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3699a-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="3699a-121">Si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="3699a-121">You'll learn:</span></span>

- <span data-ttu-id="3699a-122">Come creare un nuovo progetto MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3699a-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="3699a-123">Come creare un nuovo database con SQL Server</span><span class="sxs-lookup"><span data-stu-id="3699a-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="3699a-124">Come creare controller e visualizzazioni MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3699a-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="3699a-125">Come recuperare e visualizzare i dati</span><span class="sxs-lookup"><span data-stu-id="3699a-125">How to retrieve and display data</span></span>
- <span data-ttu-id="3699a-126">Come modificare i dati e abilitare la convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="3699a-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="3699a-127">Come aggiornare lo schema del database</span><span class="sxs-lookup"><span data-stu-id="3699a-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="3699a-128">Introduzione</span><span class="sxs-lookup"><span data-stu-id="3699a-128">Get Started</span></span>

<span data-ttu-id="3699a-129">Per iniziare, eseguire Visual Web Developer 2010 Express, che chiamerò "VWD" da ora in poi, e selezionare nuovo progetto dalla schermata Start.</span><span class="sxs-lookup"><span data-stu-id="3699a-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="3699a-130">Visual Web Developer è un IDE o un ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="3699a-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="3699a-131">Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3699a-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="3699a-132">Nella parte superiore sono disponibili una barra degli strumenti che mostra le varie opzioni disponibili, nonché il menu che è possibile usare per selezionare il file | Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="3699a-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="3699a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="3699a-134">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="3699a-134">Creating Your First Application</span></span>

<span data-ttu-id="3699a-135">È possibile creare applicazioni usando Visual Basic o Visual C#.</span><span class="sxs-lookup"><span data-stu-id="3699a-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="3699a-136">Per il momento selezionare oggetto C# visivo a sinistra, quindi scegliere "ASP.NET MVC 2 Web Application".</span><span class="sxs-lookup"><span data-stu-id="3699a-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="3699a-137">Denominare il progetto "Movies" e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="3699a-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="3699a-138">[![nuovo progetto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="3699a-139">Sul lato destro è presente la Esplora soluzioni che Mostra tutti i file e le cartelle dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3699a-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="3699a-140">La finestra grande al centro è la posizione in cui si modifica il codice e si dedica la maggior parte del tempo.</span><span class="sxs-lookup"><span data-stu-id="3699a-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="3699a-141">Visual Studio ha usato un modello predefinito per il progetto MVC ASP.NET appena creato ed è quindi disponibile un'applicazione funzionante senza eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="3699a-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="3699a-142">Si tratta di una semplice "Hello World!</span><span class="sxs-lookup"><span data-stu-id="3699a-142">This is a simple "Hello World!</span></span> <span data-ttu-id="3699a-143">progetto ed è un punto di partenza ideale per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3699a-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="3699a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="3699a-145">Selezionare il pulsante "Riproduci" sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="3699a-145">Select the "play" button to the toolbar.</span></span>

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="3699a-147">Si tratta di una freccia verde che punta a destra che compilerà il programma e avvierà l'applicazione in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="3699a-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="3699a-148">*Nota: è possibile premere F5 sulla tastiera oppure selezionare debug-&gt;avviare il debug dal menu "debug".*</span><span class="sxs-lookup"><span data-stu-id="3699a-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="3699a-149">In questo modo, Visual Web Developer avvierà un server Web di sviluppo ed eseguirà l'applicazione Web (per abilitare questa operazione non è necessaria alcuna configurazione o procedura manuale).</span><span class="sxs-lookup"><span data-stu-id="3699a-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="3699a-150">Viene quindi avviato un browser e configurato per l'esplorazione della Home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3699a-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="3699a-151">Si noti che la barra degli indirizzi del browser indica "localhost" e non un elemento come example.com.</span><span class="sxs-lookup"><span data-stu-id="3699a-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="3699a-152">Questo perché localhost fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena compilata.</span><span class="sxs-lookup"><span data-stu-id="3699a-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="3699a-153">[Home page di ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="3699a-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="3699a-154">Questo modello predefinito fornisce due pagine da visitare e una pagina di accesso di base.</span><span class="sxs-lookup"><span data-stu-id="3699a-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="3699a-155">Modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC nel processo.</span><span class="sxs-lookup"><span data-stu-id="3699a-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="3699a-156">Chiudere il browser e modificare il codice.</span><span class="sxs-lookup"><span data-stu-id="3699a-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3699a-157">avanti</span><span class="sxs-lookup"><span data-stu-id="3699a-157">Next</span></span>](getting-started-with-mvc-part2.md)
