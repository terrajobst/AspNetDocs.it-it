---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su MVC 4 ASP.NET | Microsoft Docs
author: rick-anderson
description: Questa esercitazione pratica si basa su un archivio musicale MVC (Model View Controller), un'applicazione di esercitazione che introduce e spiega in modo dettagliato come usare ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598819"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="36976-103">Concetti fondamentali su ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="36976-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="36976-104">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="36976-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="36976-105">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="36976-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="36976-106">Questa esercitazione pratica si basa su un negozio di musica MVC (Model View Controller), un'applicazione di esercitazione che introduce e spiega dettagliatamente come usare ASP.NET MVC e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="36976-107">Nel corso dell'esercitazione si apprenderà la semplicità, ma anche il potere di usare queste tecnologie insieme.</span><span class="sxs-lookup"><span data-stu-id="36976-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="36976-108">Si inizierà con una semplice applicazione che verrà compilata fino a quando non si dispone di un'applicazione Web MVC 4 ASP.NET completamente funzionante.</span><span class="sxs-lookup"><span data-stu-id="36976-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="36976-109">Questo Lab funziona con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="36976-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="36976-110">Se si vuole esplorare la versione ASP.NET MVC 3 dell'applicazione tutorial, è possibile trovarla in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="36976-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="36976-111">Questo laboratorio pratico presuppone che lo sviluppatore abbia esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="36976-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="36976-112">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di training di Web Camps, disponibile in [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="36976-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="36976-113">Il progetto specifico di questo Lab è disponibile in [nozioni fondamentali su ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="36976-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="36976-114">Applicazione Music Store</span><span class="sxs-lookup"><span data-stu-id="36976-114">The Music Store application</span></span>

<span data-ttu-id="36976-115">L'applicazione Web Music Store che verrà compilata in questo Lab comprende tre parti principali: acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="36976-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="36976-116">I visitatori potranno sfogliare gli album per genere, aggiungere album al carrello, rivedere la selezione e infine procedere con il checkout per accedere e completare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="36976-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="36976-117">Inoltre, gli amministratori dei negozi saranno in grado di gestire gli album disponibili, oltre alle relative proprietà principali.</span><span class="sxs-lookup"><span data-stu-id="36976-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="36976-118">![Schermate di Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "Schermate di Music Store")</span><span class="sxs-lookup"><span data-stu-id="36976-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="36976-119">*Schermate di Music Store*</span><span class="sxs-lookup"><span data-stu-id="36976-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="36976-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="36976-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="36976-121">L'applicazione Music Store verrà compilata usando **MVC (Model View Controller)** , un modello architetturale che separa un'applicazione in tre componenti principali:</span><span class="sxs-lookup"><span data-stu-id="36976-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="36976-122">**Modelli**: gli oggetti modello sono le parti dell'applicazione che implementano la logica di dominio.</span><span class="sxs-lookup"><span data-stu-id="36976-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="36976-123">Spesso, anche gli oggetti modello recuperano e archiviano lo stato del modello in un database.</span><span class="sxs-lookup"><span data-stu-id="36976-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="36976-124">**Visualizzazioni:** Le visualizzazioni sono i componenti che visualizzano l'interfaccia utente (UI) dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="36976-125">Questa interfaccia utente viene in genere creata in base ai dati del modello.</span><span class="sxs-lookup"><span data-stu-id="36976-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="36976-126">Un esempio è la visualizzazione di modifica degli album che visualizza caselle di testo e un elenco a discesa basato sullo stato corrente di un oggetto album.</span><span class="sxs-lookup"><span data-stu-id="36976-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="36976-127">**Controller:** I controller sono i componenti che gestiscono l'interazione dell'utente, manipolano il modello e infine selezionano una visualizzazione per il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="36976-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="36976-128">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="36976-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="36976-129">Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi.</span><span class="sxs-lookup"><span data-stu-id="36976-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="36976-130">Questa separazione consente di gestire la complessità quando si compila un'applicazione, perché consente di concentrarsi su un aspetto dell'implementazione alla volta.</span><span class="sxs-lookup"><span data-stu-id="36976-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="36976-131">Inoltre, il modello MVC semplifica il test delle applicazioni, incoraggiando l'uso dello sviluppo basato su test (TDD) per la creazione di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="36976-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="36976-132">Il Framework **ASP.NET MVC** fornisce un'alternativa al modello web form ASP.NET per la creazione di applicazioni Web basate su MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="36976-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="36976-133">Il Framework **ASP.NET MVC** è un Framework di presentazione leggero e altamente testabile che, come per le applicazioni basate su Web Form, è integrato con le funzionalità di ASP.NET esistenti, ad esempio le pagine master e l'autenticazione basata sull'appartenenza, per sfruttare tutte le potenzialità di .NET Framework di base.</span><span class="sxs-lookup"><span data-stu-id="36976-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="36976-134">Questa opzione è utile se si ha già familiarità con ASP.NET Web Form perché tutte le librerie già usate sono disponibili anche in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="36976-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="36976-135">Inoltre, l'accoppiamento libero tra i tre componenti principali di un'applicazione MVC promuove anche lo sviluppo parallelo.</span><span class="sxs-lookup"><span data-stu-id="36976-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="36976-136">Ad esempio, uno sviluppatore può lavorare sulla vista, un secondo sviluppatore può lavorare sulla logica del controller e un terzo sviluppatore può concentrarsi sulla logica di business nel modello.</span><span class="sxs-lookup"><span data-stu-id="36976-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="36976-137">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="36976-137">Objectives</span></span>

<span data-ttu-id="36976-138">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="36976-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="36976-139">Creare un'applicazione MVC ASP.NET da zero in base all'esercitazione sull'applicazione Music Store</span><span class="sxs-lookup"><span data-stu-id="36976-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="36976-140">Aggiungere controller per gestire gli URL alla Home page del sito e per esplorare la funzionalità principale</span><span class="sxs-lookup"><span data-stu-id="36976-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="36976-141">Aggiungere una visualizzazione per personalizzare il contenuto visualizzato insieme al relativo stile</span><span class="sxs-lookup"><span data-stu-id="36976-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="36976-142">Aggiungere classi di modelli per contenere e gestire i dati e la logica di dominio</span><span class="sxs-lookup"><span data-stu-id="36976-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="36976-143">Usare il modello di visualizzazione del modello per passare informazioni dalle azioni del controller ai modelli di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="36976-144">Esplorare il nuovo modello ASP.NET MVC 4 per le applicazioni Internet</span><span class="sxs-lookup"><span data-stu-id="36976-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="36976-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="36976-145">Prerequisites</span></span>

<span data-ttu-id="36976-146">Per completare il Lab, è necessario disporre degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="36976-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="36976-147">[Visual Studio 2012 Express per il Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leggere [l'appendice a](#AppendixA) per istruzioni su come installarlo)</span><span class="sxs-lookup"><span data-stu-id="36976-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="36976-148">Configurazione</span><span class="sxs-lookup"><span data-stu-id="36976-148">Setup</span></span>

<span data-ttu-id="36976-149">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="36976-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="36976-150">Per praticità, gran parte del codice che verrà gestito insieme a questo Lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="36976-151">Per installare i frammenti di codice, eseguire il file **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="36976-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="36976-152">Se non si ha familiarità con i frammenti di Visual Studio Code e si desidera apprendere come utilizzarli, è possibile fare riferimento all'appendice di questo documento &quot;[Appendice C: uso dei frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="36976-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="36976-153">Esercizi</span><span class="sxs-lookup"><span data-stu-id="36976-153">Exercises</span></span>

<span data-ttu-id="36976-154">Questo laboratorio pratico è costituito dagli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="36976-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="36976-155">Esercizio 1: creazione del progetto di applicazione Web MVC MusicStore ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="36976-156">Esercizio 2: creazione di un controller</span><span class="sxs-lookup"><span data-stu-id="36976-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="36976-157">Esercizio 3: passaggio di parametri a un controller</span><span class="sxs-lookup"><span data-stu-id="36976-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="36976-158">Esercizio 4: creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="36976-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="36976-159">Esercizio 5: creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="36976-160">Esercizio 6: utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="36976-161">Esercizio 7: un giro intorno A ASP.NET MVC 4 nuovo modello</span><span class="sxs-lookup"><span data-stu-id="36976-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="36976-162">Ogni esercizio è accompagnato da una cartella **finale** che contiene la soluzione risultante che è necessario ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="36976-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="36976-163">È possibile utilizzare questa soluzione come guida se è necessario ulteriore supporto per gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="36976-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="36976-164">Tempo stimato per il completamento del Lab: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="36976-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="36976-165">Esercizio 1: creazione del progetto di applicazione Web MVC MusicStore ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="36976-166">In questo esercizio si apprenderà come creare un'applicazione MVC ASP.NET in Visual Studio 2012 Express per il Web, oltre che nell'organizzazione principale della cartella.</span><span class="sxs-lookup"><span data-stu-id="36976-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="36976-167">Inoltre, si apprenderà come aggiungere un nuovo controller e come visualizzarne una semplice stringa nella home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="36976-168">Attività 1: creazione del progetto di applicazione Web MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="36976-169">In questa attività verrà creato un progetto di applicazione MVC ASP.NET vuoto usando il modello MVC di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="36976-170">Avviare **vs Express per il Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="36976-171">Scegliere **Nuovo progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="36976-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="36976-172">Nella finestra di dialogo **nuovo progetto** selezionare il tipo di progetto **applicazione Web MVC 4 ASP.NET** , che si trova in  **C#visuale,** elenco modelli **Web** .</span><span class="sxs-lookup"><span data-stu-id="36976-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="36976-173">Modificare il **nome** in *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="36976-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="36976-174">Impostare il percorso della soluzione all'interno di una nuova cartella di **inizio** nella cartella di origine dell'esercizio, ad esempio **[your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="36976-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="36976-175">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="36976-175">Click **OK**.</span></span>

    <span data-ttu-id="36976-176">![Finestra di dialogo Crea nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "Finestra di dialogo Crea nuovo progetto")</span><span class="sxs-lookup"><span data-stu-id="36976-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="36976-177">*Finestra di dialogo Crea nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="36976-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="36976-178">Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare il modello di **base** e verificare che il **motore di visualizzazione** selezionato sia **Razor**.</span><span class="sxs-lookup"><span data-stu-id="36976-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="36976-179">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="36976-179">Click **OK**.</span></span>

    <span data-ttu-id="36976-180">![Finestra di dialogo nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-fundamentals/_static/image3.png "Finestra di dialogo nuovo progetto MVC 4 ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="36976-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="36976-181">*Finestra di dialogo nuovo progetto MVC 4 ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="36976-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="36976-182">Attività 2: esplorazione della struttura della soluzione</span><span class="sxs-lookup"><span data-stu-id="36976-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="36976-183">Il framework ASP.NET MVC include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supportano il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="36976-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="36976-184">Questo modello consente di creare una nuova applicazione Web MVC ASP.NET con le cartelle, i modelli di elemento e le voci del file di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="36976-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="36976-185">In questa attività verrà esaminata la struttura della soluzione per comprendere gli elementi interessati e le relative relazioni.</span><span class="sxs-lookup"><span data-stu-id="36976-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="36976-186">Le cartelle seguenti sono incluse in tutte le applicazioni MVC ASP.NET, perché per impostazione predefinita il framework ASP.NET MVC usa una convenzione di &quot;rispetto alla configurazione&quot; ed esegue alcuni presupposti predefiniti in base alle convenzioni di denominazione delle cartelle.</span><span class="sxs-lookup"><span data-stu-id="36976-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="36976-187">Una volta creato il progetto, esaminare la struttura di cartelle creata nella Esplora soluzioni sul lato destro:</span><span class="sxs-lookup"><span data-stu-id="36976-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="36976-188">![Struttura di cartelle MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "Struttura di cartelle MVC ASP.NET in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="36976-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="36976-189">*Struttura di cartelle MVC ASP.NET in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="36976-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="36976-190">**Controller**.</span><span class="sxs-lookup"><span data-stu-id="36976-190">**Controllers**.</span></span> <span data-ttu-id="36976-191">Questa cartella conterrà le classi controller.</span><span class="sxs-lookup"><span data-stu-id="36976-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="36976-192">In un'applicazione basata su MVC, i controller sono responsabili della gestione dell'interazione dell'utente finale, della manipolazione del modello e infine della scelta di una visualizzazione per il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="36976-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="36976-193">Il framework MVC richiede che i nomi di tutti i controller terminino con &quot;controller&quot;, ad esempio HomeController, LoginController o ProductController.</span><span class="sxs-lookup"><span data-stu-id="36976-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="36976-194">**Modelli**.</span><span class="sxs-lookup"><span data-stu-id="36976-194">**Models**.</span></span> <span data-ttu-id="36976-195">Questa cartella viene fornita per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC.</span><span class="sxs-lookup"><span data-stu-id="36976-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="36976-196">Questo include in genere il codice che definisce gli oggetti e la logica per interagire con l'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="36976-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="36976-197">Gli oggetti dei modelli effettivi si troveranno in genere in librerie di classi separate.</span><span class="sxs-lookup"><span data-stu-id="36976-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="36976-198">Tuttavia, quando si crea una nuova applicazione, è possibile includere le classi e quindi spostarle in librerie di classi separate in un momento successivo del ciclo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="36976-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="36976-199">**Viste**.</span><span class="sxs-lookup"><span data-stu-id="36976-199">**Views**.</span></span> <span data-ttu-id="36976-200">Questa cartella è la posizione consigliata per le visualizzazioni, ovvero i componenti responsabili della visualizzazione dell'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="36976-201">Le visualizzazioni utilizzano i file. aspx,. ascx,. cshtml e. master, oltre a tutti gli altri file correlati alle visualizzazioni di rendering.</span><span class="sxs-lookup"><span data-stu-id="36976-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="36976-202">La cartella Views contiene una cartella per ogni controller. la cartella viene denominata con il prefisso del nome del controller.</span><span class="sxs-lookup"><span data-stu-id="36976-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="36976-203">Se, ad esempio, si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella denominata Home.</span><span class="sxs-lookup"><span data-stu-id="36976-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="36976-204">Per impostazione predefinita, quando il Framework di MVC ASP.NET carica una visualizzazione, Cerca un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\nomecontroller viene eseguita (**views [controllerName] [Action]. aspx**) o (**views [controllerName] [Action]. cshtml**) per le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="36976-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-205">Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il file **Global. asax** per impostare le impostazioni predefinite globali per il routing degli URL e utilizza il file **Web. config** per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="36976-206">Attività 3-aggiunta di un HomeController</span><span class="sxs-lookup"><span data-stu-id="36976-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="36976-207">Nelle applicazioni ASP.NET che non usano il framework MVC, l'interazione dell'utente è organizzata intorno alle pagine e in merito alla generazione e alla gestione degli eventi da tali pagine.</span><span class="sxs-lookup"><span data-stu-id="36976-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="36976-208">Al contrario, l'interazione dell'utente con le applicazioni MVC ASP.NET è organizzata attorno ai controller e ai relativi metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="36976-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="36976-209">D'altra parte, ASP.NET MVC Framework esegue il mapping degli URL alle classi definite controller.</span><span class="sxs-lookup"><span data-stu-id="36976-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="36976-210">I controller elaborano le richieste in ingresso, gestiscono l'input e le interazioni dell'utente, eseguono la logica dell'applicazione appropriata e determinano la risposta da restituire al client (visualizzazione HTML, download di un file, Reindirizzamento a un URL diverso e così via).</span><span class="sxs-lookup"><span data-stu-id="36976-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="36976-211">Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="36976-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="36976-212">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="36976-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="36976-213">In questa attività verrà aggiunta una classe controller che gestirà gli URL alla Home page del sito di Music Store.</span><span class="sxs-lookup"><span data-stu-id="36976-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="36976-214">Fare clic con il pulsante destro del mouse sulla cartella **controller** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi comando **controller** :</span><span class="sxs-lookup"><span data-stu-id="36976-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="36976-215">![Aggiungere un comando controller](aspnet-mvc-4-fundamentals/_static/image5.png "Aggiungere un comando controller")</span><span class="sxs-lookup"><span data-stu-id="36976-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="36976-216">*Comando Aggiungi controller*</span><span class="sxs-lookup"><span data-stu-id="36976-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="36976-217">Verrà visualizzata la finestra di dialogo **Aggiungi controller** .</span><span class="sxs-lookup"><span data-stu-id="36976-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="36976-218">Assegnare al controller il nome *HomeController* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="36976-219">![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-fundamentals/_static/image6.png "Finestra di dialogo Aggiungi controller")</span><span class="sxs-lookup"><span data-stu-id="36976-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="36976-220">*Finestra di dialogo Aggiungi controller*</span><span class="sxs-lookup"><span data-stu-id="36976-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="36976-221">Il file **HomeController.cs** viene creato nella cartella **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="36976-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="36976-222">Per fare in modo che **HomeController** restituisca una stringa nella relativa azione di indice, sostituire il metodo **index** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="36976-223">(Frammento di codice- *nozioni fondamentali su ASP.NET MVC 4-Indice HomeController EX1*)</span><span class="sxs-lookup"><span data-stu-id="36976-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="36976-224">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="36976-225">In questa attività si tenterà di provare l'applicazione in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="36976-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="36976-226">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="36976-227">Il progetto viene compilato e viene avviato il server Web IIS locale.</span><span class="sxs-lookup"><span data-stu-id="36976-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="36976-228">Il server Web IIS locale aprirà automaticamente un Web browser che punta all'URL del server Web.</span><span class="sxs-lookup"><span data-stu-id="36976-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="36976-229">![Applicazione in esecuzione in un Web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Applicazione in esecuzione in un Web browser")</span><span class="sxs-lookup"><span data-stu-id="36976-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="36976-230">*Applicazione in esecuzione in un Web browser*</span><span class="sxs-lookup"><span data-stu-id="36976-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-231">Il server Web IIS locale eseguirà il sito Web con un numero di porta libero casuale.</span><span class="sxs-lookup"><span data-stu-id="36976-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="36976-232">Nella figura precedente il sito è in esecuzione in `http://localhost:50103/`, quindi usa la porta 50103.</span><span class="sxs-lookup"><span data-stu-id="36976-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="36976-233">Il numero di porta può variare.</span><span class="sxs-lookup"><span data-stu-id="36976-233">Your port number may vary.</span></span>
2. <span data-ttu-id="36976-234">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="36976-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="36976-235">Esercizio 2: creazione di un controller</span><span class="sxs-lookup"><span data-stu-id="36976-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="36976-236">In questo esercizio verrà illustrato come aggiornare il controller per implementare la semplice funzionalità dell'applicazione Music Store.</span><span class="sxs-lookup"><span data-stu-id="36976-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="36976-237">Il controller definirà i metodi di azione per gestire ognuna delle richieste specifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="36976-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="36976-238">Una pagina di elenco dei generi musicali in Music Store</span><span class="sxs-lookup"><span data-stu-id="36976-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="36976-239">Pagina Sfoglia che elenca tutti gli album musicali per un determinato genere</span><span class="sxs-lookup"><span data-stu-id="36976-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="36976-240">Pagina dei dettagli che mostra le informazioni su uno specifico album musicale</span><span class="sxs-lookup"><span data-stu-id="36976-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="36976-241">Per l'ambito di questo esercizio, tali azioni restituiranno semplicemente una stringa a questo punto.</span><span class="sxs-lookup"><span data-stu-id="36976-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="36976-242">Attività 1-aggiunta di una nuova classe StoreController</span><span class="sxs-lookup"><span data-stu-id="36976-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="36976-243">In questa attività verrà aggiunto un nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="36976-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="36976-244">Se non è già aperto, avviare **VS Express per il Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="36976-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="36976-245">Nel menu **file** scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="36976-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="36976-246">Nella finestra di dialogo Apri progetto passare a **Source\Ex02-CreatingAController\Begin**, selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="36976-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="36976-247">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="36976-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="36976-248">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="36976-249">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36976-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="36976-250">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="36976-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="36976-251">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="36976-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-252">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="36976-253">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="36976-254">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="36976-255">Aggiungere il nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="36976-255">Add the new controller.</span></span> <span data-ttu-id="36976-256">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Controllers** all'interno del Esplora soluzioni, selezionare **Aggiungi** e quindi il comando **controller** .</span><span class="sxs-lookup"><span data-stu-id="36976-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="36976-257">Modificare il **nome del controller** in *StoreController*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="36976-258">![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-fundamentals/_static/image8.png "Finestra di dialogo Aggiungi controller")</span><span class="sxs-lookup"><span data-stu-id="36976-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="36976-259">*Finestra di dialogo Aggiungi controller*</span><span class="sxs-lookup"><span data-stu-id="36976-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="36976-260">Attività 2: modifica delle azioni di StoreController</span><span class="sxs-lookup"><span data-stu-id="36976-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="36976-261">In questa attività verranno modificati i metodi del controller chiamati **azioni**.</span><span class="sxs-lookup"><span data-stu-id="36976-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="36976-262">Le azioni sono responsabili della gestione delle richieste URL e della determinazione del contenuto da restituire al browser o all'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="36976-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="36976-263">La classe **StoreController** dispone già di un metodo **index** .</span><span class="sxs-lookup"><span data-stu-id="36976-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="36976-264">Verrà usato in un secondo momento in questo Lab per implementare la pagina in cui sono elencati tutti i generi di Music Store.</span><span class="sxs-lookup"><span data-stu-id="36976-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="36976-265">Per il momento, è sufficiente sostituire il metodo **index** con il codice seguente che restituisce una stringa &quot;Hello da Store. index ()&quot;:</span><span class="sxs-lookup"><span data-stu-id="36976-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="36976-266">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Indice StoreController EX2*)</span><span class="sxs-lookup"><span data-stu-id="36976-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="36976-267">Aggiungere i metodi **Browse** e **Details** .</span><span class="sxs-lookup"><span data-stu-id="36976-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="36976-268">A tale scopo, aggiungere il codice seguente a **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="36976-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="36976-269">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="36976-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="36976-270">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="36976-271">In questa attività si tenterà di provare l'applicazione in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="36976-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="36976-272">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-273">Il progetto viene avviato nella **Home** page.</span><span class="sxs-lookup"><span data-stu-id="36976-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="36976-274">Modificare l'URL per verificare l'implementazione di ogni azione.</span><span class="sxs-lookup"><span data-stu-id="36976-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="36976-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="36976-275">**/Store**.</span></span> <span data-ttu-id="36976-276">Viene visualizzato **&quot;Hello from Store. index ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="36976-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="36976-277">**/Store/browse**.</span><span class="sxs-lookup"><span data-stu-id="36976-277">**/Store/Browse**.</span></span> <span data-ttu-id="36976-278">Viene visualizzato **&quot;Hello from Store. browse ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="36976-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="36976-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="36976-279">**/Store/Details**.</span></span> <span data-ttu-id="36976-280">Viene visualizzato **&quot;Hello from Store. Details ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="36976-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="36976-281">![Esplorazione di StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Esplorazione di StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="36976-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="36976-282">*Esplorazione di/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="36976-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="36976-283">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="36976-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="36976-284">Esercizio 3: passaggio di parametri a un controller</span><span class="sxs-lookup"><span data-stu-id="36976-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="36976-285">Fino ad ora, sono state restituite stringhe costanti dai controller.</span><span class="sxs-lookup"><span data-stu-id="36976-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="36976-286">In questo esercizio verrà illustrato come passare i parametri a un controller usando l'URL e la QueryString, quindi le azioni del metodo rispondono con il testo al browser.</span><span class="sxs-lookup"><span data-stu-id="36976-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="36976-287">Attività 1: aggiunta del parametro genre a StoreController</span><span class="sxs-lookup"><span data-stu-id="36976-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="36976-288">In questa attività verrà utilizzata la **QueryString** per inviare parametri al metodo di azione **Browse** in **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="36976-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="36976-289">Se non è già aperto, avviare **vs Express per il Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="36976-290">Nel menu **file** scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="36976-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="36976-291">Nella finestra di dialogo Apri progetto passare a **Source\Ex03-PassingParametersToAController\Begin**, selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="36976-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="36976-292">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="36976-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="36976-293">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="36976-294">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36976-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="36976-295">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="36976-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="36976-296">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="36976-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-297">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="36976-298">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="36976-299">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="36976-300">Aprire la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="36976-300">Open **StoreController** class.</span></span> <span data-ttu-id="36976-301">A tale scopo, nella **Esplora soluzioni**espandere la cartella **Controllers** e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="36976-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="36976-302">Modificare il metodo **Browse** aggiungendo un parametro di stringa da richiedere per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="36976-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="36976-303">ASP.NET MVC passa automaticamente i parametri QueryString o post di form denominati **genre** a questo metodo di azione quando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="36976-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="36976-304">A tale scopo, sostituire il metodo **Browse** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="36976-305">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="36976-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="36976-306">Si usa il metodo di utilità **HttpUtility. HtmlEncode** per impedire agli utenti di inserire JavaScript nella visualizzazione con un collegamento come **/Store/browse? Genre =&lt;script&gt;finestra. location ='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="36976-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="36976-307">Per ulteriori informazioni, visitare [questo articolo di MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="36976-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="36976-308">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="36976-309">In questa attività si proverà l'applicazione in un Web browser e si userà il parametro **genre** .</span><span class="sxs-lookup"><span data-stu-id="36976-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="36976-310">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-311">Il progetto viene avviato nella **Home** page.</span><span class="sxs-lookup"><span data-stu-id="36976-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="36976-312">Modificare l'URL in */Store/browse? Genre = discoteca* per verificare che l'azione riceva il parametro Genre.</span><span class="sxs-lookup"><span data-stu-id="36976-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="36976-313">![Esplorazione di StoreBrowseGenre = discoteca](aspnet-mvc-4-fundamentals/_static/image10.png "Esplorazione di StoreBrowseGenre = discoteca")</span><span class="sxs-lookup"><span data-stu-id="36976-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="36976-314">*Esplorazione di/Store/Browse Genere = discoteca*</span><span class="sxs-lookup"><span data-stu-id="36976-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="36976-315">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="36976-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="36976-316">Attività 3: aggiunta di un parametro ID incorporato nell'URL</span><span class="sxs-lookup"><span data-stu-id="36976-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="36976-317">In questa attività si userà l' **URL** per passare un parametro **ID** al metodo di azione **Details** di **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="36976-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="36976-318">La convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come parametro denominato **ID**. Se il metodo di azione dispone di un parametro denominato ID, ASP.NET MVC passa automaticamente il segmento dell'URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="36976-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="36976-319">In URL **Store/Details/5**l' **ID** verrà interpretato come **5**.</span><span class="sxs-lookup"><span data-stu-id="36976-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="36976-320">Modificare il metodo **Details** di **StoreController**, aggiungendo un parametro **int** denominato **ID**. A tale scopo, sostituire il metodo **Details** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="36976-321">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="36976-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="36976-322">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="36976-323">In questa attività si proverà l'applicazione in un Web browser e si userà il parametro **ID** .</span><span class="sxs-lookup"><span data-stu-id="36976-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="36976-324">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-325">Il progetto viene avviato nella **Home** page.</span><span class="sxs-lookup"><span data-stu-id="36976-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="36976-326">Modificare l'URL in */Store/Details/5* per verificare che l'azione riceva il parametro ID.</span><span class="sxs-lookup"><span data-stu-id="36976-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="36976-327">![Esplorazione di StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Esplorazione di StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="36976-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="36976-328">*Esplorazione di/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="36976-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="36976-329">Esercizio 4: creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="36976-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="36976-330">Finora sono state restituite stringhe dalle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="36976-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="36976-331">Sebbene questo sia un modo utile per comprendere il funzionamento dei controller, non è il modo in cui vengono compilate le applicazioni Web reali.</span><span class="sxs-lookup"><span data-stu-id="36976-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="36976-332">Le visualizzazioni sono componenti che forniscono un approccio migliore per la generazione di codice HTML nel browser con l'uso di file modello.</span><span class="sxs-lookup"><span data-stu-id="36976-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="36976-333">In questo esercizio verrà illustrato come aggiungere una pagina master di layout per configurare un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e infine un modello di visualizzazione per consentire a HomeController di restituire HTML.</span><span class="sxs-lookup"><span data-stu-id="36976-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="36976-334">Attività 1-modifica del file \_layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="36976-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="36976-335">Il file **~/Views/Shared/\_layout. cshtml** consente di configurare un modello per il codice HTML comune da usare nell'intero sito Web.</span><span class="sxs-lookup"><span data-stu-id="36976-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="36976-336">In questa attività verrà aggiunta una pagina master di layout con un'intestazione comune con collegamenti alla Home page e all'area di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="36976-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="36976-337">Se non è già aperto, avviare **vs Express per il Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="36976-338">Nel menu **file** scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="36976-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="36976-339">Nella finestra di dialogo Apri progetto passare a **Source\Ex04-CreatingAView\Begin**, selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="36976-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="36976-340">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="36976-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="36976-341">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="36976-342">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36976-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="36976-343">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="36976-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="36976-344">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="36976-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-345">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="36976-346">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="36976-347">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="36976-348">Il file <strong>\_layout. cshtml</strong> contiene il layout del contenitore HTML per tutte le pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="36976-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="36976-349">Include l'elemento <strong>&lt;html&gt;</strong> per la risposta HTML, nonché gli elementi <strong>&lt;head&gt;</strong> e <strong>&lt;Body&gt;</strong> .</span><span class="sxs-lookup"><span data-stu-id="36976-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="36976-350"><strong>@RenderBody()</strong> all'interno del corpo HTML identificare le aree in cui i modelli di visualizzazione saranno in grado di compilare con contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="36976-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="36976-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="36976-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="36976-352">Aggiungere un'intestazione comune con i collegamenti alla Home page e all'area di archiviazione in tutte le pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="36976-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="36976-353">A tale scopo, aggiungere il seguente codice sotto &lt;istruzione Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="36976-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="36976-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="36976-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="36976-355">Includere un elemento div per eseguire il rendering della sezione del corpo di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="36976-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="36976-356">Sostituire <strong>@RenderBody()</strong> con il codice evidenziato seguente:C#()</span><span class="sxs-lookup"><span data-stu-id="36976-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="36976-357">Sai?</span><span class="sxs-lookup"><span data-stu-id="36976-357">Did you know?</span></span> <span data-ttu-id="36976-358">Visual Studio 2012 contiene frammenti di codice che semplificano l'aggiunta di codice comunemente usato in HTML, file di codice e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="36976-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="36976-359">Per provarlo, digitare **&lt;div&gt;** e premere **Tab** due volte per inserire un tag **div** completo.</span><span class="sxs-lookup"><span data-stu-id="36976-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="36976-360">Attività 2: aggiunta del foglio di stile CSS</span><span class="sxs-lookup"><span data-stu-id="36976-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="36976-361">Il modello di progetto vuoto include un file CSS molto semplificato che include solo gli stili utilizzati per visualizzare i form di base e i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="36976-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="36976-362">Per migliorare l'aspetto del sito, si utilizzeranno CSS e immagini aggiuntive (potenzialmente fornite da una finestra di progettazione).</span><span class="sxs-lookup"><span data-stu-id="36976-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="36976-363">In questa attività verrà aggiunto un foglio di stile CSS per definire gli stili del sito.</span><span class="sxs-lookup"><span data-stu-id="36976-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="36976-364">Il file CSS e le immagini sono inclusi nella cartella **Source\Assets\Content** di questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="36976-365">Per aggiungerli all'applicazione, trascinare il contenuto da una finestra di **Esplora risorse** nella **Esplora soluzioni** Visual Studio Express per il Web, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="36976-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="36976-366">![Trascinamento del contenuto dello stile](aspnet-mvc-4-fundamentals/_static/image12.png "Trascinamento del contenuto dello stile")</span><span class="sxs-lookup"><span data-stu-id="36976-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="36976-367">*Trascinamento del contenuto dello stile*</span><span class="sxs-lookup"><span data-stu-id="36976-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="36976-368">Verrà visualizzata una finestra di dialogo di avviso in cui viene chiesto di confermare la sostituzione del file **site. CSS** e di alcune immagini esistenti.</span><span class="sxs-lookup"><span data-stu-id="36976-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="36976-369">Selezionare **applica a tutti gli elementi** e fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="36976-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="36976-370">Attività 3-aggiunta di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="36976-371">In questa attività verrà aggiunto un modello di vista per generare la risposta HTML che utilizzerà la pagina master di layout e i CSS aggiunti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="36976-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="36976-372">Per usare un modello di vista quando si Esplora la home page del sito, è necessario innanzitutto indicare che anziché restituire una stringa, il metodo di **Indice HomeController** restituirà una **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="36976-373">Aprire la classe **HomeController** e modificare il metodo **index** per restituire un **ActionResult**e fare in modo che restituisca **View ()** .</span><span class="sxs-lookup"><span data-stu-id="36976-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="36976-374">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Indice HomeController EX4*)</span><span class="sxs-lookup"><span data-stu-id="36976-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="36976-375">A questo punto, è necessario aggiungere un modello di visualizzazione appropriato.</span><span class="sxs-lookup"><span data-stu-id="36976-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="36976-376">A tale scopo, **fare clic con il pulsante destro del mouse** all'interno del metodo di azione **index** e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="36976-377">Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .</span><span class="sxs-lookup"><span data-stu-id="36976-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="36976-378">![Aggiunta di una vista dall'interno del metodo index](aspnet-mvc-4-fundamentals/_static/image13.png "Aggiunta di una vista dall'interno del metodo index")</span><span class="sxs-lookup"><span data-stu-id="36976-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="36976-379">*Aggiunta di una vista dall'interno del metodo index*</span><span class="sxs-lookup"><span data-stu-id="36976-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="36976-380">Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** per generare un file di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="36976-381">Per impostazione predefinita, in questa finestra di dialogo viene pre-popolato il nome del modello di visualizzazione in modo che corrisponda al metodo di azione che lo utilizzerà.</span><span class="sxs-lookup"><span data-stu-id="36976-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="36976-382">Poiché è stato utilizzato il menu di scelta rapida **Aggiungi visualizzazione** all'interno del metodo di azione **Indice** all'interno di HomeController, nella finestra di dialogo **Aggiungi visualizzazione** è presente l'indice come nome di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="36976-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="36976-383">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-383">Click **Add**.</span></span>

    <span data-ttu-id="36976-384">![Finestra di dialogo Aggiungi visualizzazione](aspnet-mvc-4-fundamentals/_static/image14.png "Finestra di dialogo Aggiungi visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="36976-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="36976-385">*Finestra di dialogo Aggiungi visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="36976-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="36976-386">Visual Studio genera un modello di vista **index. cshtml** all'interno della cartella **Views\Home** e quindi lo apre.</span><span class="sxs-lookup"><span data-stu-id="36976-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="36976-387">![Visualizzazione Indice Home creata](aspnet-mvc-4-fundamentals/_static/image15.png "Visualizzazione Indice Home creata")</span><span class="sxs-lookup"><span data-stu-id="36976-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="36976-388">*Visualizzazione Indice Home creata*</span><span class="sxs-lookup"><span data-stu-id="36976-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-389">il nome e il percorso del file **index. cshtml** sono rilevanti e seguono le convenzioni di denominazione ASP.NET MVC predefinite.</span><span class="sxs-lookup"><span data-stu-id="36976-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="36976-390">La cartella \Views\**Home*\* corrisponde al nome del controller (**Home** controller).</span><span class="sxs-lookup"><span data-stu-id="36976-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="36976-391">Il nome del modello di visualizzazione (**Indice**) corrisponde al metodo di azione del controller che visualizzerà la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="36976-392">In questo modo, ASP.NET MVC evita di dover specificare in modo esplicito il nome o il percorso di un modello di vista quando si usa questa convenzione di denominazione per restituire una vista.</span><span class="sxs-lookup"><span data-stu-id="36976-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="36976-393">Il modello di visualizzazione generato è basato sul modello **\_layout. cshtml** definito in precedenza.</span><span class="sxs-lookup"><span data-stu-id="36976-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="36976-394">Aggiornare la proprietà ViewBag. title a **Home**e modificare il contenuto principale in **Questa pagina della Home page**, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="36976-395">Selezionare il progetto **MvcMusicStore** nel Esplora soluzioni e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="36976-396">Attività 4: verifica</span><span class="sxs-lookup"><span data-stu-id="36976-396">Task 4: Verification</span></span>

<span data-ttu-id="36976-397">Per verificare di aver eseguito correttamente tutti i passaggi dell'esercizio precedente, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="36976-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="36976-398">Con l'applicazione aperta in un browser, è necessario tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="36976-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="36976-399">Il metodo di azione index di HomeController ha rilevato e visualizzato il modello di vista **\Views\Home\Index.cshtml** , anche se il codice ha chiamato **Return View ()** , perché il modello di vista ha seguito la convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="36976-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="36976-400">Nella Home page viene visualizzato il messaggio di benvenuto definito nel modello di visualizzazione **\Views\Home\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="36976-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="36976-401">La Home Page usa il modello **\_layout. cshtml** , quindi il messaggio di benvenuto è contenuto nel layout HTML del sito standard.</span><span class="sxs-lookup"><span data-stu-id="36976-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="36976-402">![Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti](aspnet-mvc-4-fundamentals/_static/image16.png "Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti")</span><span class="sxs-lookup"><span data-stu-id="36976-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="36976-403">*Visualizzazione dell'indice Home con lo stile e il LayoutPage definiti*</span><span class="sxs-lookup"><span data-stu-id="36976-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="36976-404">Esercizio 5: creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="36976-405">Fino ad ora, le visualizzazioni visualizzano codice HTML hardcoded, ma per creare applicazioni Web dinamiche il modello di visualizzazione deve ricevere informazioni dal controller.</span><span class="sxs-lookup"><span data-stu-id="36976-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="36976-406">Una tecnica comune da usare a tale scopo è il modello **ViewModel** , che consente a un controller di creare un pacchetto di tutte le informazioni necessarie per generare la risposta HTML appropriata.</span><span class="sxs-lookup"><span data-stu-id="36976-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="36976-407">In questo esercizio verrà illustrato come creare una classe ViewModel e aggiungere le proprietà obbligatorie, ovvero il numero di generi nell'archivio e un elenco di tali generi.</span><span class="sxs-lookup"><span data-stu-id="36976-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="36976-408">Si aggiornerà anche il StoreController per usare il ViewModel creato e infine si creerà un nuovo modello di vista che visualizzerà le proprietà indicate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="36976-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="36976-409">Attività 1: creazione di una classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="36976-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="36976-410">In questa attività verrà creata una classe ViewModel che implementerà lo scenario di elenco dei generi di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="36976-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="36976-411">Se non è già aperto, avviare **vs Express per il Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="36976-412">Nel menu **file** scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="36976-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="36976-413">Nella finestra di dialogo Apri progetto passare a **Source\Ex05-CreatingAViewModel\Begin**, selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="36976-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="36976-414">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="36976-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="36976-415">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="36976-416">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36976-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="36976-417">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="36976-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="36976-418">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="36976-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-419">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="36976-420">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="36976-421">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="36976-422">Creare una cartella **ViewModels** per conservare il ViewModel.</span><span class="sxs-lookup"><span data-stu-id="36976-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="36976-423">A tale scopo, fare clic con il pulsante destro del mouse sul progetto **MvcMusicStore** di primo livello, scegliere **Aggiungi** e quindi **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="36976-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="36976-424">![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "Aggiunta di una nuova cartella")</span><span class="sxs-lookup"><span data-stu-id="36976-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="36976-425">*Aggiunta di una nuova cartella*</span><span class="sxs-lookup"><span data-stu-id="36976-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="36976-426">Denominare la cartella *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="36976-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="36976-427">![Cartella ViewModels in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "Cartella ViewModels in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="36976-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="36976-428">*Cartella ViewModels in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="36976-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="36976-429">Creare una classe **ViewModel** .</span><span class="sxs-lookup"><span data-stu-id="36976-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="36976-430">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **ViewModels** creata di recente, selezionare **Aggiungi** , quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="36976-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="36976-431">In **codice**scegliere l'elemento **classe** e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="36976-432">![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "Aggiunta di una nuova classe")</span><span class="sxs-lookup"><span data-stu-id="36976-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="36976-433">*Aggiunta di una nuova classe*</span><span class="sxs-lookup"><span data-stu-id="36976-433">*Adding a new Class*</span></span>

    <span data-ttu-id="36976-434">![Creazione della classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Creazione della classe StoreIndexViewModel")</span><span class="sxs-lookup"><span data-stu-id="36976-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="36976-435">*Creazione della classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="36976-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="36976-436">Attività 2: aggiunta di proprietà alla classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="36976-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="36976-437">È possibile passare due parametri da StoreController al modello di visualizzazione per generare la risposta HTML prevista, ovvero il numero di generi nell'archivio e un elenco di tali generi.</span><span class="sxs-lookup"><span data-stu-id="36976-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="36976-438">In questa attività verranno aggiunte le due proprietà seguenti alla classe **StoreIndexViewModel** : **NumberOfGenres** (Integer) e **genres** (un elenco di stringhe).</span><span class="sxs-lookup"><span data-stu-id="36976-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="36976-439">Aggiungere le proprietà **NumberOfGenres** e **genres** alla classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="36976-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="36976-440">A tale scopo, aggiungere le due righe seguenti alla definizione della classe:</span><span class="sxs-lookup"><span data-stu-id="36976-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="36976-441">(Frammento di codice- *ASP.NET MVC 4 Fundamentals-proprietà StoreIndexViewModel eX5*)</span><span class="sxs-lookup"><span data-stu-id="36976-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="36976-442">La notazione **{get; set;}** usa la funzionalità C#delle proprietà implementate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="36976-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="36976-443">Fornisce i vantaggi di una proprietà senza richiedere la dichiarazione di un campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="36976-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="36976-444">Attività 3-aggiornamento di StoreController per l'uso di StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="36976-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="36976-445">La classe **StoreIndexViewModel** incapsula le informazioni necessarie per passare dal metodo di **Indice** di **StoreController**a un modello di visualizzazione per generare una risposta.</span><span class="sxs-lookup"><span data-stu-id="36976-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="36976-446">In questa attività verrà aggiornato il **StoreController** per l'uso di **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="36976-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="36976-447">Aprire la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="36976-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="36976-448">![Apertura della classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Apertura della classe StoreController")</span><span class="sxs-lookup"><span data-stu-id="36976-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="36976-449">*Apertura della classe StoreController*</span><span class="sxs-lookup"><span data-stu-id="36976-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="36976-450">Per usare la classe **StoreIndexViewModel** da **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del codice **StoreController** :</span><span class="sxs-lookup"><span data-stu-id="36976-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="36976-451">(Frammento di codice: *ASP.NET MVC 4 Nozioni fondamentali-eX5 StoreIndexViewModel using ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="36976-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="36976-452">Modificare il metodo di azione dell' **Indice** di **StoreController**in modo da creare e popolare un oggetto **StoreIndexViewModel** e quindi passarlo a un modello di visualizzazione per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="36976-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-453">Nei modelli Lab &quot;ASP.NET MVC e l'accesso ai dati&quot; si scriverà il codice che recupera l'elenco dei generi di archiviazione da un database.</span><span class="sxs-lookup"><span data-stu-id="36976-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="36976-454">Nel codice seguente si creerà un **elenco** di generi di dati fittizi che popolano il **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="36976-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="36976-455">Dopo aver creato e configurato l'oggetto **StoreIndexViewModel** , questo verrà passato come argomento al metodo **View** .</span><span class="sxs-lookup"><span data-stu-id="36976-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="36976-456">Ciò indica che il modello di visualizzazione utilizzerà tale oggetto per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="36976-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="36976-457">Sostituire il metodo **index** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="36976-458">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-metodo di indice StoreController eX5*)</span><span class="sxs-lookup"><span data-stu-id="36976-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="36976-459">Se non si ha familiarità C#con, è possibile presupporre che l'uso di **var** significhi che la variabile **ViewModel** è ad associazione tardiva.</span><span class="sxs-lookup"><span data-stu-id="36976-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="36976-460">Questo non è corretto. il C# compilatore usa l'inferenza dei tipi in base a quanto assegnato alla variabile per determinare che **ViewModel** è di tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="36976-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="36976-461">Inoltre, compilando la variabile **ViewModel** locale come tipo **StoreIndexViewModel** , si ottiene il controllo in fase di compilazione e il supporto dell'editor di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="36976-462">Attività 4: creazione di un modello di visualizzazione che usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="36976-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="36976-463">In questa attività verrà creato un modello di visualizzazione che utilizzerà un oggetto StoreIndexViewModel passato dal controller per visualizzare un elenco di generi.</span><span class="sxs-lookup"><span data-stu-id="36976-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="36976-464">Prima di creare il nuovo modello di vista, compilare il progetto in modo che la **finestra di dialogo Aggiungi visualizzazione** conosca la classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="36976-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="36976-465">Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="36976-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="36976-466">![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilazione del progetto")</span><span class="sxs-lookup"><span data-stu-id="36976-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="36976-467">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="36976-467">*Building the project*</span></span>
2. <span data-ttu-id="36976-468">Creare un nuovo modello di vista.</span><span class="sxs-lookup"><span data-stu-id="36976-468">Create a new View template.</span></span> <span data-ttu-id="36976-469">A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo **index** e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="36976-470">![Aggiunta di una visualizzazione](aspnet-mvc-4-fundamentals/_static/image23.png "Aggiunta di una visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="36976-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="36976-471">*Aggiunta di una visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="36976-471">*Adding a View*</span></span>
3. <span data-ttu-id="36976-472">Poiché la **finestra di dialogo Aggiungi visualizzazione** è stata richiamata da **StoreController**, il modello di visualizzazione verrà aggiunto per impostazione predefinita in un file **\Views\Store\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="36976-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="36976-473">Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e quindi selezionare **StoreIndexViewModel** come **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="36976-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="36976-474">Assicurarsi inoltre che il motore di visualizzazione selezionato sia **Razor**.</span><span class="sxs-lookup"><span data-stu-id="36976-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="36976-475">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-475">Click **Add**.</span></span>

    <span data-ttu-id="36976-476">![Finestra di dialogo Aggiungi visualizzazione](aspnet-mvc-4-fundamentals/_static/image24.png "Finestra di dialogo Aggiungi visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="36976-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="36976-477">*Finestra di dialogo Aggiungi visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="36976-477">*Add View Dialog*</span></span>

    <span data-ttu-id="36976-478">Il file del modello di vista **\Views\Store\Index.cshtml** viene creato e aperto.</span><span class="sxs-lookup"><span data-stu-id="36976-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="36976-479">In base alle informazioni fornite nella finestra di dialogo **Aggiungi visualizzazione** nell'ultimo passaggio, il modello di vista prevede un'istanza di **StoreIndexViewModel** come dati da usare per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="36976-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="36976-480">Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span><span class="sxs-lookup"><span data-stu-id="36976-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="36976-481">Attività 5: aggiornamento del modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="36976-482">In questa attività verrà aggiornato il modello di visualizzazione creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.</span><span class="sxs-lookup"><span data-stu-id="36976-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="36976-483">Per eseguire il codice all'interno del modello di vista, si userà la sintassi @ (spesso denominata &quot;pepite di codice&quot;).</span><span class="sxs-lookup"><span data-stu-id="36976-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="36976-484">Nel file **index. cshtml** , all'interno della cartella **Store** , sostituire il codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="36976-485">Eseguire il loop sull'elenco di generi in **StoreIndexViewModel** e creare un elenco di **&gt;HTML&lt;UL** usando un ciclo **foreach** .</span><span class="sxs-lookup"><span data-stu-id="36976-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="36976-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="36976-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="36976-487">Premere **F5** per eseguire l'applicazione e sfogliare **/Store**.</span><span class="sxs-lookup"><span data-stu-id="36976-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="36976-488">Viene visualizzato l'elenco dei generi passati nell'oggetto **StoreIndexViewModel** da **StoreController** al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="36976-489">![Visualizzazione di un elenco di generi](aspnet-mvc-4-fundamentals/_static/image26.png "Visualizzazione di un elenco di generi")</span><span class="sxs-lookup"><span data-stu-id="36976-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="36976-490">*Visualizzazione di un elenco di generi*</span><span class="sxs-lookup"><span data-stu-id="36976-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="36976-491">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="36976-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="36976-492">Esercizio 6: utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="36976-493">Nell'esercizio 3 si è appreso come passare i parametri al controller.</span><span class="sxs-lookup"><span data-stu-id="36976-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="36976-494">In questo esercizio verrà illustrato come usare tali parametri nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="36976-495">A tale scopo, verranno introdotti prima le classi del modello che consentiranno di gestire i dati e la logica di dominio.</span><span class="sxs-lookup"><span data-stu-id="36976-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="36976-496">Inoltre, si apprenderà come creare collegamenti alle pagine all'interno dell'applicazione ASP.NET MVC senza preoccuparsi di elementi come la codifica dei percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="36976-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="36976-497">Attività 1-aggiunta di classi di modelli</span><span class="sxs-lookup"><span data-stu-id="36976-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="36976-498">A differenza dei ViewModel, che vengono creati solo per passare informazioni dal controller alla visualizzazione, le classi di modello vengono compilate per contenere e gestire i dati e la logica di dominio.</span><span class="sxs-lookup"><span data-stu-id="36976-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="36976-499">In questa attività verranno aggiunte due classi modello per rappresentare i concetti seguenti: **genere** e **album**.</span><span class="sxs-lookup"><span data-stu-id="36976-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="36976-500">Se non è già aperto, avviare **vs Express per il Web**</span><span class="sxs-lookup"><span data-stu-id="36976-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="36976-501">Nel menu **file** scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="36976-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="36976-502">Nella finestra di dialogo Apri progetto passare a **Source\Ex06-UsingParametersInView\Begin**, selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="36976-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="36976-503">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="36976-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="36976-504">Se è stata aperta la soluzione **iniziale** fornita, sarà necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="36976-505">A tale scopo, fare clic sul menu **progetto** e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36976-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="36976-506">Nella finestra di dialogo **Gestisci pacchetti NuGet** fare clic su **Ripristina** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="36976-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="36976-507">Infine, compilare la soluzione facendo clic su **compila** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="36976-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="36976-508">Uno dei vantaggi dell'uso di NuGet è che non è necessario distribuire tutte le librerie nel progetto, riducendo le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="36976-509">Con NuGet Power Tools, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="36976-510">Questo è il motivo per cui sarà necessario eseguire questi passaggi dopo aver aperto una soluzione esistente da questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="36976-511">Aggiungere una classe del modello di **genere** .</span><span class="sxs-lookup"><span data-stu-id="36976-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="36976-512">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Models** nella **Esplora soluzioni**, scegliere **Aggiungi** e quindi l'opzione **nuovo elemento** .</span><span class="sxs-lookup"><span data-stu-id="36976-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="36976-513">In **codice**scegliere l'elemento **classe** e denominare il file *genre.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="36976-514">![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "Aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="36976-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="36976-515">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="36976-515">*Adding a new item*</span></span>

    <span data-ttu-id="36976-516">![Aggiungi classe modello di genere](aspnet-mvc-4-fundamentals/_static/image28.png "Aggiungi classe modello di genere")</span><span class="sxs-lookup"><span data-stu-id="36976-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="36976-517">*Aggiungi classe modello di genere*</span><span class="sxs-lookup"><span data-stu-id="36976-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="36976-518">Aggiungere una proprietà **Name** alla classe Genre.</span><span class="sxs-lookup"><span data-stu-id="36976-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="36976-519">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-519">To do this, add the following code:</span></span>

    <span data-ttu-id="36976-520">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 genre*)</span><span class="sxs-lookup"><span data-stu-id="36976-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="36976-521">Seguendo la stessa procedura descritta in precedenza, aggiungere una classe **album** .</span><span class="sxs-lookup"><span data-stu-id="36976-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="36976-522">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Models** nella **Esplora soluzioni**, scegliere **Aggiungi** e quindi l'opzione **nuovo elemento** .</span><span class="sxs-lookup"><span data-stu-id="36976-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="36976-523">In **codice**scegliere l'elemento **classe** e denominare il file *album.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="36976-524">Aggiungere due proprietà alla classe album: **genre** e **title**.</span><span class="sxs-lookup"><span data-stu-id="36976-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="36976-525">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-525">To do this, add the following code:</span></span>

    <span data-ttu-id="36976-526">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="36976-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="36976-527">Attività 2-aggiunta di un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="36976-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="36976-528">In questa attività verrà usato un **StoreBrowseViewModel** per visualizzare gli album che corrispondono a un genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="36976-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="36976-529">In questa attività si creerà questa classe e quindi si aggiungono due proprietà per gestire il **genere** e l'elenco degli **album**.</span><span class="sxs-lookup"><span data-stu-id="36976-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="36976-530">Aggiungere una classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="36976-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="36976-531">A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **ViewModels** nella **Esplora soluzioni**, selezionare **Aggiungi** e quindi l'opzione **nuovo elemento** .</span><span class="sxs-lookup"><span data-stu-id="36976-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="36976-532">In **codice**scegliere l'elemento **classe** e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="36976-533">Aggiungere un riferimento ai modelli nella classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="36976-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="36976-534">A tale scopo, aggiungere il codice seguente usando lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="36976-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="36976-535">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="36976-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="36976-536">Aggiungere due proprietà alla classe **StoreBrowseViewModel** : **genre** e **albums**.</span><span class="sxs-lookup"><span data-stu-id="36976-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="36976-537">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-537">To do this, add the following code:</span></span>

    <span data-ttu-id="36976-538">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="36976-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="36976-539">Che cos' **è list&lt;Album&gt;** ?: questa definizione usa il **tipo list&lt;t&gt;** , dove **t** vincola il tipo a cui appartengono gli elementi di questo **elenco** , in questo caso **album** (o uno qualsiasi dei relativi discendenti).</span><span class="sxs-lookup"><span data-stu-id="36976-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="36976-540">Questa possibilità di progettare classi e metodi che rinviano la specifica di uno o più tipi fino a quando la classe o il metodo non viene dichiarato e ne viene creata un'istanza C# dal codice client è una funzionalità del linguaggio denominato **generics**.</span><span class="sxs-lookup"><span data-stu-id="36976-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="36976-541">**List&lt;t&gt;** è l'equivalente generico del tipo **ArrayList** ed è disponibile nello spazio dei nomi **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="36976-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="36976-542">Uno dei vantaggi dell'uso dei **generics** è che, poiché il tipo è specificato, non è necessario prestare attenzione alle operazioni di controllo dei tipi, ad esempio il cast degli elementi nell' **album** come si farebbe con un **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="36976-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="36976-543">Attività 3-uso del nuovo ViewModel in StoreController</span><span class="sxs-lookup"><span data-stu-id="36976-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="36976-544">In questa attività verranno modificati i metodi di azione **Browse** e **Details** di **StoreController**per usare il nuovo **StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="36976-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="36976-545">Aggiungere un riferimento alla cartella Models nella classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="36976-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="36976-546">A tale scopo, espandere la cartella **Controllers** nella **Esplora soluzioni** e aprire la classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="36976-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="36976-547">Aggiungere quindi il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-547">Then add the following code:</span></span>

    <span data-ttu-id="36976-548">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="36976-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="36976-549">Sostituire il metodo di azione **Browse** per usare la classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="36976-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="36976-550">Si creeranno un genere e due nuovi oggetti album con dati fittizi, nel laboratorio pratico successivo si utilizzeranno dati reali da un database.</span><span class="sxs-lookup"><span data-stu-id="36976-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="36976-551">A tale scopo, sostituire il metodo **Browse** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="36976-552">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="36976-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="36976-553">Sostituire il metodo di azione **Dettagli** per usare la classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="36976-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="36976-554">Verrà creato un nuovo oggetto **album** da restituire alla **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="36976-555">A tale scopo, sostituire il metodo **Details** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="36976-556">(Frammento di codice- *ASP.NET MVC 4 Nozioni fondamentali-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="36976-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="36976-557">Attività 4: aggiunta di un modello di visualizzazione Sfoglia</span><span class="sxs-lookup"><span data-stu-id="36976-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="36976-558">In questa attività verrà aggiunta una visualizzazione **Sfoglia** per visualizzare gli album trovati per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="36976-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="36976-559">Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che la finestra di dialogo **Aggiungi visualizzazione** conosca la classe **ViewModel** da usare.</span><span class="sxs-lookup"><span data-stu-id="36976-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="36976-560">Compilare il progetto selezionando la voce di menu **Compila** e quindi **Compila MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="36976-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="36976-561">Aggiungere una visualizzazione **Sfoglia** .</span><span class="sxs-lookup"><span data-stu-id="36976-561">Add a **Browse** View.</span></span> <span data-ttu-id="36976-562">A tale scopo, fare clic con il pulsante destro del mouse sul metodo di azione **Browse** del **StoreController** e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="36976-563">Nella finestra di dialogo **Aggiungi visualizzazione** verificare che il nome della vista sia **Browse**.</span><span class="sxs-lookup"><span data-stu-id="36976-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="36976-564">Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **StoreBrowseViewModel** nell'elenco a discesa **classe modello** .</span><span class="sxs-lookup"><span data-stu-id="36976-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="36976-565">Lasciare gli altri campi con il relativo valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="36976-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="36976-566">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-566">Then click **Add**.</span></span>

    <span data-ttu-id="36976-567">![Aggiunta di una visualizzazione Sfoglia](aspnet-mvc-4-fundamentals/_static/image29.png "Aggiunta di una visualizzazione Sfoglia")</span><span class="sxs-lookup"><span data-stu-id="36976-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="36976-568">*Aggiunta di una visualizzazione Sfoglia*</span><span class="sxs-lookup"><span data-stu-id="36976-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="36976-569">Modificare il **cshtml browse** per visualizzare le informazioni del genere, accedendo all'oggetto **StoreBrowseViewModel** passato al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="36976-570">A tale scopo, sostituire il contenuto con il codice seguente:C#()</span><span class="sxs-lookup"><span data-stu-id="36976-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="36976-571">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="36976-572">In questa attività si verificherà che il metodo **Browse** recuperi gli album dall'azione del metodo **Browse** .</span><span class="sxs-lookup"><span data-stu-id="36976-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="36976-573">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-574">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="36976-574">The project starts in the Home page.</span></span> <span data-ttu-id="36976-575">Modificare l'URL in **/Store/browse? Genre = discoteca** per verificare che l'azione restituisca due album.</span><span class="sxs-lookup"><span data-stu-id="36976-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="36976-576">![Esplorazione degli album del disco del negozio](aspnet-mvc-4-fundamentals/_static/image30.png "Esplorazione degli album del disco del negozio")</span><span class="sxs-lookup"><span data-stu-id="36976-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="36976-577">*Esplorazione degli album del disco del negozio*</span><span class="sxs-lookup"><span data-stu-id="36976-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="36976-578">Attività 6: visualizzazione di informazioni su un album specifico</span><span class="sxs-lookup"><span data-stu-id="36976-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="36976-579">In questa attività verrà implementata la visualizzazione **Store/Details** per visualizzare informazioni su un album specifico.</span><span class="sxs-lookup"><span data-stu-id="36976-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="36976-580">In questo laboratorio pratico, tutto ciò che verrà visualizzato sull'album è già contenuto nel modello di **visualizzazione** .</span><span class="sxs-lookup"><span data-stu-id="36976-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="36976-581">Quindi, invece di creare una classe **StoreDetailsViewModel** , si userà il modello **StoreBrowseViewModel** corrente che passa l'album.</span><span class="sxs-lookup"><span data-stu-id="36976-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="36976-582">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="36976-583">Aggiungere una nuova visualizzazione **Dettagli** per il metodo di azione **Dettagli** **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="36976-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="36976-584">A tale scopo, fare clic con il pulsante destro del mouse sul metodo **Details** nella classe **StoreController** e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="36976-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="36976-585">Nella finestra di dialogo **Aggiungi visualizzazione** , verificare che il **nome della vista** sia **Details**.</span><span class="sxs-lookup"><span data-stu-id="36976-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="36976-586">Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** e selezionare **album** dall'elenco a discesa **classe modello** .</span><span class="sxs-lookup"><span data-stu-id="36976-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="36976-587">Lasciare gli altri campi con il relativo valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="36976-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="36976-588">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="36976-588">Then click **Add**.</span></span> <span data-ttu-id="36976-589">Verrà creato e aperto un file **\Views\Store\Details.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="36976-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="36976-590">![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "Aggiunta di una visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="36976-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="36976-591">*Aggiunta di una visualizzazione dettagli*</span><span class="sxs-lookup"><span data-stu-id="36976-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="36976-592">Modificare il file **Details. cshtml** per visualizzare le informazioni sull'album, accedendo all'oggetto **album** passato al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="36976-593">A tale scopo, sostituire il contenuto con il codice seguente:C#()</span><span class="sxs-lookup"><span data-stu-id="36976-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="36976-594">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="36976-595">In questa attività si verificherà che la visualizzazione **Dettagli** recupera le informazioni sull'album dal metodo di **azione dettagli** .</span><span class="sxs-lookup"><span data-stu-id="36976-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="36976-596">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-597">Il progetto viene avviato nella **Home** page.</span><span class="sxs-lookup"><span data-stu-id="36976-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="36976-598">Modificare l'URL in **/Store/Details/5** per verificare le informazioni sull'album.</span><span class="sxs-lookup"><span data-stu-id="36976-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="36976-599">![Dettagli degli album di esplorazione](aspnet-mvc-4-fundamentals/_static/image32.png "Dettagli degli album di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="36976-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="36976-600">*Esplorazione dei dettagli degli album*</span><span class="sxs-lookup"><span data-stu-id="36976-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="36976-601">Attività 8: aggiunta di collegamenti tra pagine</span><span class="sxs-lookup"><span data-stu-id="36976-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="36976-602">In questa attività si aggiungerà un collegamento nella visualizzazione Store per avere un collegamento in ogni nome del genere all'URL **/Store/browse** appropriato.</span><span class="sxs-lookup"><span data-stu-id="36976-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="36976-603">In questo modo, quando si fa clic su un genere, ad esempio **discoteca**, si passa a **/Store/browse? genre = disco** URL.</span><span class="sxs-lookup"><span data-stu-id="36976-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="36976-604">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="36976-605">Aggiornare la pagina di **Indice** per aggiungere un collegamento alla pagina **Browse** .</span><span class="sxs-lookup"><span data-stu-id="36976-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="36976-606">A tale scopo, nella **Esplora soluzioni** espandere la cartella **views** , quindi la cartella **Store** e fare doppio clic sulla pagina **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="36976-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="36976-607">Aggiungere un collegamento alla visualizzazione Sfoglia che indica il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="36976-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="36976-608">A tale scopo, sostituire il codice evidenziato seguente nei tag **&lt;li&gt;** : (C#)</span><span class="sxs-lookup"><span data-stu-id="36976-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="36976-609">un altro approccio consiste nel collegarsi direttamente alla pagina, con un codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="36976-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="36976-610">&lt;a href =&quot;/Store/Browse? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="36976-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="36976-611">Sebbene questo approccio funzioni, dipende da una stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="36976-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="36976-612">Se successivamente si rinomina il controller, sarà necessario modificare manualmente questa istruzione.</span><span class="sxs-lookup"><span data-stu-id="36976-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="36976-613">Un'alternativa migliore consiste nell'usare un metodo **helper HTML** .</span><span class="sxs-lookup"><span data-stu-id="36976-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="36976-614">ASP.NET MVC include un metodo helper HTML disponibile per tali attività.</span><span class="sxs-lookup"><span data-stu-id="36976-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="36976-615">Il metodo helper **HTML. ActionLink ()** semplifica la compilazione di HTML **&lt;un&gt;** collegamenti, assicurando che i percorsi URL siano codificati correttamente con URL.</span><span class="sxs-lookup"><span data-stu-id="36976-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="36976-616">HTML. ActionLink include diversi overload.</span><span class="sxs-lookup"><span data-stu-id="36976-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="36976-617">In questo esercizio si userà uno che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="36976-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="36976-618">Testo del collegamento, che visualizzerà il nome del genere</span><span class="sxs-lookup"><span data-stu-id="36976-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="36976-619">Nome azione controller (**Sfoglia**)</span><span class="sxs-lookup"><span data-stu-id="36976-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="36976-620">Valori dei parametri di route, specificando il nome (**genere**) e il valore (**nome del genere**)</span><span class="sxs-lookup"><span data-stu-id="36976-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="36976-621">Attività 9: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="36976-622">In questa attività si verificherà che ogni genere venga visualizzato con un collegamento all'URL **/Store/browse** appropriato.</span><span class="sxs-lookup"><span data-stu-id="36976-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="36976-623">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-624">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="36976-624">The project starts in the Home page.</span></span> <span data-ttu-id="36976-625">Modificare l'URL in **/Store** per verificare che ogni genere sia collegato all'URL **/Store/browse** appropriato.</span><span class="sxs-lookup"><span data-stu-id="36976-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="36976-626">![Esplorazione dei generi con collegamenti alla pagina Sfoglia](aspnet-mvc-4-fundamentals/_static/image33.png "Esplorazione dei generi con collegamenti alla pagina Sfoglia")</span><span class="sxs-lookup"><span data-stu-id="36976-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="36976-627">*Esplorazione dei generi con collegamenti alla pagina Sfoglia*</span><span class="sxs-lookup"><span data-stu-id="36976-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="36976-628">Attività 10-uso della raccolta ViewModel dinamica per passare i valori</span><span class="sxs-lookup"><span data-stu-id="36976-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="36976-629">In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il controller e la visualizzazione senza apportare alcuna modifica al modello.</span><span class="sxs-lookup"><span data-stu-id="36976-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="36976-630">ASP.NET MVC 4 fornisce la raccolta &quot;ViewModel&quot;, che può essere assegnata a qualsiasi valore dinamico e accessibile anche all'interno di controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="36976-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="36976-631">A questo punto si userà la raccolta dinamica ViewBag per passare un elenco di **generi** &quot;&quot; dal controller alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="36976-632">La visualizzazione dell'indice dell'archivio consente di accedere a **ViewModel** e visualizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="36976-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="36976-633">Se necessario, chiudere il browser per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="36976-634">Aprire **StoreController.cs** e modificare il metodo **index** per creare un elenco di generi con protagonisti nella raccolta ViewModel:</span><span class="sxs-lookup"><span data-stu-id="36976-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="36976-635">Per accedere alle proprietà, è anche possibile usare la sintassi **ViewBag [&quot;&quot;di restarted]** .</span><span class="sxs-lookup"><span data-stu-id="36976-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="36976-636">L'icona a stella **&quot;&quot;. png** è inclusa nella cartella **Source\Assets\Images** di questo Lab.</span><span class="sxs-lookup"><span data-stu-id="36976-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="36976-637">Per aggiungerlo all'applicazione, trascinare il contenuto da una finestra di **Esplora risorse** nel **Esplora soluzioni** in Visual Web Developer Express, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="36976-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="36976-638">![Aggiunta dell'immagine a stella alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "Aggiunta dell'immagine a stella alla soluzione")</span><span class="sxs-lookup"><span data-stu-id="36976-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="36976-639">*Aggiunta dell'immagine a stella alla soluzione*</span><span class="sxs-lookup"><span data-stu-id="36976-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="36976-640">Aprire il file View **Store/index. cshtml** e modificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="36976-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="36976-641">Si leggerà il &quot;proprietà&quot; con le stelle nella raccolta **ViewBag** e si chiede se il nome del genere corrente è presente nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="36976-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="36976-642">In tal caso, viene visualizzata un'icona a stella a destra del collegamento di genere.</span><span class="sxs-lookup"><span data-stu-id="36976-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="36976-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="36976-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="36976-644">Attività 11: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="36976-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="36976-645">In questa attività si verificherà che i generi con protagonisti visualizzino un'icona a stella.</span><span class="sxs-lookup"><span data-stu-id="36976-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="36976-646">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="36976-647">Il progetto viene avviato nella **Home** page.</span><span class="sxs-lookup"><span data-stu-id="36976-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="36976-648">Modificare l'URL in **/Store** per verificare che ogni genere in primo piano abbia l'etichetta rispettabile:</span><span class="sxs-lookup"><span data-stu-id="36976-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="36976-649">![Esplorazione dei generi con elementi con stella](aspnet-mvc-4-fundamentals/_static/image35.png "Esplorazione dei generi con elementi con stella")</span><span class="sxs-lookup"><span data-stu-id="36976-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="36976-650">*Esplorazione dei generi con elementi con stella*</span><span class="sxs-lookup"><span data-stu-id="36976-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="36976-651">Esercizio 7: un giro intorno A ASP.NET MVC 4 nuovo modello</span><span class="sxs-lookup"><span data-stu-id="36976-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="36976-652">In questo esercizio si esploreranno i miglioramenti apportati ai modelli di progetto ASP.NET MVC 4, esaminando le funzionalità più rilevanti del nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="36976-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="36976-653">Attività 1: esplorazione del modello di applicazione Internet MVC 4 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="36976-654">Se non è già aperto, avviare **vs Express per il Web**</span><span class="sxs-lookup"><span data-stu-id="36976-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="36976-655">Selezionare il **file | Nuovo |** Comando di menu Project.</span><span class="sxs-lookup"><span data-stu-id="36976-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="36976-656">Nella finestra di dialogo **nuovo progetto** selezionare l' **oggetto C#visivo | Modello Web** nell'albero del riquadro sinistro e scegliere l' **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="36976-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="36976-657">**Denominare** il progetto *MusicStore* e aggiornare il **nome della soluzione** per *iniziare*, quindi selezionare un percorso (oppure lasciare l'impostazione predefinita) e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="36976-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="36976-658">![Creazione di un nuovo progetto MVC 4 ASP.NET](aspnet-mvc-4-fundamentals/_static/image36.png "Creazione di un nuovo progetto MVC 4 ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="36976-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="36976-659">*Creazione di un nuovo progetto MVC 4 ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="36976-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="36976-660">Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare il modello di progetto **applicazione Internet** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="36976-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="36976-661">Si noti che è possibile selezionare Razor o ASPX come motore di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="36976-662">![Creazione di una nuova applicazione ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "Creazione di una nuova applicazione ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="36976-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="36976-663">*Creazione di una nuova applicazione ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="36976-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-664">Sintassi Razor è stato introdotto in ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="36976-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="36976-665">L'obiettivo è ridurre al minimo il numero di caratteri e le sequenze di tasti richiesti in un file, abilitando un flusso di lavoro di codifica veloce e fluido.</span><span class="sxs-lookup"><span data-stu-id="36976-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="36976-666">Razor si avvale di C#competenze del linguaggio/VB (o other) esistenti e fornisce una sintassi di markup del modello che consente un flusso di lavoro di costruzione html impressionante.</span><span class="sxs-lookup"><span data-stu-id="36976-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="36976-667">Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato.</span><span class="sxs-lookup"><span data-stu-id="36976-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="36976-668">È possibile consultare le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="36976-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="36976-669">**Modelli di stile moderno**</span><span class="sxs-lookup"><span data-stu-id="36976-669">**Modern-style templates**</span></span>

        <span data-ttu-id="36976-670">I modelli sono stati rinnovati, offrendo stili più moderni.</span><span class="sxs-lookup"><span data-stu-id="36976-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="36976-671">![Modelli ASP.NET MVC 4 ridisegnati](aspnet-mvc-4-fundamentals/_static/image38.png "Modelli ASP.NET MVC 4 ridisegnati")</span><span class="sxs-lookup"><span data-stu-id="36976-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="36976-672">*Modelli ASP.NET MVC 4 ridisegnati*</span><span class="sxs-lookup"><span data-stu-id="36976-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="36976-673">**Rendering adattivo**</span><span class="sxs-lookup"><span data-stu-id="36976-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="36976-674">Estrarre il ridimensionamento della finestra del browser e notare il modo in cui il layout di pagina si adatta dinamicamente alle nuove dimensioni della finestra.</span><span class="sxs-lookup"><span data-stu-id="36976-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="36976-675">Questi modelli usano la tecnica di rendering adattivo per il rendering corretto nelle piattaforme desktop e per dispositivi mobili senza alcuna personalizzazione.</span><span class="sxs-lookup"><span data-stu-id="36976-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="36976-676">![Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse](aspnet-mvc-4-fundamentals/_static/image39.png "Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse")</span><span class="sxs-lookup"><span data-stu-id="36976-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="36976-677">*Modello di progetto ASP.NET MVC 4 in dimensioni del browser diverse*</span><span class="sxs-lookup"><span data-stu-id="36976-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="36976-678">Chiudere il browser per arrestare il debugger e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="36976-679">A questo punto è possibile esplorare la soluzione e vedere alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="36976-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="36976-680">![ASP.NET MVC4-Internet-Application-Project-template](aspnet-mvc-4-fundamentals/_static/image40.png "Modello di progetto di applicazione Internet MVC 4 ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="36976-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="36976-681">*Modello di progetto di applicazione Internet MVC 4 ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="36976-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="36976-682">**Markup HTML5**</span><span class="sxs-lookup"><span data-stu-id="36976-682">**HTML5 markup**</span></span>

       <span data-ttu-id="36976-683">Sfogliare le viste dei modelli per individuare il nuovo markup del tema, ad esempio aprire la visualizzazione **About. cshtml** all'interno della **Home** directory.</span><span class="sxs-lookup"><span data-stu-id="36976-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="36976-684">![Nuovo modello, usando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nuovo modello, usando il markup Razor e HTML5")</span><span class="sxs-lookup"><span data-stu-id="36976-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="36976-685">*Nuovo modello, usando il markup Razor e HTML5*</span><span class="sxs-lookup"><span data-stu-id="36976-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="36976-686">**Librerie JavaScript incluse**</span><span class="sxs-lookup"><span data-stu-id="36976-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="36976-687">**jQuery**: jQuery semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e le interazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="36976-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="36976-688">**interfaccia utente di jQuery**: questa libreria fornisce le astrazioni per l'interazione e l'animazione di basso livello, gli effetti avanzati e i widget a tema, basati sulla libreria JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="36976-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="36976-689">Per informazioni sull'interfaccia utente di jQuery e jQuery, vedere [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="36976-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="36976-690">**Knockout**: il modello predefinito ASP.NET MVC 4 include ora **Knockout**, un Framework di MVVM JavaScript che consente di creare applicazioni Web avanzate e altamente reattive con JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="36976-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="36976-691">Analogamente a ASP.NET MVC 3, le librerie dell'interfaccia utente jQuery e jQuery sono incluse anche in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="36976-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="36976-692">È possibile ottenere altre informazioni sulla libreria knockout in questo collegamento: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="36976-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="36976-693">**Modernizzatore**: questa libreria viene eseguita automaticamente, rendendo il sito compatibile con i browser meno recenti quando si usano le tecnologie HTML5 e CSS3.</span><span class="sxs-lookup"><span data-stu-id="36976-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="36976-694">È possibile ottenere altre informazioni sulla libreria Modernizzator in questo collegamento: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="36976-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="36976-695">**SimpleMembership inclusi nella soluzione**</span><span class="sxs-lookup"><span data-stu-id="36976-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="36976-696">SimpleMembership è stato progettato come sostituzione per il ruolo ASP.NET precedente e il sistema di provider di appartenenze.</span><span class="sxs-lookup"><span data-stu-id="36976-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="36976-697">Offre molte nuove funzionalità che semplificano lo sviluppo delle pagine Web in modo più flessibile.</span><span class="sxs-lookup"><span data-stu-id="36976-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="36976-698">Il modello Internet ha già configurato alcune operazioni per l'integrazione di SimpleMembership, ad esempio, AccountController è pronto per l'uso di Metodo OAuthWebSecurity (per la registrazione dell'account OAuth, l'accesso, la gestione e così via) e la sicurezza Web.</span><span class="sxs-lookup"><span data-stu-id="36976-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="36976-699">![SimpleMembership inclusi nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclusi nella soluzione")</span><span class="sxs-lookup"><span data-stu-id="36976-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="36976-700">*SimpleMembership inclusi nella soluzione*</span><span class="sxs-lookup"><span data-stu-id="36976-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="36976-701">Ulteriori informazioni su [Metodo OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) sono disponibili in MSDN.</span><span class="sxs-lookup"><span data-stu-id="36976-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="36976-702">Inoltre, è possibile distribuire l'applicazione in siti Web di Microsoft Azure dopo [l'Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="36976-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="36976-703">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="36976-703">Summary</span></span>

<span data-ttu-id="36976-704">Completando questa esercitazione pratica, sono state apprese le nozioni di base di ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="36976-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="36976-705">Gli elementi principali di un'applicazione MVC e il modo in cui interagiscono</span><span class="sxs-lookup"><span data-stu-id="36976-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="36976-706">Come creare un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="36976-707">Come aggiungere e configurare i controller per gestire i parametri passati attraverso l'URL e la QueryString</span><span class="sxs-lookup"><span data-stu-id="36976-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="36976-708">Come aggiungere una pagina master di layout per configurare un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di visualizzazione per visualizzare il contenuto HTML</span><span class="sxs-lookup"><span data-stu-id="36976-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="36976-709">Come usare il modello ViewModel per passare le proprietà al modello di visualizzazione per visualizzare le informazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="36976-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="36976-710">Come usare i parametri passati ai controller nel modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="36976-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="36976-711">Come aggiungere collegamenti alle pagine all'interno dell'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="36976-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="36976-712">Come aggiungere e utilizzare le proprietà dinamiche in una vista</span><span class="sxs-lookup"><span data-stu-id="36976-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="36976-713">Miglioramenti apportati ai modelli di progetto ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="36976-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="36976-714">Appendice A: installazione di Visual Studio Express 2012 per il Web</span><span class="sxs-lookup"><span data-stu-id="36976-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="36976-715">È possibile installare **Microsoft Visual Studio Express 2012 per il Web** o un'altra versione &quot;Express&quot; usando il **[installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="36976-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="36976-716">Le istruzioni seguenti illustrano i passaggi necessari per installare *Visual Studio Express 2012 per il Web* con *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="36976-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="36976-717">Passare a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="36976-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="36976-718">In alternativa, se è già stata installata l'installazione guidata piattaforma Web, è possibile aprirla e cercare il prodotto &quot;<em>Visual Studio Express 2012 per il Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="36976-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="36976-719">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="36976-719">Click on **Install Now**.</span></span> <span data-ttu-id="36976-720">Se non si dispone dell' **installazione guidata piattaforma Web** , si verrà reindirizzati per il download e l'installazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="36976-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="36976-721">Quando l'installazione **guidata piattaforma Web** è aperta, fare clic su **Installa** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="36976-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="36976-722">![Installa Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="36976-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="36976-723">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="36976-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="36976-724">Leggere tutti i termini e le licenze dei prodotti e fare clic su **Accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="36976-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettazione delle condizioni di licenza](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="36976-726">*Accettazione delle condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="36976-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="36976-727">Attendere il completamento del processo di download e installazione.</span><span class="sxs-lookup"><span data-stu-id="36976-727">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="36976-729">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="36976-729">*Installation progress*</span></span>
6. <span data-ttu-id="36976-730">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="36976-730">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="36976-732">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="36976-732">*Installation completed*</span></span>
7. <span data-ttu-id="36976-733">Fare clic su **Esci** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="36976-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="36976-734">Per aprire Visual Studio Express per il Web, passare alla schermata **Start** e iniziare a scrivere &quot;**visual Studio Express**&quot;, quindi fare clic sul riquadro **vs Express per il Web** .</span><span class="sxs-lookup"><span data-stu-id="36976-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Riquadro VS Express per il Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="36976-736">*Riquadro VS Express per il Web*</span><span class="sxs-lookup"><span data-stu-id="36976-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="36976-737">Appendice B: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="36976-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="36976-738">In questa appendice verrà illustrato come creare un nuovo sito Web dalla portale di gestione di Windows Azure e come pubblicare l'applicazione ottenuta seguendo il Lab, sfruttando la funzionalità di pubblicazione Distribuzione Web fornita da Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="36976-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="36976-739">Attività 1: creazione di un nuovo sito Web dal portale di Windows Azure</span><span class="sxs-lookup"><span data-stu-id="36976-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="36976-740">Accedere al [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere con le credenziali Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="36976-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-741">Con Windows Azure è possibile ospitare gratuitamente 10 siti Web ASP.NET e quindi ridimensionarli in base alla crescita del traffico.</span><span class="sxs-lookup"><span data-stu-id="36976-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="36976-742">È possibile iscriversi [qui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="36976-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="36976-743">![Accedere a Windows portale di Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Accedere a Windows portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="36976-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="36976-744">*Accedere a portale di gestione di Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="36976-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="36976-745">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="36976-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="36976-746">![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "Creazione di un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="36976-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="36976-747">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="36976-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="36976-748">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="36976-749">Quindi selezionare opzione **creazione rapida** .</span><span class="sxs-lookup"><span data-stu-id="36976-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="36976-750">Fornire un URL disponibile per il nuovo sito Web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="36976-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-751">Un sito Web di Microsoft Azure è l'host di un'applicazione Web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="36976-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="36976-752">L'opzione Creazione rapida consente di distribuire un'applicazione Web completata nel sito Web di Windows Azure dall'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="36976-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="36976-753">Non sono inclusi i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="36976-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="36976-754">![Creazione di un nuovo sito Web mediante creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "Creazione di un nuovo sito Web mediante creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="36976-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="36976-755">*Creazione di un nuovo sito Web mediante creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="36976-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="36976-756">Attendere la creazione del nuovo **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="36976-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="36976-757">Una volta creato il sito Web, fare clic sul collegamento nella colonna **URL** .</span><span class="sxs-lookup"><span data-stu-id="36976-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="36976-758">Verificare che il nuovo sito Web sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="36976-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="36976-759">![Esplorazione del nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image51.png "Esplorazione del nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="36976-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="36976-760">*Esplorazione del nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="36976-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="36976-761">![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "Sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="36976-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="36976-762">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="36976-762">*Web site running*</span></span>
6. <span data-ttu-id="36976-763">Tornare al portale e fare clic sul nome del sito Web nella colonna **nome** per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="36976-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="36976-764">![Apertura delle pagine di gestione del sito Web](aspnet-mvc-4-fundamentals/_static/image53.png "Apertura delle pagine di gestione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="36976-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="36976-765">*Apertura delle pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="36976-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="36976-766">Nella sezione **Riepilogo rapido** della pagina **Dashboard** fare clic sul collegamento **Scarica profilo di pubblicazione** .</span><span class="sxs-lookup"><span data-stu-id="36976-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-767">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione Web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="36976-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="36976-768">Nel profilo di pubblicazione sono contenuti gli URL, le credenziali utente e le stringhe di database necessari per la connessione e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="36976-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="36976-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per il Web** e **Microsoft Visual Studio 2012** supportano la lettura di profili di pubblicazione per automatizzare la configurazione di questi programmi per la pubblicazione di applicazioni Web in siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="36976-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="36976-770">![Download del profilo di pubblicazione del sito Web](aspnet-mvc-4-fundamentals/_static/image54.png "Download del profilo di pubblicazione del sito Web")</span><span class="sxs-lookup"><span data-stu-id="36976-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="36976-771">*Download del profilo di pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="36976-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="36976-772">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="36976-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="36976-773">Più avanti in questo esercizio verrà illustrato come utilizzare questo file per pubblicare un'applicazione Web in siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36976-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="36976-774">![Salvataggio del file del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "Salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="36976-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="36976-775">*Salvataggio del file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="36976-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="36976-776">Attività 2: configurazione del server di database</span><span class="sxs-lookup"><span data-stu-id="36976-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="36976-777">Se l'applicazione usa i database di SQL Server sarà necessario creare un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="36976-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="36976-778">Se si desidera distribuire una semplice applicazione che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="36976-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="36976-779">Per archiviare il database dell'applicazione, sarà necessario un server di database SQL.</span><span class="sxs-lookup"><span data-stu-id="36976-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="36976-780">È possibile visualizzare i server del database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure in **database sql** | **Server** | **Dashboard del server**.</span><span class="sxs-lookup"><span data-stu-id="36976-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="36976-781">Se non è stato creato un server, è possibile crearne uno usando il pulsante **Aggiungi** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="36976-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="36976-782">Annotare il **nome e l'URL del server, il nome e la password dell'account di accesso dell'amministratore**, che verranno usati nelle attività successive.</span><span class="sxs-lookup"><span data-stu-id="36976-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="36976-783">Non creare ancora il database, perché verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="36976-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="36976-784">![Dashboard del server di database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard del server di database SQL")</span><span class="sxs-lookup"><span data-stu-id="36976-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="36976-785">*Dashboard del server di database SQL*</span><span class="sxs-lookup"><span data-stu-id="36976-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="36976-786">Nell'attività successiva verrà testato la connessione al database da Visual Studio. per questo motivo, è necessario includere l'indirizzo IP locale nell'elenco di **indirizzi IP consentiti**del server.</span><span class="sxs-lookup"><span data-stu-id="36976-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="36976-787">A tale scopo, fare clic su **Configura**, selezionare l'indirizzo IP da **indirizzo IP client corrente** e incollarlo nelle caselle di testo indirizzo IP **iniziale** e **indirizzo IP finale** , quindi fare clic sul pulsante ![Aggiungi-client-IP-address-OK-pulsante](aspnet-mvc-4-fundamentals/_static/image57.png).</span><span class="sxs-lookup"><span data-stu-id="36976-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Aggiunta dell'indirizzo IP del client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="36976-789">*Aggiunta dell'indirizzo IP del client*</span><span class="sxs-lookup"><span data-stu-id="36976-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="36976-790">Una volta aggiunto l' **indirizzo IP del client** all'elenco indirizzi IP consentiti, fare clic su **Salva** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="36976-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Conferma modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="36976-792">*Conferma modifiche*</span><span class="sxs-lookup"><span data-stu-id="36976-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="36976-793">Attività 3: pubblicazione di un'applicazione ASP.NET MVC 4 con Distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="36976-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="36976-794">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="36976-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="36976-795">Nella **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto di sito Web e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="36976-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="36976-796">![Pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "Pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="36976-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="36976-797">*Pubblicazione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="36976-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="36976-798">Importare il profilo di pubblicazione salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="36976-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="36976-799">![Importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "Importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="36976-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="36976-800">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="36976-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="36976-801">Fare clic su **convalida connessione**.</span><span class="sxs-lookup"><span data-stu-id="36976-801">Click **Validate Connection**.</span></span> <span data-ttu-id="36976-802">Al termine della convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="36976-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36976-803">La convalida è completa quando viene visualizzato un segno di spunta verde accanto al pulsante Convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="36976-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="36976-804">![Convalida della connessione](aspnet-mvc-4-fundamentals/_static/image62.png "Convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="36976-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="36976-805">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="36976-805">*Validating connection*</span></span>
4. <span data-ttu-id="36976-806">Nella pagina **Impostazioni** , nella sezione **database** , fare clic sul pulsante accanto alla casella di testo della connessione del database, ad esempio **DefaultConnection**.</span><span class="sxs-lookup"><span data-stu-id="36976-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="36976-807">![Configurazione distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "Configurazione distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="36976-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="36976-808">*Configurazione distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="36976-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="36976-809">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="36976-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="36976-810">In **nome server** Digitare l'URL del server di database SQL utilizzando il prefisso *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="36976-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="36976-811">In **nome utente** Digitare il nome di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="36976-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="36976-812">In **password** Digitare la password di accesso dell'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="36976-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="36976-813">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="36976-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="36976-814">![Configurazione della stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "Configurazione della stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="36976-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="36976-815">*Configurazione della stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="36976-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="36976-816">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="36976-816">Then click **OK**.</span></span> <span data-ttu-id="36976-817">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="36976-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="36976-818">![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "Creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="36976-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="36976-819">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="36976-819">*Creating the database*</span></span>
7. <span data-ttu-id="36976-820">La stringa di connessione che verrà utilizzata per connettersi al database SQL in Windows Azure viene visualizzata nella casella di testo default Connection (connessione predefinita).</span><span class="sxs-lookup"><span data-stu-id="36976-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="36976-821">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="36976-821">Then click **Next**.</span></span>

    <span data-ttu-id="36976-822">![Stringa di connessione che punta al database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "Stringa di connessione che punta al database SQL")</span><span class="sxs-lookup"><span data-stu-id="36976-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="36976-823">*Stringa di connessione che punta al database SQL*</span><span class="sxs-lookup"><span data-stu-id="36976-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="36976-824">Nella pagina **Anteprima** fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="36976-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="36976-825">![Pubblicazione dell'applicazione Web](aspnet-mvc-4-fundamentals/_static/image67.png "Pubblicazione dell'applicazione Web")</span><span class="sxs-lookup"><span data-stu-id="36976-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="36976-826">*Pubblicazione dell'applicazione Web*</span><span class="sxs-lookup"><span data-stu-id="36976-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="36976-827">Al termine del processo di pubblicazione, nel browser predefinito viene aperto il sito Web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="36976-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="36976-828">![Applicazione pubblicata in Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="36976-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="36976-829">*Applicazione pubblicata in Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="36976-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="36976-830">Appendice C: utilizzo di frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="36976-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="36976-831">Con i frammenti di codice, tutto il codice necessario è a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="36976-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="36976-832">Il documento Lab indica esattamente quando è possibile usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="36976-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="36976-833">![Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="36976-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="36976-834">*Uso dei frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="36976-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="36976-835">***Per aggiungere un frammento di codice usando laC# tastiera (solo)***</span><span class="sxs-lookup"><span data-stu-id="36976-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="36976-836">Posizionare il cursore nel punto in cui si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="36976-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="36976-837">Iniziare a digitare il nome del frammento (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="36976-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="36976-838">Osservare come IntelliSense Visualizza i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="36976-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="36976-839">Selezionare il frammento di codice corretto (oppure continua a digitare fino a quando non viene selezionato il nome dell'intero frammento).</span><span class="sxs-lookup"><span data-stu-id="36976-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="36976-840">Premere il tasto TAB due volte per inserire il frammento nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="36976-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="36976-841">![Inizia a digitare il nome del frammento](aspnet-mvc-4-fundamentals/_static/image70.png "Inizia a digitare il nome del frammento")</span><span class="sxs-lookup"><span data-stu-id="36976-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="36976-842">*Inizia a digitare il nome del frammento*</span><span class="sxs-lookup"><span data-stu-id="36976-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="36976-843">![Premere TAB per selezionare il frammento evidenziato](aspnet-mvc-4-fundamentals/_static/image71.png "Premere TAB per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="36976-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="36976-844">*Premere TAB per selezionare il frammento evidenziato*</span><span class="sxs-lookup"><span data-stu-id="36976-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="36976-845">![Premere nuovamente TAB per espandere il frammento di codice](aspnet-mvc-4-fundamentals/_static/image72.png "Premere nuovamente TAB per espandere il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="36976-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="36976-846">*Premere nuovamente TAB per espandere il frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="36976-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="36976-847">***Per aggiungere un frammento di codice utilizzando ilC#mouse (, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="36976-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="36976-848">Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="36976-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="36976-849">Selezionare **Inserisci frammento** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="36976-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="36976-850">Selezionare il frammento pertinente nell'elenco facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="36976-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="36976-851">![Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento](aspnet-mvc-4-fundamentals/_static/image73.png "Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento")</span><span class="sxs-lookup"><span data-stu-id="36976-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="36976-852">*Fare clic con il pulsante destro del mouse su dove si vuole inserire il frammento di codice e selezionare Inserisci frammento*</span><span class="sxs-lookup"><span data-stu-id="36976-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="36976-853">![Selezionare il frammento pertinente dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "Selezionare il frammento pertinente dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="36976-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="36976-854">*Selezionare il frammento pertinente dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="36976-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
