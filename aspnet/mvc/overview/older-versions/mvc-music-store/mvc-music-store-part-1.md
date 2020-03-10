---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: panoramica e nuovo progetto di file > | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 1 vengono illustrati i Cenni preliminari e > nuovo progetto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559983"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="c0ce9-104">Parte 1: panoramica e nuovo progetto di > file</span><span class="sxs-lookup"><span data-stu-id="c0ce9-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="c0ce9-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c0ce9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c0ce9-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c0ce9-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c0ce9-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c0ce9-109">Nella parte 1 vengono illustrati i Cenni preliminari e&gt;nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="c0ce9-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c0ce9-110">Overview</span></span>

<span data-ttu-id="c0ce9-111">MVC Music Store è un'applicazione di esercitazione che introduce e illustra in modo dettagliato come utilizzare ASP.NET MVC e Visual Web Developer per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="c0ce9-112">Inizieremo lentamente, quindi l'esperienza di sviluppo Web a livello di principianti è accettabile.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="c0ce9-113">L'applicazione che verrà compilata è un semplice negozio di musica.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="c0ce9-114">L'applicazione include tre parti principali: acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="c0ce9-115">I visitatori possono sfogliare gli album per genere:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="c0ce9-116">Possono visualizzare un singolo album e aggiungerlo al carrello:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="c0ce9-117">Possono esaminare il carrello, rimuovendo tutti gli elementi che non vogliono più:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="c0ce9-118">Continuando con l'estrazione verrà richiesto di effettuare l'accesso o di registrarsi per un account utente.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="c0ce9-119">Dopo aver creato un account, è possibile completare l'ordine compilando le informazioni di spedizione e pagamento.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="c0ce9-120">Per semplificare le cose, stiamo eseguendo una promozione straordinaria: tutti gli elementi sono gratuiti se entrano nel codice promozione "FREE".</span><span class="sxs-lookup"><span data-stu-id="c0ce9-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="c0ce9-121">Dopo l'ordinamento, viene visualizzata una schermata di conferma semplice:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="c0ce9-122">Oltre alle pagine rivolte ai clienti, viene anche compilata una sezione amministratore che mostra un elenco di album da cui gli amministratori possono creare, modificare ed eliminare gli album:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="c0ce9-123">1. file-&gt; nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="c0ce9-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="c0ce9-124">Installazione del software</span><span class="sxs-lookup"><span data-stu-id="c0ce9-124">Installing the software</span></span>

<span data-ttu-id="c0ce9-125">Questa esercitazione inizierà con la creazione di un nuovo progetto ASP.NET MVC 3 con la versione gratuita di Visual Web Developer 2010 Express (gratuita), quindi verranno aggiunte in modo incrementale le funzionalità per creare un'applicazione completa funzionante.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="c0ce9-126">Verranno illustrati l'accesso al database, gli scenari di inserimento dei moduli, la convalida dei dati, l'utilizzo di pagine master per un layout di pagina coerente, l'utilizzo di AJAX per gli aggiornamenti e la convalida della pagina, l'accesso degli utenti e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="c0ce9-127">È possibile seguire la procedura dettagliata oppure è possibile scaricare l'applicazione completata da [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="c0ce9-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="c0ce9-128">Per compilare l'applicazione, è possibile utilizzare Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versione gratuita di Visual Studio 2010).</span><span class="sxs-lookup"><span data-stu-id="c0ce9-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="c0ce9-129">Verrà usato il SQL Server Compact (anche gratuito) per ospitare il database.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="c0ce9-130">Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="c0ce9-131">[Prerequisiti di Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="c0ce9-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="c0ce9-132">[Aggiornamento degli strumenti di ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="c0ce9-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="c0ce9-133">[SQL Server Compact 4,0]-inclusione del supporto per Runtime e strumenti</span><span class="sxs-lookup"><span data-stu-id="c0ce9-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="c0ce9-134">Creazione di un nuovo progetto MVC 3 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c0ce9-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="c0ce9-135">Per iniziare, selezionare "nuovo progetto" dal menu file in Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="c0ce9-136">Verrà visualizzata la finestra di dialogo nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="c0ce9-137">Selezionare il gruppo Visual C# -&gt; Web Templates a sinistra, quindi scegliere il modello "ASP.NET MVC 3 Web Application" nella colonna centrale.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="c0ce9-138">Denominare il progetto MvcMusicStore e premere il pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="c0ce9-139">Verrà visualizzata una finestra di dialogo secondaria che consente di apportare alcune impostazioni specifiche di MVC per il progetto.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="c0ce9-140">Selezionare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-140">Select the following:</span></span>

<span data-ttu-id="c0ce9-141">Modello di progetto-selezionare vuoto</span><span class="sxs-lookup"><span data-stu-id="c0ce9-141">Project Template - select Empty</span></span>

<span data-ttu-id="c0ce9-142">Motore di visualizzazione-seleziona Razor</span><span class="sxs-lookup"><span data-stu-id="c0ce9-142">View Engine - select Razor</span></span>

<span data-ttu-id="c0ce9-143">Usa markup semantico HTML5-selezionato</span><span class="sxs-lookup"><span data-stu-id="c0ce9-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="c0ce9-144">Verificare che le impostazioni siano indicate di seguito, quindi fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="c0ce9-145">Verrà creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-145">This will create our project.</span></span> <span data-ttu-id="c0ce9-146">Verranno ora esaminate le cartelle che sono state aggiunte all'applicazione nella Esplora soluzioni sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="c0ce9-147">Il modello MVC 3 vuoto non è completamente vuoto, ma aggiunge una struttura di cartelle di base:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="c0ce9-148">ASP.NET MVC usa alcune convenzioni di denominazione di base per i nomi delle cartelle:</span><span class="sxs-lookup"><span data-stu-id="c0ce9-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="c0ce9-149">**Cartella**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-149">**Folder**</span></span> | <span data-ttu-id="c0ce9-150">**Scopo**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="c0ce9-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-151">**/Controllers**</span></span> | <span data-ttu-id="c0ce9-152">I controller rispondono all'input dal browser, decidono cosa fare con esso e restituiscono la risposta all'utente.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="c0ce9-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-153">**/Views**</span></span> | <span data-ttu-id="c0ce9-154">Viste che contengono i modelli dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="c0ce9-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="c0ce9-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-155">**/Models**</span></span> | <span data-ttu-id="c0ce9-156">I modelli contengono e modificano i dati</span><span class="sxs-lookup"><span data-stu-id="c0ce9-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="c0ce9-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-157">**/Content**</span></span> | <span data-ttu-id="c0ce9-158">Questa cartella include immagini, CSS e qualsiasi altro contenuto statico</span><span class="sxs-lookup"><span data-stu-id="c0ce9-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="c0ce9-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="c0ce9-159">**/Scripts**</span></span> | <span data-ttu-id="c0ce9-160">Questa cartella include i file JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0ce9-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="c0ce9-161">Queste cartelle sono incluse anche in un'applicazione MVC ASP.NET vuota perché per impostazione predefinita il framework ASP.NET MVC usa un approccio di "Convenzione sulla configurazione" ed esegue alcuni presupposti predefiniti in base alle convenzioni di denominazione delle cartelle.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="c0ce9-162">Per impostazione predefinita, ad esempio, i controller cercano le visualizzazioni nella cartella Views senza che sia necessario specificarlo in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="c0ce9-163">Con le convenzioni predefinite è possibile ridurre la quantità di codice che è necessario scrivere e consentire agli altri sviluppatori di comprendere il progetto in modo più semplice.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="c0ce9-164">Queste convenzioni verranno spiegate più in base alla compilazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c0ce9-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c0ce9-165">avanti</span><span class="sxs-lookup"><span data-stu-id="c0ce9-165">Next</span></span>](mvc-music-store-part-2.md)
