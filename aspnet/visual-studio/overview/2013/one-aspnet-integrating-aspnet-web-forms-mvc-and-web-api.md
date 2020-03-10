---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio pratico: una ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web | Microsoft Docs'
author: rick-anderson
description: ASP.NET è un Framework per la creazione di siti Web, app e servizi usando tecnologie specializzate, ad esempio MVC, API Web e altri. Con il ASP.NET di espansione h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623200"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="8956c-104">Laboratorio pratico: una ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web</span><span class="sxs-lookup"><span data-stu-id="8956c-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>

<span data-ttu-id="8956c-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8956c-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8956c-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="8956c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="8956c-107">ASP.NET è un Framework per la creazione di siti Web, app e servizi usando tecnologie specializzate, ad esempio MVC, API Web e altri.</span><span class="sxs-lookup"><span data-stu-id="8956c-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="8956c-108">Con l'espansione ASP.NET ha visto che la sua creazione ed è stata espressa la necessità di integrare queste tecnologie, sono stati compiuti sforzi recenti per lavorare verso **un ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="8956c-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="8956c-109">Visual Studio 2013 introduce un nuovo sistema di progetto unificato che consente di compilare un'applicazione e di usare tutte le tecnologie ASP.NET in un unico progetto.</span><span class="sxs-lookup"><span data-stu-id="8956c-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="8956c-110">Questa funzionalità Elimina la necessità di scegliere una tecnologia all'inizio di un progetto e di usarla e incoraggia invece l'uso di più framework ASP.NET all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="8956c-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="8956c-111">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="8956c-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="8956c-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8956c-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8956c-113">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="8956c-113">Objectives</span></span>

<span data-ttu-id="8956c-114">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="8956c-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="8956c-115">Creare un sito Web basato su un tipo di progetto **ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="8956c-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="8956c-116">Usare Framework **ASP.NET** diversi, ad esempio **MVC** e **API Web** nello stesso progetto</span><span class="sxs-lookup"><span data-stu-id="8956c-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="8956c-117">Identificare i componenti principali di un'applicazione **ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="8956c-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="8956c-118">Sfrutta il Framework di **impalcatura ASP.NET** per creare automaticamente controller e visualizzazioni per eseguire operazioni CRUD basate sulle classi del modello</span><span class="sxs-lookup"><span data-stu-id="8956c-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="8956c-119">Esporre lo stesso set di informazioni in formati leggibili dal computer e leggibili usando lo strumento appropriato per ogni processo</span><span class="sxs-lookup"><span data-stu-id="8956c-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8956c-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8956c-120">Prerequisites</span></span>

<span data-ttu-id="8956c-121">Per completare questa esercitazione pratica, è necessario quanto segue:</span><span class="sxs-lookup"><span data-stu-id="8956c-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="8956c-122">[Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="8956c-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="8956c-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="8956c-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8956c-124">Configurazione</span><span class="sxs-lookup"><span data-stu-id="8956c-124">Setup</span></span>

<span data-ttu-id="8956c-125">Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="8956c-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="8956c-126">Aprire Esplora risorse e passare alla cartella di **origine** del Lab.</span><span class="sxs-lookup"><span data-stu-id="8956c-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="8956c-127">Fare clic con il pulsante destro del mouse su **Setup. cmd** e scegliere **Esegui come amministratore** per avviare il processo di installazione che configurerà l'ambiente e installerà i frammenti di codice di Visual Studio per questo Lab.</span><span class="sxs-lookup"><span data-stu-id="8956c-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="8956c-128">Se viene visualizzata la finestra di dialogo controllo account utente, confermare l'azione per continuare.</span><span class="sxs-lookup"><span data-stu-id="8956c-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="8956c-129">Prima di eseguire l'installazione, verificare di aver selezionato tutte le dipendenze per questo Lab.</span><span class="sxs-lookup"><span data-stu-id="8956c-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="8956c-130">Uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="8956c-130">Using the Code Snippets</span></span>

<span data-ttu-id="8956c-131">Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="8956c-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="8956c-132">Per praticità, la maggior parte di questo codice viene fornita come Visual Studio Code frammenti, a cui è possibile accedere da Visual Studio 2013 per evitare di aggiungerla manualmente.</span><span class="sxs-lookup"><span data-stu-id="8956c-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="8956c-133">Ogni esercizio è accompagnato da una soluzione iniziale che si trova nella cartella **Begin** dell'esercizio che consente di seguire ogni esercizio in modo indipendente dagli altri.</span><span class="sxs-lookup"><span data-stu-id="8956c-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="8956c-134">Tenere presente che i frammenti di codice aggiunti durante un esercizio risultano mancanti da queste soluzioni iniziali e potrebbero non funzionare fino a quando non è stato completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="8956c-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="8956c-135">All'interno del codice sorgente per un esercizio, si troverà anche una cartella **finale** contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="8956c-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="8956c-136">È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.</span><span class="sxs-lookup"><span data-stu-id="8956c-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8956c-137">Esercizi</span><span class="sxs-lookup"><span data-stu-id="8956c-137">Exercises</span></span>

<span data-ttu-id="8956c-138">Questa esercitazione pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8956c-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="8956c-139">Creazione di un nuovo progetto Web Form</span><span class="sxs-lookup"><span data-stu-id="8956c-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="8956c-140">Creazione di un controller MVC tramite impalcature</span><span class="sxs-lookup"><span data-stu-id="8956c-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="8956c-141">Creazione di un controller API Web mediante l'impalcatura</span><span class="sxs-lookup"><span data-stu-id="8956c-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="8956c-142">Tempo stimato per il completamento del Lab: **60 minuti**</span><span class="sxs-lookup"><span data-stu-id="8956c-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="8956c-143">Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="8956c-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="8956c-144">Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="8956c-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="8956c-145">Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** .</span><span class="sxs-lookup"><span data-stu-id="8956c-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="8956c-146">Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="8956c-147">Esercizio 1: creazione di un nuovo progetto Web Form</span><span class="sxs-lookup"><span data-stu-id="8956c-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="8956c-148">In questo esercizio verrà creato un nuovo sito Web Form in Visual Studio 2013 usando l'esperienza di progetto unificata **ASP.NET** , che consente di integrare facilmente i componenti Web Form, MVC e API Web nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="8956c-149">Si esaminerà quindi la soluzione generata e ne identificherà le parti e infine il sito Web verrà visualizzato in azione.</span><span class="sxs-lookup"><span data-stu-id="8956c-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="8956c-150">Attività 1: creazione di un nuovo sito con un'esperienza ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8956c-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="8956c-151">In questa attività si avvierà la creazione di un nuovo sito Web in Visual Studio in base a un tipo di progetto **ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="8956c-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="8956c-152">**Un ASP.NET** unifica tutte le tecnologie ASP.NET e offre la possibilità di combinare e abbinarle in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="8956c-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="8956c-153">Si riconosceranno quindi i diversi componenti di Web Form, MVC e API Web che si trovano all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="8956c-154">Aprire **Visual Studio Express 2013 per il Web** e selezionare **file | Nuovo progetto...** per avviare una nuova soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Creazione di un nuovo progetto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="8956c-156">*Creazione di un nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="8956c-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="8956c-157">Nella finestra di dialogo **nuovo progetto** selezionare **applicazione Web ASP.NET** sotto l'oggetto **visivo C# | Scheda Web** e verificare che sia selezionato **.NET Framework 4,5** .</span><span class="sxs-lookup"><span data-stu-id="8956c-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="8956c-158">Denominare il progetto *MyHybridSite*, scegliere un **percorso** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8956c-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nuovo progetto di applicazione Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="8956c-160">*Creazione di un nuovo progetto di applicazione Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="8956c-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="8956c-161">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **Web Form** e selezionare le opzioni **MVC** e **API Web** .</span><span class="sxs-lookup"><span data-stu-id="8956c-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="8956c-162">Inoltre, assicurarsi che l'opzione **autenticazione** sia impostata su **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="8956c-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="8956c-163">Fare clic su **OK** per continuare.</span><span class="sxs-lookup"><span data-stu-id="8956c-163">Click **OK** to continue.</span></span>

    ![Creazione di un nuovo progetto con il modello Web Form, inclusi i componenti dell'API Web e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="8956c-165">*Creazione di un nuovo progetto con il modello Web Form, inclusi i componenti dell'API Web e MVC*</span><span class="sxs-lookup"><span data-stu-id="8956c-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="8956c-166">È ora possibile esplorare la struttura della soluzione generata.</span><span class="sxs-lookup"><span data-stu-id="8956c-166">You can now explore the structure of the generated solution.</span></span>

    ![Esplorazione della soluzione generata](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="8956c-168">*Esplorazione della soluzione generata*</span><span class="sxs-lookup"><span data-stu-id="8956c-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="8956c-169">**Account:** Questa cartella contiene le pagine Web Form da registrare, accedere e gestire gli account utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="8956c-170">Questa cartella viene aggiunta quando si seleziona l'opzione di autenticazione **account utente singolo** durante la configurazione del modello di progetto Web Form.</span><span class="sxs-lookup"><span data-stu-id="8956c-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="8956c-171">**Modelli:** Questa cartella conterrà le classi che rappresentano i dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="8956c-172">**Controller** e **visualizzazioni**: queste cartelle sono necessarie per i componenti **ASP.NET MVC** e **API Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="8956c-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="8956c-173">Negli esercizi successivi si esamineranno le tecnologie MVC e API Web.</span><span class="sxs-lookup"><span data-stu-id="8956c-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="8956c-174">I file **default. aspx**, **Contact. aspx** e **About. aspx** sono pagine Web Form predefinite che è possibile utilizzare come punti di partenza per compilare le pagine specifiche per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="8956c-175">La logica di programmazione di tali file si trova in un file separato denominato file di&quot; code-behind &quot;, che include un'estensione &quot;. aspx. vb&quot; o &quot;. aspx.cs&quot; (a seconda della lingua utilizzata).</span><span class="sxs-lookup"><span data-stu-id="8956c-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="8956c-176">La logica code-behind viene eseguita sul server e produce dinamicamente l'output HTML per la pagina.</span><span class="sxs-lookup"><span data-stu-id="8956c-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="8956c-177">Le pagine **site. master** e **site. mobile. master** definiscono l'aspetto e il comportamento standard di tutte le pagine dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="8956c-178">Fare doppio clic sul file **default. aspx** per esplorare il contenuto della pagina.</span><span class="sxs-lookup"><span data-stu-id="8956c-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Esplorazione della pagina default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="8956c-180">*Esplorazione della pagina default. aspx*</span><span class="sxs-lookup"><span data-stu-id="8956c-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-181">La direttiva della **pagina** nella parte superiore del file definisce gli attributi della pagina Web Form.</span><span class="sxs-lookup"><span data-stu-id="8956c-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="8956c-182">Ad esempio, l'attributo **MasterPageFile** specifica il percorso della pagina master. in questo caso, la pagina *site. master* e l'attributo **Inherits** definiscono la classe code-behind per la pagina da ereditare.</span><span class="sxs-lookup"><span data-stu-id="8956c-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="8956c-183">Questa classe si trova nel file determinato dall'attributo **CodeBehind** .</span><span class="sxs-lookup"><span data-stu-id="8956c-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="8956c-184">Il controllo **ASP: content** include il contenuto effettivo della pagina (testo, markup e controlli) e viene mappato a un controllo **ASP: ContentPlaceHolder** nella pagina master.</span><span class="sxs-lookup"><span data-stu-id="8956c-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="8956c-185">In questo caso, verrà eseguito il rendering del contenuto della pagina all'interno del controllo *MainContent* definito nella pagina *site. master* .</span><span class="sxs-lookup"><span data-stu-id="8956c-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="8956c-186">Espandere l' **App\_** cartella di avvio e notare il file **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="8956c-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="8956c-187">Visual Studio include il file nella soluzione generata perché è stata inclusa l'API Web quando si configura il progetto con un modello ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8956c-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="8956c-188">Aprire il file **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="8956c-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="8956c-189">Nella classe *WebApiConfig* si troverà la configurazione associata all'API Web, che esegue il mapping delle route HTTP ai **controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="8956c-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="8956c-190">Aprire il file **RouteConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="8956c-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="8956c-191">All'interno del metodo *RegisterRoutes* si troverà la configurazione associata a MVC, che esegue il mapping delle route HTTP ai **controller MVC**.</span><span class="sxs-lookup"><span data-stu-id="8956c-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="8956c-192">Attività 2: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="8956c-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="8956c-193">In questa attività verrà eseguita la soluzione generata, verranno esplorate l'app e alcune delle relative funzionalità, ad esempio la riscrittura degli URL e l'autenticazione incorporata.</span><span class="sxs-lookup"><span data-stu-id="8956c-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="8956c-194">Per eseguire la soluzione, premere **F5** o fare clic sul pulsante **Start** posizionato sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="8956c-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="8956c-195">Il home page applicazione deve essere aperto nel browser.</span><span class="sxs-lookup"><span data-stu-id="8956c-195">The application home page should open in the browser.</span></span>

    ![Esecuzione della soluzione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="8956c-197">Verificare che vengano richiamate le pagine Web Form.</span><span class="sxs-lookup"><span data-stu-id="8956c-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="8956c-198">A tale scopo, aggiungere **/Contact.aspx** all'URL nella barra degli indirizzi e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="8956c-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![URL leggibili](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="8956c-200">*URL brevi*</span><span class="sxs-lookup"><span data-stu-id="8956c-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-201">Come si può notare, l'URL viene modificato in **/contatti**.</span><span class="sxs-lookup"><span data-stu-id="8956c-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="8956c-202">A partire da **ASP.NET 4**, le funzionalità di routing degli URL sono state aggiunte ai Web Form, quindi è possibile scrivere url come *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* anziché *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* .</span><span class="sxs-lookup"><span data-stu-id="8956c-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="8956c-203">Per altre informazioni, vedere [routing degli URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="8956c-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="8956c-204">Verrà ora esaminato il flusso di autenticazione integrato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="8956c-205">A tale scopo, fare clic su **registra** nell'angolo superiore destro della pagina.</span><span class="sxs-lookup"><span data-stu-id="8956c-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrazione di un nuovo utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="8956c-207">*Registrazione di un nuovo utente*</span><span class="sxs-lookup"><span data-stu-id="8956c-207">*Registering a new user*</span></span>
4. <span data-ttu-id="8956c-208">Nella pagina **Register** immettere un **nome utente** e una **password**e quindi fare clic su **Register (registra**).</span><span class="sxs-lookup"><span data-stu-id="8956c-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Pagina di registrazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="8956c-210">*Pagina di registrazione*</span><span class="sxs-lookup"><span data-stu-id="8956c-210">*Register page*</span></span>
5. <span data-ttu-id="8956c-211">L'applicazione registra il nuovo account e l'utente viene autenticato.</span><span class="sxs-lookup"><span data-stu-id="8956c-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Utente autenticato](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="8956c-213">*Utente autenticato*</span><span class="sxs-lookup"><span data-stu-id="8956c-213">*User authenticated*</span></span>
6. <span data-ttu-id="8956c-214">Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="8956c-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="8956c-215">Esercizio 2: creazione di un controller MVC mediante l'impalcatura</span><span class="sxs-lookup"><span data-stu-id="8956c-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="8956c-216">In questo esercizio si utilizzerà il Framework di impalcatura ASP.NET fornito da Visual Studio per creare un controller ASP.NET MVC 5 con azioni e visualizzazioni Razor per eseguire operazioni CRUD, senza scrivere una sola riga di codice.</span><span class="sxs-lookup"><span data-stu-id="8956c-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="8956c-217">Il processo di impalcatura utilizzerà Entity Framework Code First per generare il contesto dei dati e lo schema del database nel database SQL.</span><span class="sxs-lookup"><span data-stu-id="8956c-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="8956c-218">**Informazioni su Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="8956c-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="8956c-219">Entity Framework (EF) è un mapper relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite la programmazione con un modello di applicazione concettuale anziché programmare direttamente utilizzando uno schema di archiviazione relazionale.</span><span class="sxs-lookup"><span data-stu-id="8956c-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="8956c-220">Il flusso di lavoro di Entity Framework Code First modellazione consente di usare le proprie classi di dominio per rappresentare il modello su cui è basato EF durante l'esecuzione di query, rilevamento delle modifiche e funzioni di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="8956c-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="8956c-221">Utilizzando il flusso di lavoro di sviluppo Code First, non è necessario avviare l'applicazione tramite la creazione di un database o la specifica di uno schema.</span><span class="sxs-lookup"><span data-stu-id="8956c-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="8956c-222">È invece possibile scrivere classi .NET standard che definiscono gli oggetti del modello di dominio più appropriati per l'applicazione e Entity Framework creerà il database per l'utente.</span><span class="sxs-lookup"><span data-stu-id="8956c-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="8956c-223">Per altre informazioni su Entity Framework, vedere [qui](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="8956c-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="8956c-224">Attività 1: creazione di un nuovo modello</span><span class="sxs-lookup"><span data-stu-id="8956c-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="8956c-225">Verrà ora definita una classe **Person** , che sarà il modello usato dal processo di ponteggi per creare il controller MVC e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="8956c-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="8956c-226">Si inizierà creando una classe del modello **Person** e le operazioni CRUD nel controller verranno create automaticamente usando le funzionalità di ponteggi.</span><span class="sxs-lookup"><span data-stu-id="8956c-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="8956c-227">Aprire **Visual Studio Express 2013 per il Web** e la soluzione **MyHybridSite. sln** che si trova nella cartella **source/EX2-MvcScaffolding/Begin** .</span><span class="sxs-lookup"><span data-stu-id="8956c-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="8956c-228">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="8956c-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="8956c-229">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **Models** del progetto **MyHybridSite** e scegliere **Aggiungi | Classe...**</span><span class="sxs-lookup"><span data-stu-id="8956c-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Aggiunta della classe del modello person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="8956c-231">*Aggiunta della classe del modello person*</span><span class="sxs-lookup"><span data-stu-id="8956c-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="8956c-232">Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare un nome al file *Person.cs* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8956c-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Creazione della classe del modello person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="8956c-234">*Creazione della classe del modello person*</span><span class="sxs-lookup"><span data-stu-id="8956c-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="8956c-235">Sostituire il contenuto del file **Person.cs** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="8956c-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="8956c-236">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="8956c-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="8956c-237">(Frammento di codice- *BringingTogetherOneAspNet-EX2-PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="8956c-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="8956c-238">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MyHybridSite** e selezionare **Compila**oppure premere **CTRL + MAIUSC + B** per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="8956c-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="8956c-239">Attività 2: creazione di un controller MVC</span><span class="sxs-lookup"><span data-stu-id="8956c-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="8956c-240">Ora che il modello **Person** è stato creato, si userà l'impalcatura MVC ASP.NET con Entity Framework per creare le azioni e le visualizzazioni del controller CRUD per **Person**.</span><span class="sxs-lookup"><span data-stu-id="8956c-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="8956c-241">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **controller** del progetto **MyHybridSite** e scegliere **Aggiungi | Nuovo elemento con impalcatura...**</span><span class="sxs-lookup"><span data-stu-id="8956c-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Creazione di un nuovo controller con ponteggi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="8956c-243">*Creazione di un nuovo controller con ponteggi*</span><span class="sxs-lookup"><span data-stu-id="8956c-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="8956c-244">Nella finestra di dialogo **Aggiungi impalcatura** selezionare **controller MVC 5 con visualizzazioni, usando Entity Framework** e quindi fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="8956c-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Selezione del controller MVC 5 con visualizzazioni e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="8956c-246">*Selezione del controller MVC 5 con visualizzazioni e Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="8956c-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="8956c-247">Impostare *MvcPersonController* come **nome del controller**, selezionare l'opzione **use Async controller actions** e selezionare **Person (MyHybridSite. Models)** come **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="8956c-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Aggiunta di un controller MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="8956c-249">*Aggiunta di un controller MVC con impalcature*</span><span class="sxs-lookup"><span data-stu-id="8956c-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="8956c-250">In **classe contesto dati**fare clic su **nuovo contesto dati...** .</span><span class="sxs-lookup"><span data-stu-id="8956c-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Creazione di un nuovo contesto dati](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="8956c-252">*Creazione di un nuovo contesto dati*</span><span class="sxs-lookup"><span data-stu-id="8956c-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="8956c-253">Nella finestra di dialogo **nuovo contesto dati** assegnare al nuovo contesto dati il nome *PersonContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8956c-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Creazione del nuovo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="8956c-255">*Creazione del nuovo tipo PersonContext*</span><span class="sxs-lookup"><span data-stu-id="8956c-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="8956c-256">Fare clic su **Aggiungi** per creare il nuovo controller per la **persona** con ponteggi.</span><span class="sxs-lookup"><span data-stu-id="8956c-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="8956c-257">In Visual Studio vengono quindi generate le azioni del controller, il contesto dei dati Person e le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="8956c-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Dopo aver creato il controller MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="8956c-259">*Dopo aver creato il controller MVC con impalcature*</span><span class="sxs-lookup"><span data-stu-id="8956c-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="8956c-260">Aprire il file **MvcPersonController.cs** nella cartella **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="8956c-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="8956c-261">Si noti che i metodi di azione CRUD sono stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="8956c-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="8956c-262">Selezionando la casella di controllo **USA azioni del controller asincrono** dalle opzioni di impalcatura nei passaggi precedenti, Visual Studio genera metodi di azione asincroni per tutte le azioni che coinvolgono l'accesso al contesto dati Person.</span><span class="sxs-lookup"><span data-stu-id="8956c-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="8956c-263">Si consiglia di utilizzare i metodi di azione asincroni per le richieste a esecuzione prolungata, non associate alla CPU, per evitare che il server Web esegua il lavoro mentre è in corso l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="8956c-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="8956c-264">Attività 3: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="8956c-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="8956c-265">In questa attività verrà eseguita nuovamente la soluzione per verificare che le visualizzazioni per la **persona** funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="8956c-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="8956c-266">Verrà aggiunta una nuova persona per verificare che sia stata salvata correttamente nel database.</span><span class="sxs-lookup"><span data-stu-id="8956c-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="8956c-267">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="8956c-268">Passare a **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="8956c-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="8956c-269">Verrà visualizzata la vista con impalcatura che mostra l'elenco di persone.</span><span class="sxs-lookup"><span data-stu-id="8956c-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="8956c-270">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="8956c-270">Click **Create New** to add a new person.</span></span>

    ![Esplorazione delle visualizzazioni MVC con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="8956c-272">*Esplorazione delle visualizzazioni MVC con impalcature*</span><span class="sxs-lookup"><span data-stu-id="8956c-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="8956c-273">Nella visualizzazione **Crea** specificare un **nome** e un' **età** per la persona e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="8956c-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Aggiunta di una nuova persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="8956c-275">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="8956c-275">*Adding a new person*</span></span>
5. <span data-ttu-id="8956c-276">La nuova persona viene aggiunta all'elenco.</span><span class="sxs-lookup"><span data-stu-id="8956c-276">The new person is added to the list.</span></span> <span data-ttu-id="8956c-277">Nell'elenco elemento fare clic su **Dettagli** per visualizzare la visualizzazione dettagli della persona.</span><span class="sxs-lookup"><span data-stu-id="8956c-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="8956c-278">Quindi, nella visualizzazione **Dettagli** fare clic su **Torna a elenco** per tornare alla visualizzazione elenco.</span><span class="sxs-lookup"><span data-stu-id="8956c-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Visualizzazione dettagli persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="8956c-280">*Visualizzazione dettagli persona*</span><span class="sxs-lookup"><span data-stu-id="8956c-280">*Person's details view*</span></span>
6. <span data-ttu-id="8956c-281">Fare clic sul collegamento **Elimina** per eliminare la persona.</span><span class="sxs-lookup"><span data-stu-id="8956c-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="8956c-282">Nella visualizzazione **Elimina** fare clic su **Elimina** per confermare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Eliminazione di una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="8956c-284">*Eliminazione di una persona*</span><span class="sxs-lookup"><span data-stu-id="8956c-284">*Deleting a person*</span></span>
7. <span data-ttu-id="8956c-285">Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="8956c-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="8956c-286">Esercizio 3: creazione di un controller API Web mediante l'impalcatura</span><span class="sxs-lookup"><span data-stu-id="8956c-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="8956c-287">Il Framework API Web fa parte dello stack ASP.NET ed è progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e la ricezione di dati in formato JSON o XML tramite un'API RESTful.</span><span class="sxs-lookup"><span data-stu-id="8956c-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="8956c-288">In questo esercizio si userà di nuovo l'impalcatura ASP.NET per generare un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="8956c-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="8956c-289">Si utilizzeranno le stesse classi **Person** e **PersonContext** dell'esercizio precedente per fornire gli stessi dati di persona in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="8956c-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="8956c-290">Si vedrà come è possibile esporre le stesse risorse in modi diversi all'interno della stessa applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8956c-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="8956c-291">Attività 1: creazione di un controller API Web</span><span class="sxs-lookup"><span data-stu-id="8956c-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="8956c-292">In questa attività verrà creato un nuovo **controller API Web** che esporrà i dati dell'utente in un formato utilizzabile da computer come JSON.</span><span class="sxs-lookup"><span data-stu-id="8956c-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="8956c-293">Se non è già aperto, aprire **Visual Studio Express 2013 per il Web** e aprire la soluzione **MyHybridSite. sln** che si trova nella cartella **source/EX3-WebAPI/Begin** .</span><span class="sxs-lookup"><span data-stu-id="8956c-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="8956c-294">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="8956c-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-295">Se si inizia con la soluzione Begin dall'esercizio 3, premere **CTRL + MAIUSC + B** per compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="8956c-296">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **controller** del progetto **MyHybridSite** e scegliere **Aggiungi | Nuovo elemento con impalcatura...**</span><span class="sxs-lookup"><span data-stu-id="8956c-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Creazione di un nuovo controller con ponteggi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="8956c-298">*Creazione di un nuovo controller con ponteggi*</span><span class="sxs-lookup"><span data-stu-id="8956c-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="8956c-299">Nella finestra di dialogo **Aggiungi impalcatura** selezionare **API Web** nel riquadro sinistro, quindi **controller API Web 2 con azioni, usando Entity Framework** nel riquadro centrale, quindi fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="8956c-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="8956c-300">![Selezione del controller Web API 2 con azioni e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selezione del controller Web API 2 con azioni e Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="8956c-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="8956c-301">*Selezione del controller Web API 2 con azioni e Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="8956c-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="8956c-302">Impostare *ApiPersonController* come **nome del controller**, selezionare l'opzione **use Async controller actions** e selezionare **Person (MyHybridSite. Models)** e **PersonContext (MyHybridSite. Models)** rispettivamente come **modello** e le classi del **contesto dati** .</span><span class="sxs-lookup"><span data-stu-id="8956c-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="8956c-303">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8956c-303">Then click **Add**.</span></span>

    <span data-ttu-id="8956c-304">![Aggiunta di un controller API Web con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Aggiunta di un controller API Web con impalcature")</span><span class="sxs-lookup"><span data-stu-id="8956c-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="8956c-305">*Aggiunta di un controller API Web con impalcature*</span><span class="sxs-lookup"><span data-stu-id="8956c-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="8956c-306">In Visual Studio verrà quindi generata la classe **ApiPersonController** con le quattro azioni CRUD da usare con i dati.</span><span class="sxs-lookup"><span data-stu-id="8956c-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="8956c-307">![Dopo la creazione del controller API Web con impalcature](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Dopo la creazione del controller API Web con impalcature")</span><span class="sxs-lookup"><span data-stu-id="8956c-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="8956c-308">*Dopo la creazione del controller API Web con impalcature*</span><span class="sxs-lookup"><span data-stu-id="8956c-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="8956c-309">Aprire il file **ApiPersonController.cs** ed esaminare il metodo di azione *GetPeople* .</span><span class="sxs-lookup"><span data-stu-id="8956c-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="8956c-310">Questo metodo esegue una query sul campo DB del tipo **PersonContext** per ottenere i dati relativi alle persone.</span><span class="sxs-lookup"><span data-stu-id="8956c-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="8956c-311">Si noti ora il commento sopra la definizione del metodo.</span><span class="sxs-lookup"><span data-stu-id="8956c-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="8956c-312">Fornisce l'URI che espone l'azione che verrà utilizzata nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="8956c-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="8956c-313">Per impostazione predefinita, l'API Web è configurata in modo da intercettare le query nel percorso */API* per evitare conflitti con i controller MVC.</span><span class="sxs-lookup"><span data-stu-id="8956c-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="8956c-314">Se è necessario modificare questa impostazione, fare riferimento a [routing in API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8956c-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="8956c-315">Attività 2: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="8956c-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="8956c-316">In questa attività verranno usati gli **strumenti di sviluppo F12** di Internet Explorer per esaminare la risposta completa dal controller API Web.</span><span class="sxs-lookup"><span data-stu-id="8956c-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="8956c-317">Si vedrà come acquisire il traffico di rete per ottenere informazioni più dettagliate sui dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="8956c-318">Assicurarsi che **Internet Explorer** sia selezionato nel pulsante **Avvia** sulla barra degli strumenti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8956c-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opzione Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="8956c-320">Gli **strumenti di sviluppo F12** hanno un ampio set di funzionalità non analizzate in questa esercitazione pratica.</span><span class="sxs-lookup"><span data-stu-id="8956c-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="8956c-321">Per altre informazioni, vedere [uso degli strumenti di sviluppo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="8956c-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>

1. <span data-ttu-id="8956c-322">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-323">Per seguire correttamente questa attività, è necessario che l'applicazione disponga di dati.</span><span class="sxs-lookup"><span data-stu-id="8956c-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="8956c-324">Se il database è vuoto, è possibile tornare all'attività 3 nell'esercizio 2 e seguire i passaggi per creare una nuova persona usando le visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="8956c-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="8956c-325">Nel browser premere **F12** per aprire il pannello **strumenti di sviluppo** .</span><span class="sxs-lookup"><span data-stu-id="8956c-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="8956c-326">Premere **CTRL** + **4** oppure fare clic sull'icona della **rete** , quindi fare clic sul pulsante freccia verde per avviare l'acquisizione del traffico di rete.</span><span class="sxs-lookup"><span data-stu-id="8956c-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="8956c-327">![Avvio dell'acquisizione di rete dell'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Avvio dell'acquisizione di rete dell'API Web")</span><span class="sxs-lookup"><span data-stu-id="8956c-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="8956c-328">*Avvio dell'acquisizione di rete dell'API Web*</span><span class="sxs-lookup"><span data-stu-id="8956c-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="8956c-329">Aggiungere l' **API/ApiPerson** all'URL nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="8956c-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="8956c-330">Verranno ora esaminati i dettagli della risposta da **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="8956c-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="8956c-331">![Recupero dei dati della persona tramite l'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Recupero dei dati della persona tramite l'API Web")</span><span class="sxs-lookup"><span data-stu-id="8956c-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="8956c-332">*Recupero dei dati della persona tramite l'API Web*</span><span class="sxs-lookup"><span data-stu-id="8956c-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-333">Al termine del download, verrà richiesto di eseguire un'azione con il file scaricato.</span><span class="sxs-lookup"><span data-stu-id="8956c-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="8956c-334">Lasciare aperta la finestra di dialogo per poter controllare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="8956c-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="8956c-335">A questo punto, verrà ispezionato il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="8956c-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="8956c-336">A tale scopo, fare clic sulla scheda **Dettagli** e quindi su **corpo della risposta**.</span><span class="sxs-lookup"><span data-stu-id="8956c-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="8956c-337">È possibile verificare che i dati scaricati siano un elenco di oggetti con **ID**proprietà, **nome** ed **età** corrispondenti alla classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="8956c-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="8956c-338">![Visualizzazione del corpo della risposta dell'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Visualizzazione del corpo della risposta dell'API Web")</span><span class="sxs-lookup"><span data-stu-id="8956c-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="8956c-339">*Visualizzazione del corpo della risposta dell'API Web*</span><span class="sxs-lookup"><span data-stu-id="8956c-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="8956c-340">Attività 3: aggiunta di pagine della Guida dell'API Web</span><span class="sxs-lookup"><span data-stu-id="8956c-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="8956c-341">Quando si crea un'API Web, è utile creare una pagina della Guida in modo che altri sviluppatori sappiano come chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="8956c-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="8956c-342">È possibile creare e aggiornare manualmente le pagine della documentazione, ma è preferibile generarle automaticamente per evitare di dover eseguire operazioni di manutenzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="8956c-343">In questa attività si userà un pacchetto NuGet per generare automaticamente le pagine della Guida dell'API Web nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="8956c-344">Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**e quindi fare clic su **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="8956c-344">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="8956c-345">Nella finestra **console di gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="8956c-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="8956c-346">Il pacchetto **Microsoft. AspNet. WebAPI. helpPage** installa gli assembly necessari e aggiunge viste MVC per le pagine della guida nella cartella **areas/helpPage** .</span><span class="sxs-lookup"><span data-stu-id="8956c-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="8956c-347">![Area HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Area HelpPage")</span><span class="sxs-lookup"><span data-stu-id="8956c-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="8956c-348">*Area HelpPage*</span><span class="sxs-lookup"><span data-stu-id="8956c-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="8956c-349">Per impostazione predefinita, le pagine della guida contengono stringhe segnaposto per la documentazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="8956c-350">È possibile utilizzare i commenti relativi alla documentazione XML per creare la documentazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="8956c-351">Per abilitare questa funzionalità, aprire il file **HelpPageConfig.cs** che si trova nella cartella **areas/HelpPage/app\_Start** e rimuovere il commento dalla riga seguente:</span><span class="sxs-lookup"><span data-stu-id="8956c-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="8956c-352">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MyHybridSite**, scegliere **Proprietà** e fare clic sulla scheda **Compila** .</span><span class="sxs-lookup"><span data-stu-id="8956c-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="8956c-353">![Scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Sezione compilazione")</span><span class="sxs-lookup"><span data-stu-id="8956c-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="8956c-354">*Scheda Compila*</span><span class="sxs-lookup"><span data-stu-id="8956c-354">*Build tab*</span></span>
5. <span data-ttu-id="8956c-355">In **output**selezionare **file di documentazione XML**.</span><span class="sxs-lookup"><span data-stu-id="8956c-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="8956c-356">Nella casella di modifica digitare **App\_data/XmlDocument. XML**.</span><span class="sxs-lookup"><span data-stu-id="8956c-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="8956c-357">![Sezione output nella scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Sezione output nella scheda Compila")</span><span class="sxs-lookup"><span data-stu-id="8956c-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="8956c-358">*Sezione output nella scheda Compila*</span><span class="sxs-lookup"><span data-stu-id="8956c-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="8956c-359">Premere **CTRL** + **S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="8956c-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="8956c-360">Aprire il file **ApiPersonController.cs** dalla cartella **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="8956c-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="8956c-361">Immettere una nuova riga tra la firma del metodo *GetPeople* e il commento *//Get API/ApiPerson* , quindi digitare tre barre.</span><span class="sxs-lookup"><span data-stu-id="8956c-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-362">Visual Studio inserisce automaticamente gli elementi XML che definiscono la documentazione del metodo.</span><span class="sxs-lookup"><span data-stu-id="8956c-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="8956c-363">Aggiungere un testo di riepilogo e il valore restituito per il metodo *GetPeople* .</span><span class="sxs-lookup"><span data-stu-id="8956c-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="8956c-364">Dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="8956c-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="8956c-365">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="8956c-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="8956c-366">Aggiungere **/Help** all'URL nella barra degli indirizzi per passare alla pagina della guida.</span><span class="sxs-lookup"><span data-stu-id="8956c-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="8956c-367">![Pagina della Guida API Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Pagina della Guida API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="8956c-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="8956c-368">*Pagina della Guida API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="8956c-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8956c-369">Il contenuto principale della pagina è una tabella di API, raggruppate per controller.</span><span class="sxs-lookup"><span data-stu-id="8956c-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="8956c-370">Le voci della tabella vengono generate in modo dinamico, usando l'interfaccia **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="8956c-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="8956c-371">Se si aggiunge o si aggiorna un controller API, la tabella verrà aggiornata automaticamente alla successiva compilazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8956c-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="8956c-372">La colonna **API** elenca il metodo HTTP e l'URI relativo.</span><span class="sxs-lookup"><span data-stu-id="8956c-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="8956c-373">La colonna **Descrizione** contiene informazioni estratte dalla documentazione del metodo.</span><span class="sxs-lookup"><span data-stu-id="8956c-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="8956c-374">Si noti che la descrizione aggiunta sopra la definizione del metodo viene visualizzata nella colonna Descrizione.</span><span class="sxs-lookup"><span data-stu-id="8956c-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="8956c-375">![Descrizione del metodo API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Descrizione del metodo API")</span><span class="sxs-lookup"><span data-stu-id="8956c-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="8956c-376">*Descrizione del metodo API*</span><span class="sxs-lookup"><span data-stu-id="8956c-376">*API method description*</span></span>
13. <span data-ttu-id="8956c-377">Fare clic su uno dei metodi API per passare a una pagina con informazioni più dettagliate, inclusi i corpi della risposta di esempio.</span><span class="sxs-lookup"><span data-stu-id="8956c-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="8956c-378">![Pagina informazioni dettagliate](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Pagina informazioni dettagliate")</span><span class="sxs-lookup"><span data-stu-id="8956c-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="8956c-379">*Pagina informazioni dettagliate*</span><span class="sxs-lookup"><span data-stu-id="8956c-379">*Detailed information page*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8956c-380">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="8956c-380">Summary</span></span>

<span data-ttu-id="8956c-381">Completando questa esercitazione pratica si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="8956c-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="8956c-382">Creare una nuova applicazione Web usando l'esperienza ASP.NET in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8956c-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="8956c-383">Integrare più tecnologie ASP.NET in un unico progetto</span><span class="sxs-lookup"><span data-stu-id="8956c-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="8956c-384">Generare controller e visualizzazioni MVC dalle classi del modello usando l'impalcatura ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8956c-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="8956c-385">Generare controller API Web che usano funzionalità come la programmazione asincrona e l'accesso ai dati tramite Entity Framework</span><span class="sxs-lookup"><span data-stu-id="8956c-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="8956c-386">Genera automaticamente pagine della Guida dell'API Web per i controller</span><span class="sxs-lookup"><span data-stu-id="8956c-386">Automatically generate Web API Help Pages for your controllers</span></span>
