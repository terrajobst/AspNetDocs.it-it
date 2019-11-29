---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introduzione con Web Form ASP.NET 4,7 e Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni dettagliate illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615459"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="16074-103">Introduzione con Web Form ASP.NET 4,5 e Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="16074-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>

<span data-ttu-id="16074-104">[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="16074-104">[Download Wingtip Toys Sample Project (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="16074-105">Questa serie di esercitazioni illustra come compilare un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="16074-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="16074-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="16074-106">Introduction</span></span>

<span data-ttu-id="16074-107">Questa serie di esercitazioni illustra la creazione di un'applicazione Web Form ASP.NET con Visual Studio 2017 e ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16074-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="16074-108">Si creerà un'applicazione denominata **Wingtip Toys** , un sito Web di una vetrina semplificata che vende gli elementi online.</span><span class="sxs-lookup"><span data-stu-id="16074-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="16074-109">Durante la serie vengono evidenziate le nuove funzionalità di ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16074-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="16074-110">Destinatari</span><span class="sxs-lookup"><span data-stu-id="16074-110">Target audience</span></span>

<span data-ttu-id="16074-111">Gli sviluppatori che non hanno familiarità con ASP.NET Web Form sono i destinatari di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="16074-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="16074-112">È necessario conoscere le aree seguenti:</span><span class="sxs-lookup"><span data-stu-id="16074-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="16074-113">Programmazione orientata a oggetti (OOP) e linguaggi</span><span class="sxs-lookup"><span data-stu-id="16074-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="16074-114">Sviluppo Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="16074-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="16074-115">Database relazionali</span><span class="sxs-lookup"><span data-stu-id="16074-115">Relational databases</span></span>
- <span data-ttu-id="16074-116">Architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="16074-116">N-tier architecture</span></span>

<span data-ttu-id="16074-117">Per esaminare queste aree, provare a studiare il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="16074-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="16074-118">Introduzione con VisualC#</span><span class="sxs-lookup"><span data-stu-id="16074-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="16074-119">[Sviluppo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="16074-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="16074-120">Database relazionale</span><span class="sxs-lookup"><span data-stu-id="16074-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="16074-121">Architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="16074-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="16074-122">Funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="16074-122">Application features</span></span>

<span data-ttu-id="16074-123">Le funzionalità del Web Form ASP.NET presentate in questa serie includono:</span><span class="sxs-lookup"><span data-stu-id="16074-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="16074-124">Progetto di applicazione Web (non progetto sito Web)</span><span class="sxs-lookup"><span data-stu-id="16074-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="16074-125">Web Form</span><span class="sxs-lookup"><span data-stu-id="16074-125">Web Forms</span></span>
- <span data-ttu-id="16074-126">Pagine master, configurazione</span><span class="sxs-lookup"><span data-stu-id="16074-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="16074-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="16074-127">Bootstrap</span></span>
- <span data-ttu-id="16074-128">Entity Framework Code First, database locale</span><span class="sxs-lookup"><span data-stu-id="16074-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="16074-129">Convalida della richiesta</span><span class="sxs-lookup"><span data-stu-id="16074-129">Request Validation</span></span>
- <span data-ttu-id="16074-130">Controlli dati fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="16074-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="16074-131">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="16074-131">Model Binding</span></span>
- <span data-ttu-id="16074-132">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="16074-132">Data Annotations</span></span>
- <span data-ttu-id="16074-133">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="16074-133">Value Providers</span></span>
- <span data-ttu-id="16074-134">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="16074-134">SSL and OAuth</span></span>
- <span data-ttu-id="16074-135">ASP.NET Identity, configurazione e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="16074-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="16074-136">Convalida non intrusiva</span><span class="sxs-lookup"><span data-stu-id="16074-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="16074-137">Routing</span><span class="sxs-lookup"><span data-stu-id="16074-137">Routing</span></span>
- <span data-ttu-id="16074-138">Gestione degli errori di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="16074-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="16074-139">Scenari e attività delle applicazioni</span><span class="sxs-lookup"><span data-stu-id="16074-139">Application scenarios and tasks</span></span>

<span data-ttu-id="16074-140">Le attività della serie di esercitazioni includono:</span><span class="sxs-lookup"><span data-stu-id="16074-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="16074-141">Creazione, revisione ed esecuzione di un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="16074-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="16074-142">Creazione di una struttura di database</span><span class="sxs-lookup"><span data-stu-id="16074-142">Creating a database structure</span></span>
- <span data-ttu-id="16074-143">Inizializzazione e seeding di un database</span><span class="sxs-lookup"><span data-stu-id="16074-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="16074-144">Personalizzazione dell'interfaccia utente con stili, grafica e una pagina master</span><span class="sxs-lookup"><span data-stu-id="16074-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="16074-145">Aggiunta di pagine e spostamento</span><span class="sxs-lookup"><span data-stu-id="16074-145">Adding pages and navigation</span></span>
- <span data-ttu-id="16074-146">Visualizzazione dei dettagli del menu e dei dati del prodotto</span><span class="sxs-lookup"><span data-stu-id="16074-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="16074-147">Creazione di un carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="16074-147">Creating a shopping cart</span></span>
- <span data-ttu-id="16074-148">Aggiunta del supporto per SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="16074-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="16074-149">Aggiunta di un metodo di pagamento</span><span class="sxs-lookup"><span data-stu-id="16074-149">Adding a payment method</span></span>
- <span data-ttu-id="16074-150">Inclusione di un ruolo amministratore e di un utente per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="16074-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="16074-151">Limitazione dell'accesso a pagine e cartelle specifiche</span><span class="sxs-lookup"><span data-stu-id="16074-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="16074-152">Caricamento di un file nell'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="16074-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="16074-153">Implementazione della convalida dell'input</span><span class="sxs-lookup"><span data-stu-id="16074-153">Implementing input validation</span></span>
- <span data-ttu-id="16074-154">Registrazione delle route per l'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="16074-154">Registering routes for the web application</span></span>
- <span data-ttu-id="16074-155">Implementazione della gestione degli errori e della registrazione degli errori</span><span class="sxs-lookup"><span data-stu-id="16074-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="16074-156">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="16074-156">Overview</span></span>

<span data-ttu-id="16074-157">Questa serie di esercitazioni è destinata a chi ha familiarità con i concetti di programmazione, ma è una novità di ASP.NET Web Form.</span><span class="sxs-lookup"><span data-stu-id="16074-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="16074-158">Se si ha già familiarità con ASP.NET Web Forms, questa serie può comunque essere utile per apprendere le nuove funzionalità di ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16074-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="16074-159">Per i lettori che non hanno familiarità con i concetti di programmazione e i Web Form ASP.NET, vedere le esercitazioni aggiuntive su Web Form disponibili nella sezione [Introduzione](../../../index.md) del sito Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="16074-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="16074-160">Il ASP.NET 4,5 fornito in questa serie di esercitazioni include le seguenti funzionalità:</span><span class="sxs-lookup"><span data-stu-id="16074-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="16074-161">Interfaccia utente semplice per la creazione di progetti che offre [supporto per molti framework ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e API Web).</span><span class="sxs-lookup"><span data-stu-id="16074-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="16074-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), layout, tema e Framework di progettazione reattivo.</span><span class="sxs-lookup"><span data-stu-id="16074-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="16074-163">[ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software di hosting Web diverso da IIS.</span><span class="sxs-lookup"><span data-stu-id="16074-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="16074-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="16074-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="16074-165">Un aggiornamento alla Entity Framework consente di:</span><span class="sxs-lookup"><span data-stu-id="16074-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="16074-166">Recuperare e modificare i dati come oggetti fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="16074-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="16074-167">Accesso ai dati in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="16074-167">Access data asynchronously</span></span>
  - <span data-ttu-id="16074-168">Gestione degli errori di connessione temporanei</span><span class="sxs-lookup"><span data-stu-id="16074-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="16074-169">Log istruzioni SQL</span><span class="sxs-lookup"><span data-stu-id="16074-169">Log SQL statements</span></span>

<span data-ttu-id="16074-170">Per un elenco completo delle funzionalità di ASP.NET 4,5, vedere [ASP.NET and Web Tools per Visual Studio 2013 Note sulla versione](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="16074-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="16074-171">Applicazione di esempio Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="16074-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="16074-172">Gli screenshot seguenti rientrano nell'applicazione Web Form ASP.NET creata in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="16074-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="16074-173">Quando si esegue l'applicazione in Visual Studio, viene visualizzata la Home page Web seguente.</span><span class="sxs-lookup"><span data-stu-id="16074-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys-pagina predefinita](introduction-and-overview/_static/image1.png)

<span data-ttu-id="16074-175">È possibile registrarsi come nuovo utente o accedere come utente esistente.</span><span class="sxs-lookup"><span data-stu-id="16074-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="16074-176">La navigazione superiore include collegamenti alle categorie di prodotti e ai relativi prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="16074-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="16074-177">Se si seleziona **prodotti**, verranno visualizzati tutti i prodotti disponibili.</span><span class="sxs-lookup"><span data-stu-id="16074-177">If you select **Products**, all available products are displayed.</span></span> 

![Giocattoli Wingtip-prodotti](introduction-and-overview/_static/image2.png)

<span data-ttu-id="16074-179">Se si seleziona un prodotto specifico, vengono visualizzati i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="16074-179">If you select a specific product, product details are displayed.</span></span>

![Wingtip Toys-dettagli sul prodotto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="16074-181">Un utente può registrarsi e accedere con la funzionalità predefinita del modello Web Form.</span><span class="sxs-lookup"><span data-stu-id="16074-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="16074-182">Questa esercitazione illustra anche come eseguire l'accesso con un account Gmail esistente.</span><span class="sxs-lookup"><span data-stu-id="16074-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="16074-183">Inoltre, è possibile accedere come amministratore per aggiungere e rimuovere prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="16074-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys-accedi](introduction-and-overview/_static/image4.png)

<span data-ttu-id="16074-185">Dopo aver eseguito l'accesso come utente, è possibile aggiungere prodotti al carrello acquisti ed effettuare il checkout con PayPal.</span><span class="sxs-lookup"><span data-stu-id="16074-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="16074-186">L'applicazione di esempio è progettata per funzionare in sandbox per sviluppatori di PayPal.</span><span class="sxs-lookup"><span data-stu-id="16074-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="16074-187">Non si verifica alcuna transazione di denaro effettiva.</span><span class="sxs-lookup"><span data-stu-id="16074-187">No actual money transaction takes place.</span></span>

![Wingtip Toys-carrello acquisti](introduction-and-overview/_static/image5.png)

<span data-ttu-id="16074-189">PayPal conferma le informazioni relative all'account, all'ordine e al pagamento.</span><span class="sxs-lookup"><span data-stu-id="16074-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="16074-191">Dopo la restituzione da PayPal, è possibile esaminare e completare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="16074-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys-revisione degli ordini](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="16074-193">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="16074-193">Prerequisites</span></span>

<span data-ttu-id="16074-194">Prima di iniziare, verificare che nel computer sia installato il software seguente:</span><span class="sxs-lookup"><span data-stu-id="16074-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="16074-195">[Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="16074-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="16074-196">Il .NET Framework viene installato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="16074-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="16074-197">Questa serie di esercitazioni USA Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="16074-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="16074-198">Per completare questa serie di esercitazioni, è possibile usare tale o Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="16074-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="16074-199">Si noti quanto segue su Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="16074-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="16074-200">In questa serie di esercitazioni, Microsoft Visual Studio 2017 e Microsoft Visual Studio Community 2017 sono denominati *Visual Studio* .</span><span class="sxs-lookup"><span data-stu-id="16074-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="16074-201">Visual Studio 2017 è installato accanto a qualsiasi versione precedente già installata.</span><span class="sxs-lookup"><span data-stu-id="16074-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="16074-202">I siti creati nelle versioni precedenti possono essere aperti in Visual Studio 2017 e continuano a essere aperti nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="16074-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="16074-203">La prima volta che si avvia Visual Studio si presuppone che siano state selezionate le impostazioni per *lo sviluppo Web* .</span><span class="sxs-lookup"><span data-stu-id="16074-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="16074-204">Per altre informazioni, vedere [procedura: selezionare le impostazioni dell'ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="16074-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="16074-205">Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il progetto Web presentato in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="16074-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="16074-206">Scaricare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="16074-206">Download the sample application</span></span>

 <span data-ttu-id="16074-207">È possibile scaricare l'applicazione di esempio completata in qualsiasi momento dal sito degli esempi MSDN:</span><span class="sxs-lookup"><span data-stu-id="16074-207">You can download the completed sample application at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="16074-208">[Introduzione con Web form ASP.NET 4,5 Visual Studio 2013 e Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="16074-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="16074-209">Questo download include gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="16074-209">This download has the following items:</span></span>

- <span data-ttu-id="16074-210">Applicazione di esempio nella cartella *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="16074-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="16074-211">Risorse usate per creare l'applicazione di esempio nella cartella *WingtipToys-assets* nella cartella *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="16074-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="16074-212">Il download è un file con *estensione zip* .</span><span class="sxs-lookup"><span data-stu-id="16074-212">The download is a *.zip* file.</span></span> <span data-ttu-id="16074-213">Per visualizzare il progetto completato creato da questa serie di esercitazioni, trovare e *C#* selezionare la cartella nel file con estensione zip.</span><span class="sxs-lookup"><span data-stu-id="16074-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="16074-214">Salvare la C# cartella nella cartella utilizzata per lavorare con i progetti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16074-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="16074-215">Per impostazione predefinita, la cartella progetti di Visual Studio 2017 è:</span><span class="sxs-lookup"><span data-stu-id="16074-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="16074-216"><strong>C:\Users&#92;</strong>  <strong><em>&lt;username&gt;</em></strong> <strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="16074-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="16074-217">Rinominare la ***C#*** cartella in ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="16074-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="16074-218">Se è già presente una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente la cartella esistente prima di rinominare la *C#* cartella in *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="16074-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="16074-219">Per eseguire il progetto completato, aprire la cartella *WingtipToys* e fare doppio clic sul file *WingtipToys. sln* .</span><span class="sxs-lookup"><span data-stu-id="16074-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="16074-220">Visual Studio 2017 apre il progetto.</span><span class="sxs-lookup"><span data-stu-id="16074-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="16074-221">Fare quindi clic con il pulsante destro del mouse sul file *default. aspx* in **Esplora soluzioni** e selezionare **Visualizza nel browser**.</span><span class="sxs-lookup"><span data-stu-id="16074-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="16074-222">Eseguire un quiz Web Form ASP.NET per esaminare il contenuto</span><span class="sxs-lookup"><span data-stu-id="16074-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="16074-223">Dopo aver completato la serie di esercitazioni, eseguire un quiz per testare le proprie conoscenze e rafforzare i concetti chiave.</span><span class="sxs-lookup"><span data-stu-id="16074-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="16074-224">Ogni domanda fornisce una spiegazione e collegamenti a istruzioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="16074-224">Each question provides an explanation and links to additional guidance.</span></span>

* [<span data-ttu-id="16074-225">Quiz Web Form ASP.NET</span><span class="sxs-lookup"><span data-stu-id="16074-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="16074-226">Supporto e commenti per l'esercitazione</span><span class="sxs-lookup"><span data-stu-id="16074-226">Tutorial support and comments</span></span>

<span data-ttu-id="16074-227">Per domande e commenti, usare la sezione Q e una inclusa nella pagina [di esempio introduzione con ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="16074-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="16074-228">I commenti su questa serie di esercitazioni sono benvenuti.</span><span class="sxs-lookup"><span data-stu-id="16074-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="16074-229">Quando questa serie di esercitazioni viene aggiornata, viene eseguito ogni sforzo per prendere in considerazione le correzioni o i suggerimenti per i miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="16074-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="16074-230">Se si verifica un errore, i messaggi di errore corrispondenti potrebbero generare confusione, senza alcuna spiegazione adeguata su come risolverlo.</span><span class="sxs-lookup"><span data-stu-id="16074-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="16074-231">Per informazioni, è possibile consultare i [Forum di ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="16074-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="16074-232">Un'altra fonte interessante è la sezione Q e una della pagina di esempio [Introduzione con ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="16074-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="16074-233">avanti</span><span class="sxs-lookup"><span data-stu-id="16074-233">Next</span></span>](create-the-project.md)
