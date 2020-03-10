---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: file-> nuovo progetto | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 1 vengono illustrati i Cenni preliminari e file/nuovo progetto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636129"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="e99cb-104">Parte 1: file-> nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="e99cb-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="e99cb-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e99cb-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e99cb-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="e99cb-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e99cb-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="e99cb-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e99cb-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="e99cb-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e99cb-109">Nella parte 1 vengono illustrati i Cenni preliminari e file/nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="e99cb-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="e99cb-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e99cb-110">Overview</span></span>

<span data-ttu-id="e99cb-111">Questa esercitazione è un'introduzione ai Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e99cb-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="e99cb-112">Inizieremo lentamente, quindi l'esperienza di sviluppo Web a livello di principianti è accettabile.</span><span class="sxs-lookup"><span data-stu-id="e99cb-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="e99cb-113">L'applicazione che verrà compilata è un semplice negozio online.</span><span class="sxs-lookup"><span data-stu-id="e99cb-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="e99cb-114">I visitatori possono sfogliare i prodotti in base alla categoria:</span><span class="sxs-lookup"><span data-stu-id="e99cb-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="e99cb-115">Possono visualizzare un singolo prodotto e aggiungerlo al carrello:</span><span class="sxs-lookup"><span data-stu-id="e99cb-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="e99cb-116">Possono esaminare il carrello, rimuovendo tutti gli elementi che non vogliono più:</span><span class="sxs-lookup"><span data-stu-id="e99cb-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="e99cb-117">Continuando con l'estrazione verrà richiesto di</span><span class="sxs-lookup"><span data-stu-id="e99cb-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="e99cb-118">Dopo l'ordinamento, viene visualizzata una schermata di conferma semplice:</span><span class="sxs-lookup"><span data-stu-id="e99cb-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="e99cb-119">Si inizierà con la creazione di un nuovo progetto WebForms di ASP.NET in Visual Studio 2010 e si aggiungeranno in modo incrementale le funzionalità per creare un'applicazione completa funzionante.</span><span class="sxs-lookup"><span data-stu-id="e99cb-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="e99cb-120">Verranno illustrati l'accesso al database, le visualizzazioni elenco e griglia, le pagine di aggiornamento dei dati, la convalida dei dati, l'utilizzo di pagine master per un layout di pagina coerente, AJAX, convalida, appartenenza utente e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="e99cb-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="e99cb-121">È possibile seguire la procedura dettagliata oppure è possibile scaricare l'applicazione completata da [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="e99cb-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="e99cb-122">È possibile usare Visual Studio 2010 o la versione gratuita di Visual Web Developer 2010 da [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="e99cb-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="e99cb-123">Per compilare l'applicazione, è possibile utilizzare SQL Server o la SQL Server Express gratuita per ospitare il database.</span><span class="sxs-lookup"><span data-stu-id="e99cb-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="e99cb-124">File/nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="e99cb-124">File / New Project</span></span>

<span data-ttu-id="e99cb-125">Per iniziare, selezionare il nuovo progetto dal menu file in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e99cb-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="e99cb-126">Verrà visualizzata la finestra di dialogo nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="e99cb-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="e99cb-127">Selezionare il gruppo di modelli C# visivi e Web a sinistra, quindi scegliere il modello "applicazione Web ASP.NET" nella colonna centrale.</span><span class="sxs-lookup"><span data-stu-id="e99cb-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="e99cb-128">Denominare il progetto TailspinSpyworks e premere il pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="e99cb-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="e99cb-129">Verrà creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="e99cb-129">This will create our project.</span></span> <span data-ttu-id="e99cb-130">Verranno ora esaminate le cartelle incluse nell'applicazione nell'Esplora soluzioni sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="e99cb-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="e99cb-131">La soluzione vuota non è completamente vuota, ma aggiunge una struttura di cartelle di base:</span><span class="sxs-lookup"><span data-stu-id="e99cb-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="e99cb-132">Prendere nota delle convenzioni implementate dal modello di progetto ASP.NET 4 predefinito.</span><span class="sxs-lookup"><span data-stu-id="e99cb-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="e99cb-133">La cartella "account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza di NET.</span><span class="sxs-lookup"><span data-stu-id="e99cb-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="e99cb-134">La cartella "Scripts" funge da repository per i file JavaScript lato client e i file jQuery. js di base vengono resi disponibili per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e99cb-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="e99cb-135">La cartella "stili" viene usata per organizzare gli oggetti visivi del sito Web (fogli di stile CSS)</span><span class="sxs-lookup"><span data-stu-id="e99cb-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="e99cb-136">Quando si preme F5 per eseguire l'applicazione e si esegue il rendering della pagina default. aspx, viene visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="e99cb-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="e99cb-137">Il primo miglioramento dell'applicazione consiste nel sostituire il file Style. CSS dal modello Web Forms predefinito con le classi CSS e i file di immagine associati che eseguiranno il rendering dell'oggetto visivo asthetics che si vuole per l'applicazione SpyWorks di Tilt.</span><span class="sxs-lookup"><span data-stu-id="e99cb-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="e99cb-138">Dopo questa operazione, il rendering della pagina default. aspx è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="e99cb-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="e99cb-139">Si notino i collegamenti all'immagine nella parte superiore destra della pagina e le voci di menu aggiunte alla pagina master.</span><span class="sxs-lookup"><span data-stu-id="e99cb-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="e99cb-140">Solo i collegamenti "Accedi" e "account" indicano le pagine esistenti (generate dal modello predefinito) e il resto delle pagine che si implementeranno durante la compilazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e99cb-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="e99cb-141">La pagina master verrà rilocata anche nella directory Styles.</span><span class="sxs-lookup"><span data-stu-id="e99cb-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="e99cb-142">Anche se questa è solo una preferenza, può rendere le cose più semplici se si decide di rendere l'applicazione "skinable" in futuro.</span><span class="sxs-lookup"><span data-stu-id="e99cb-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="e99cb-143">Al termine di questa operazione, sarà necessario modificare i riferimenti della pagina master in tutti i file con estensione aspx generati dalle pagine Web Forms ASP.NET predefinite.</span><span class="sxs-lookup"><span data-stu-id="e99cb-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e99cb-144">avanti</span><span class="sxs-lookup"><span data-stu-id="e99cb-144">Next</span></span>](tailspin-spyworks-part-2.md)
