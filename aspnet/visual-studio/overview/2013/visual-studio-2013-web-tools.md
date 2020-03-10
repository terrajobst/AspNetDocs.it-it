---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio pratico: strumenti Web di Visual Studio 2013 | Microsoft Docs'
author: rick-anderson
description: Visual Studio è un ambiente di sviluppo eccellente per. Progetti Web e Windows basati su .NET. Include un editor di testo potente che può essere usato facilmente...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622675"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="7f058-104">Laboratorio pratico: strumenti Web di Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7f058-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="7f058-105">dal [team di Web Camp](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7f058-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7f058-106">Scarica il kit di formazione di Web Camp</span><span class="sxs-lookup"><span data-stu-id="7f058-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="7f058-107">Visual Studio è un ambiente di sviluppo eccellente per. Progetti Web e Windows basati su .NET.</span><span class="sxs-lookup"><span data-stu-id="7f058-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="7f058-108">Include un editor di testo potente che può essere facilmente usato per modificare i file autonomi senza un progetto.</span><span class="sxs-lookup"><span data-stu-id="7f058-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="7f058-109">Visual Studio gestisce un albero di analisi con funzionalità complete quando si modifica ogni file.</span><span class="sxs-lookup"><span data-stu-id="7f058-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="7f058-110">Questo consente a Visual Studio di fornire azioni di completamento automatico e basate su documenti senza precedenti, rendendo l'esperienza di sviluppo molto più veloce e più gradevole.</span><span class="sxs-lookup"><span data-stu-id="7f058-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="7f058-111">Queste funzionalità sono particolarmente potenti nei documenti HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="7f058-112">Tutte queste potenzialità sono disponibili anche per le estensioni, semplificando l'estensione degli editor con nuove funzionalità avanzate in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="7f058-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="7f058-113">Web Essentials è una raccolta di (per lo più) miglioramenti correlati al Web per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="7f058-114">Sono inclusi numerosi nuovi completamenti IntelliSense (in particolare per CSS), nuove funzionalità di Browser Link, JSHint automatici per file JavaScript, nuovi avvisi per HTML e CSS e molte altre funzionalità essenziali per lo sviluppo Web moderno.</span><span class="sxs-lookup"><span data-stu-id="7f058-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="7f058-115">Tutti i codici e i frammenti di codice di esempio sono inclusi nel kit di formazione di Web Camp, disponibile all' [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7f058-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="7f058-116">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7f058-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7f058-117">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="7f058-117">Objectives</span></span>

<span data-ttu-id="7f058-118">In questo laboratorio pratico si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="7f058-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="7f058-119">Usare le nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio frammenti di codice HTML5 avanzati e codifica Zen</span><span class="sxs-lookup"><span data-stu-id="7f058-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="7f058-120">Usare le nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione colori e la descrizione comando della matrice del browser</span><span class="sxs-lookup"><span data-stu-id="7f058-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="7f058-121">Usare le nuove funzionalità dell'editor JavaScript incluse in Web Essentials, ad esempio Extract to file e IntelliSense per tutti gli elementi HTML</span><span class="sxs-lookup"><span data-stu-id="7f058-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="7f058-122">Scambiare dati tra il browser e Visual Studio usando Browser Link</span><span class="sxs-lookup"><span data-stu-id="7f058-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7f058-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7f058-123">Prerequisites</span></span>

<span data-ttu-id="7f058-124">Per completare questa esercitazione pratica, è necessario quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7f058-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="7f058-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="7f058-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="7f058-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="7f058-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="7f058-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="7f058-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7f058-128">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7f058-128">Setup</span></span>

<span data-ttu-id="7f058-129">Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="7f058-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="7f058-130">Aprire una finestra di Esplora risorse e passare alla cartella di **origine** del Lab.</span><span class="sxs-lookup"><span data-stu-id="7f058-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="7f058-131">Fare clic con il pulsante destro del mouse su **Setup. cmd** e scegliere **Esegui come amministratore** per avviare il processo di installazione che configurerà l'ambiente e installerà i frammenti di codice di Visual Studio per questo Lab.</span><span class="sxs-lookup"><span data-stu-id="7f058-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="7f058-132">Se viene visualizzata la finestra di dialogo controllo account utente, confermare l'azione per continuare.</span><span class="sxs-lookup"><span data-stu-id="7f058-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="7f058-133">Prima di eseguire l'installazione, verificare di aver selezionato tutte le dipendenze per questo Lab.</span><span class="sxs-lookup"><span data-stu-id="7f058-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="7f058-134">Uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="7f058-134">Using the Code Snippets</span></span>

<span data-ttu-id="7f058-135">Nell'intero documento del Lab verrà richiesto di inserire blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="7f058-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="7f058-136">Per praticità, la maggior parte di questo codice viene fornita come Visual Studio Code frammenti, a cui è possibile accedere da Visual Studio 2013 per evitare di aggiungerla manualmente.</span><span class="sxs-lookup"><span data-stu-id="7f058-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="7f058-137">Ogni esercizio è accompagnato da una soluzione iniziale che si trova nella cartella **Begin** dell'esercizio che consente di seguire ogni esercizio in modo indipendente dagli altri.</span><span class="sxs-lookup"><span data-stu-id="7f058-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="7f058-138">Tenere presente che i frammenti di codice aggiunti durante un esercizio risultano mancanti da queste soluzioni iniziali e potrebbero non funzionare fino a quando non è stato completato l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="7f058-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="7f058-139">All'interno del codice sorgente per un esercizio, si troverà anche una cartella **finale** contenente una soluzione di Visual Studio con il codice risultante dal completamento dei passaggi nell'esercizio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="7f058-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="7f058-140">È possibile usare queste soluzioni come materiale sussidiario se è necessaria ulteriore assistenza durante l'utilizzo di questa esercitazione pratica.</span><span class="sxs-lookup"><span data-stu-id="7f058-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7f058-141">Esercizi</span><span class="sxs-lookup"><span data-stu-id="7f058-141">Exercises</span></span>

<span data-ttu-id="7f058-142">Questa esercitazione pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7f058-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="7f058-143">Uso di Browser Link e Web Essentials</span><span class="sxs-lookup"><span data-stu-id="7f058-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="7f058-144">Vantaggi dei frammenti di codice e di IntelliSense</span><span class="sxs-lookup"><span data-stu-id="7f058-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="7f058-145">Quando si avvia Visual Studio per la prima volta, è necessario selezionare una delle raccolte di impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="7f058-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="7f058-146">Ogni raccolta predefinita è progettata in modo da corrispondere a un particolare stile di sviluppo e determina il layout delle finestre, il comportamento dell'editor, i frammenti di codice IntelliSense e le opzioni della finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7f058-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="7f058-147">Le procedure descritte in questa esercitazione descrivono le azioni necessarie per eseguire un'attività specifica in Visual Studio quando si usa la raccolta di **impostazioni di sviluppo generali** .</span><span class="sxs-lookup"><span data-stu-id="7f058-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="7f058-148">Se si sceglie una raccolta di impostazioni diversa per l'ambiente di sviluppo, è possibile che siano presenti differenze nei passaggi da tenere in considerazione.</span><span class="sxs-lookup"><span data-stu-id="7f058-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="7f058-149">Esercizio 1: utilizzo di Browser Link e Web Essentials</span><span class="sxs-lookup"><span data-stu-id="7f058-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="7f058-150">**Web Essentials** è un'estensione di Visual Studio che aggiunge un'ampia gamma di funzionalità utili per lo sviluppo Web moderno, per lo più più veloce e gradevole.</span><span class="sxs-lookup"><span data-stu-id="7f058-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="7f058-151">È possibile installare Web Essentials dalla raccolta di estensioni in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="7f058-152">**Browser link** è una nuova funzionalità inclusa in Visual Studio 2013 che fornisce un canale tra l'IDE di Visual Studio e qualsiasi browser aperto per lo scambio di dati tra l'applicazione Web e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="7f058-153">Web Essentials estende Browser Link con strumenti per modificare il modello a oggetti DOM e gli stili CSS delle pagine Web direttamente dal browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="7f058-154">In questo esercizio si esamineranno alcune delle funzionalità supportate da **Web Essentials** e **browser link** per migliorare una semplice pagina di quiz.</span><span class="sxs-lookup"><span data-stu-id="7f058-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="7f058-155">Attività 1: esecuzione del progetto in più browser</span><span class="sxs-lookup"><span data-stu-id="7f058-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="7f058-156">In questa attività verrà configurata l'applicazione Web per l'esecuzione in più browser contemporaneamente, operazione utile per i test tra browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="7f058-157">Aprire **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="7f058-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="7f058-158">Nel menu **file** selezionare **Apri | Progetto/soluzione...** e passare a **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** nella cartella di **origine** del Lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="7f058-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="7f058-159">Selezionare **Begin. sln** e fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="7f058-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="7f058-160">Nella barra degli strumenti di Visual Studio espandere il menu del browser e selezionare **Sfoglia con...** .</span><span class="sxs-lookup"><span data-stu-id="7f058-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="7f058-161">![Sfoglia con l'opzione di menu](visual-studio-2013-web-tools/_static/image1.png "Sfoglia con... nel menu del browser")</span><span class="sxs-lookup"><span data-stu-id="7f058-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="7f058-162">*Sfoglia con l'opzione di menu*</span><span class="sxs-lookup"><span data-stu-id="7f058-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="7f058-163">Nella finestra di dialogo **Sfoglia con** selezionare sia **Google Chrome** che **Internet Explorer** tenendo premuto il tasto **CTRL** e facendo clic su **Imposta come predefinito**.</span><span class="sxs-lookup"><span data-stu-id="7f058-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="7f058-164">![Finestra di dialogo Sfoglia con](visual-studio-2013-web-tools/_static/image2.png "Finestra di dialogo Sfoglia con")</span><span class="sxs-lookup"><span data-stu-id="7f058-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="7f058-165">*Selezione di più browser predefiniti*</span><span class="sxs-lookup"><span data-stu-id="7f058-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="7f058-166">Sia Google Chrome che Internet Explorer dovrebbero ora essere visualizzati come browser predefiniti.</span><span class="sxs-lookup"><span data-stu-id="7f058-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="7f058-167">Fare clic su **Annulla**per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7f058-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="7f058-168">![Google Chrome e Internet Explorer come browser predefiniti](visual-studio-2013-web-tools/_static/image3.png "Browser predefiniti per Google Chrome e Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="7f058-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="7f058-169">*Google Chrome e Internet Explorer come browser predefiniti*</span><span class="sxs-lookup"><span data-stu-id="7f058-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-170">Al termine della configurazione dei browser predefiniti, nel menu del browser viene selezionata l'opzione **più browser** .</span><span class="sxs-lookup"><span data-stu-id="7f058-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="7f058-171">![Più browser](visual-studio-2013-web-tools/_static/image4.png "Più browser")</span><span class="sxs-lookup"><span data-stu-id="7f058-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="7f058-172">Premere **CTRL** + **F5** per eseguire l'applicazione senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="7f058-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="7f058-173">Quando entrambe le finestre del browser sono aperte, posizionarne una sopra l'altra per visualizzare gli aggiornamenti contemporaneamente in entrambi i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="7f058-174">Nei browser dovrebbe essere visualizzata una pagina Web con un rettangolo blu chiaro.</span><span class="sxs-lookup"><span data-stu-id="7f058-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="7f058-175">![Posizionamento di un browser sopra l'altro](visual-studio-2013-web-tools/_static/image5.png "Posizionamento di un browser sopra l'altro")</span><span class="sxs-lookup"><span data-stu-id="7f058-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="7f058-176">*Posizionamento di un browser sopra l'altro*</span><span class="sxs-lookup"><span data-stu-id="7f058-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="7f058-177">Non chiudere i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-177">Do not close the browsers.</span></span> <span data-ttu-id="7f058-178">Verranno utilizzati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="7f058-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="7f058-179">Attività 2-uso della codifica Zen per creare elementi HTML</span><span class="sxs-lookup"><span data-stu-id="7f058-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="7f058-180">La **codifica Zen** è un plug-in di editor per la codifica e la modifica di codice HTML, XML, XSL (o qualsiasi altro formato di codice strutturato) ad alta velocità.</span><span class="sxs-lookup"><span data-stu-id="7f058-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="7f058-181">Il nucleo di questo plug-in è un potente motore di abbreviazione che consente di espandere espressioni, analogamente ai selettori CSS, nel codice HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="7f058-182">Il codice Zen è un modo rapido per scrivere codice HTML usando una sintassi del selettore di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="7f058-183">In questo esercizio si userà la funzionalità di codifica Zen fornita da Web Essentials per generare i pulsanti HTML che rappresentano le opzioni della domanda.</span><span class="sxs-lookup"><span data-stu-id="7f058-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="7f058-184">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="7f058-185">Aprire il file **index. cshtml** che si trova nella cartella **views** | **Home** .</span><span class="sxs-lookup"><span data-stu-id="7f058-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="7f058-186">Sostituire il **&lt;!--todo: aggiungere qui le opzioni,&gt;** commento con il codice seguente e premere **Tab**.</span><span class="sxs-lookup"><span data-stu-id="7f058-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="7f058-187">Il codice deve essere espanso in HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="7f058-188">![HTML espanso](visual-studio-2013-web-tools/_static/image6.png "HTML espanso")</span><span class="sxs-lookup"><span data-stu-id="7f058-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="7f058-189">*HTML espanso*</span><span class="sxs-lookup"><span data-stu-id="7f058-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-190">Per altre informazioni sulla sintassi di codifica Zen, vedere l' [articolo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)seguente.</span><span class="sxs-lookup"><span data-stu-id="7f058-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="7f058-191">Fare clic sul pulsante **Aggiorna browser collegati** per aggiornare entrambi i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="7f058-192">![Aggiornare i browser collegati](visual-studio-2013-web-tools/_static/image7.png "Aggiornare i browser collegati")</span><span class="sxs-lookup"><span data-stu-id="7f058-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="7f058-193">*Aggiornare i browser collegati*</span><span class="sxs-lookup"><span data-stu-id="7f058-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="7f058-194">![Internet Explorer-pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-pagina aggiornata con quattro pulsanti")</span><span class="sxs-lookup"><span data-stu-id="7f058-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="7f058-195">*Internet Explorer-pagina aggiornata con quattro pulsanti*</span><span class="sxs-lookup"><span data-stu-id="7f058-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="7f058-196">![Google Chrome-pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-pagina aggiornata con quattro pulsanti")</span><span class="sxs-lookup"><span data-stu-id="7f058-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="7f058-197">*Google Chrome-pagina aggiornata con quattro pulsanti*</span><span class="sxs-lookup"><span data-stu-id="7f058-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="7f058-198">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="7f058-199">Sono stati aggiunti i pulsanti alla pagina, ma è comunque necessario aggiungere una domanda di modello.</span><span class="sxs-lookup"><span data-stu-id="7f058-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="7f058-200">A tale scopo, si userà una nuova funzionalità di Web Essentials denominata **Loren ipsum Generator**.</span><span class="sxs-lookup"><span data-stu-id="7f058-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="7f058-201">Individuare l'elemento **div** con l'attributo **Class** **front**.</span><span class="sxs-lookup"><span data-stu-id="7f058-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="7f058-202">Aggiungere il codice seguente come primo elemento figlio del **div**e premere **Tab**.</span><span class="sxs-lookup"><span data-stu-id="7f058-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="7f058-203">Il codice deve essere espanso in HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="7f058-204">![Loren ipsum generato automaticamente](visual-studio-2013-web-tools/_static/image10.png "Loren ipsum generato automaticamente")</span><span class="sxs-lookup"><span data-stu-id="7f058-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="7f058-205">*Loren ipsum generato automaticamente*</span><span class="sxs-lookup"><span data-stu-id="7f058-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-206">Come parte della codifica Zen, è ora possibile generare il codice Loreal ipsum direttamente nell'editor HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="7f058-207">È sufficiente digitare **Loret** e premere **Tab** per inserire un testo di 30 parole Loret ipsum.</span><span class="sxs-lookup"><span data-stu-id="7f058-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="7f058-208">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="7f058-208">E.g.</span></span> <span data-ttu-id="7f058-209">*lorem10* inserisce 10 parole ipsum di Lore.</span><span class="sxs-lookup"><span data-stu-id="7f058-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="7f058-210">Si aggiungerà un logo nella parte superiore della domanda usando un'altra nuova funzionalità di Web Essentials denominata **Loret pixel Generator**.</span><span class="sxs-lookup"><span data-stu-id="7f058-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="7f058-211">Aggiungere il codice seguente come primo elemento figlio dell'elemento **div** con **contenitore** come valore della **classe** e premere **Tab**.</span><span class="sxs-lookup"><span data-stu-id="7f058-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="7f058-212">Il codice deve espandersi in HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="7f058-213">![Generato automaticamente dal pixel Lore](visual-studio-2013-web-tools/_static/image11.png "Generato automaticamente dal pixel Lore")</span><span class="sxs-lookup"><span data-stu-id="7f058-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="7f058-214">*Generato automaticamente dal pixel Lore*</span><span class="sxs-lookup"><span data-stu-id="7f058-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-215">Come parte della codifica Zen, è anche possibile generare codice del pixel di Lorein direttamente nell'editor HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="7f058-216">È sufficiente digitare **pix-200x200-Animals** e premere **Tab** e viene inserito un tag **IMG** con un'immagine 200x200 di un animale.</span><span class="sxs-lookup"><span data-stu-id="7f058-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="7f058-217">Per altre informazioni, vedere il [pixel Lore](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="7f058-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="7f058-218">Fare clic sul pulsante **Aggiorna browser collegati** per aggiornare entrambi i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="7f058-219">![Internet Explorer-immagine e testo generati automaticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-immagine e testo generati automaticamente")</span><span class="sxs-lookup"><span data-stu-id="7f058-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="7f058-220">*Internet Explorer-immagine e testo generati automaticamente*</span><span class="sxs-lookup"><span data-stu-id="7f058-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="7f058-221">![Google Chrome-immagine e testo generati automaticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-immagine e testo generati automaticamente")</span><span class="sxs-lookup"><span data-stu-id="7f058-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="7f058-222">*Google Chrome-immagine e testo generati automaticamente*</span><span class="sxs-lookup"><span data-stu-id="7f058-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-223">Poiché l'immagine viene selezionata in modo casuale quando si aggiunge il frammento di codice, l'immagine mostrata nei browser potrebbe variare.</span><span class="sxs-lookup"><span data-stu-id="7f058-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="7f058-224">Non chiudere i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-224">Do not close the browsers.</span></span> <span data-ttu-id="7f058-225">Verranno utilizzati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="7f058-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="7f058-226">Attività 3-aggiornamento di una proprietà di stile</span><span class="sxs-lookup"><span data-stu-id="7f058-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="7f058-227">In questa attività si userà la funzionalità della modalità di **controllo** browser link per rilevare la posizione esatta in cui viene generato l'elemento DOM specifico e quindi aggiornare la proprietà Color di tale elemento usando una selezione colori fornita da Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="7f058-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="7f058-228">Nel browser Internet Explorer premere **CTRL** + **ALT** + **I** per abilitare la modalità di controllo.</span><span class="sxs-lookup"><span data-stu-id="7f058-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="7f058-229">Spostare il puntatore sul bordo blu chiaro e fare clic su.</span><span class="sxs-lookup"><span data-stu-id="7f058-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="7f058-230">![Passaggio del puntatore sul bordo blu chiaro](visual-studio-2013-web-tools/_static/image14.png "Passaggio del puntatore sul bordo blu chiaro")</span><span class="sxs-lookup"><span data-stu-id="7f058-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="7f058-231">*Passaggio del puntatore sul bordo blu chiaro*</span><span class="sxs-lookup"><span data-stu-id="7f058-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="7f058-232">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="7f058-233">Si noti che l'elemento HTML selezionato nel browser è selezionato anche nell'editor HTML di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="7f058-234">![Elemento HTML selezionato nell'editor HTML di Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML selezionato nell'editor HTML di Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="7f058-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="7f058-235">*Elemento HTML selezionato nell'editor HTML di Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="7f058-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="7f058-236">Verrà ora aggiornata la classe CSS **anteriore** per modificare lo stile dell'elemento selezionato.</span><span class="sxs-lookup"><span data-stu-id="7f058-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="7f058-237">A tale scopo, premere **CTRL** + per aprire la **casella di ricerca** **passa a** .</span><span class="sxs-lookup"><span data-stu-id="7f058-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="7f058-238">Digitare **site. CSS** e premere **invio** per aprire il file.</span><span class="sxs-lookup"><span data-stu-id="7f058-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="7f058-239">![Apertura del file site. CSS](visual-studio-2013-web-tools/_static/image16.png "Apertura del file site. CSS")</span><span class="sxs-lookup"><span data-stu-id="7f058-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="7f058-240">*Apertura del file site. CSS*</span><span class="sxs-lookup"><span data-stu-id="7f058-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="7f058-241">Premere **CTRL** + **F** e digitare **. Flip-container. front** per trovare il selettore CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="7f058-242">Fare clic sul quadrato blu nella proprietà border della classe per aprire la selezione colori.</span><span class="sxs-lookup"><span data-stu-id="7f058-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="7f058-243">![Apertura della selezione colori](visual-studio-2013-web-tools/_static/image17.png "Apertura della selezione colori")</span><span class="sxs-lookup"><span data-stu-id="7f058-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="7f058-244">*Apertura della selezione colori*</span><span class="sxs-lookup"><span data-stu-id="7f058-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="7f058-245">Espandere la selezione colori facendo clic sul pulsante con la freccia di espansione e selezionando un nuovo colore.</span><span class="sxs-lookup"><span data-stu-id="7f058-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="7f058-246">![Espansione della selezione colori](visual-studio-2013-web-tools/_static/image18.png "Espansione della selezione colori")</span><span class="sxs-lookup"><span data-stu-id="7f058-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="7f058-247">*Espansione della selezione colori*</span><span class="sxs-lookup"><span data-stu-id="7f058-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="7f058-248">Premere **CTRL** + **ALT** + **invio** per aggiornare i browser collegati.</span><span class="sxs-lookup"><span data-stu-id="7f058-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="7f058-249">Passare a Internet Explorer e osservare come è stato modificato il colore del bordo.</span><span class="sxs-lookup"><span data-stu-id="7f058-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="7f058-250">![Internet Explorer-colore bordo aggiornato](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-colore bordo aggiornato")</span><span class="sxs-lookup"><span data-stu-id="7f058-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="7f058-251">*Internet Explorer-colore bordo aggiornato*</span><span class="sxs-lookup"><span data-stu-id="7f058-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="7f058-252">Passa a Google Chrome e osserva come è stato modificato il colore del bordo.</span><span class="sxs-lookup"><span data-stu-id="7f058-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="7f058-253">![Google Chrome-colore bordo aggiornato](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-colore bordo aggiornato")</span><span class="sxs-lookup"><span data-stu-id="7f058-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="7f058-254">*Google Chrome-colore bordo aggiornato*</span><span class="sxs-lookup"><span data-stu-id="7f058-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="7f058-255">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="7f058-256">Passare alla fine del file **site. CSS** e premere **CTRL** + **F** per individuare il selettore **. BTN** .</span><span class="sxs-lookup"><span data-stu-id="7f058-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="7f058-257">Si noti che la proprietà **-webkit-border-radius** è sottolineata in verde.</span><span class="sxs-lookup"><span data-stu-id="7f058-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="7f058-258">![-webkit-border-radius (proprietà) del selettore BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius (proprietà) del selettore BTN")</span><span class="sxs-lookup"><span data-stu-id="7f058-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="7f058-259">*-webkit-border-radius (proprietà) del selettore BTN*</span><span class="sxs-lookup"><span data-stu-id="7f058-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="7f058-260">Posizionare il punto di inserimento nella proprietà **-webkit-border-radius** .</span><span class="sxs-lookup"><span data-stu-id="7f058-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="7f058-261">Verrà visualizzata una linea blu sotto la prima lettera della prima parola della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7f058-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="7f058-262">Si tratta dello **Smart tag**.</span><span class="sxs-lookup"><span data-stu-id="7f058-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="7f058-263">Premere **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="7f058-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="7f058-264">per aprire il menu suggerimenti e fare clic su **Aggiungi proprietà standard mancante (border-radius)** .</span><span class="sxs-lookup"><span data-stu-id="7f058-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="7f058-265">![Aggiungere il suggerimento della proprietà standard mancante](visual-studio-2013-web-tools/_static/image22.png "Aggiungere il suggerimento della proprietà standard mancante")</span><span class="sxs-lookup"><span data-stu-id="7f058-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="7f058-266">*Aggiungere il suggerimento della proprietà standard mancante*</span><span class="sxs-lookup"><span data-stu-id="7f058-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="7f058-267">La proprietà **border-radius** viene aggiunta automaticamente alla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="7f058-268">![Aggiunta proprietà standard mancante](visual-studio-2013-web-tools/_static/image23.png "Aggiunta proprietà standard mancante")</span><span class="sxs-lookup"><span data-stu-id="7f058-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="7f058-269">*Aggiunta proprietà standard mancante*</span><span class="sxs-lookup"><span data-stu-id="7f058-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="7f058-270">Spostare il puntatore sulla proprietà **border-radius** per visualizzare la **Descrizione comando della matrice del browser**.</span><span class="sxs-lookup"><span data-stu-id="7f058-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="7f058-271">La **Descrizione comando della matrice del browser** Mostra la disponibilità della proprietà in ogni browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="7f058-272">![Descrizione comando matrice browser](visual-studio-2013-web-tools/_static/image24.png "Descrizione comando matrice browser")</span><span class="sxs-lookup"><span data-stu-id="7f058-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="7f058-273">*Descrizione comando matrice browser*</span><span class="sxs-lookup"><span data-stu-id="7f058-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="7f058-274">Si noti che il valore della proprietà **border-radius** è ancora sottolineato.</span><span class="sxs-lookup"><span data-stu-id="7f058-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="7f058-275">Spostare il puntatore del mouse sul valore per visualizzare il messaggio di avviso.</span><span class="sxs-lookup"><span data-stu-id="7f058-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="7f058-276">![Avviso del valore della proprietà border-radius](visual-studio-2013-web-tools/_static/image25.png "Avviso del valore della proprietà border-radius")</span><span class="sxs-lookup"><span data-stu-id="7f058-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="7f058-277">*Avviso del valore della proprietà border-radius*</span><span class="sxs-lookup"><span data-stu-id="7f058-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="7f058-278">Rimuovere l'unità del valore della proprietà **border-radius** come suggerito dalla descrizione comando.</span><span class="sxs-lookup"><span data-stu-id="7f058-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="7f058-279">Come **border-radius** è la proprietà standard per definire il modo in cui gli angoli dei bordi arrotondati sono, è possibile rimuovere la proprietà **-webkit-border-radius** e il valore dalla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="7f058-280">Posizionare il punto di inserimento nella proprietà di **ritorno a capo automatico** e notare che lo smart tag viene visualizzato anche sotto.</span><span class="sxs-lookup"><span data-stu-id="7f058-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="7f058-281">Aprire il menu e fare clic su **Aggiungi specifiche del fornitore mancanti**.</span><span class="sxs-lookup"><span data-stu-id="7f058-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="7f058-282">![Aggiunta del suggerimento per i fornitori mancanti](visual-studio-2013-web-tools/_static/image26.png "Aggiunta del suggerimento per i fornitori mancanti")</span><span class="sxs-lookup"><span data-stu-id="7f058-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="7f058-283">*Aggiunta del suggerimento per i fornitori mancanti*</span><span class="sxs-lookup"><span data-stu-id="7f058-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="7f058-284">La proprietà **-MS-Word-Wrap** viene aggiunta automaticamente alla regola CSS.</span><span class="sxs-lookup"><span data-stu-id="7f058-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="7f058-285">![Proprietà specifica del fornitore aggiunta](visual-studio-2013-web-tools/_static/image27.png "Proprietà specifica del fornitore aggiunta")</span><span class="sxs-lookup"><span data-stu-id="7f058-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="7f058-286">*Proprietà specifica del fornitore aggiunta*</span><span class="sxs-lookup"><span data-stu-id="7f058-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="7f058-287">Attività 4: aggiornamento del codice HTML dal browser</span><span class="sxs-lookup"><span data-stu-id="7f058-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="7f058-288">In questa attività verrà utilizzata la funzionalità **modalità progettazione** browser link per modificare l'oggetto DOM dal browser e trasferire le modifiche al file di origine HTML in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f058-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="7f058-289">In Google Chrome premere **CTRL** + **ALT** + **D** per abilitare la modalità progettazione.</span><span class="sxs-lookup"><span data-stu-id="7f058-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="7f058-290">Spostare il puntatore sull'etichetta **Loret ipsum dolor sit amet** e fare clic su.</span><span class="sxs-lookup"><span data-stu-id="7f058-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="7f058-291">![Modifica della domanda](visual-studio-2013-web-tools/_static/image28.png "Modifica della domanda")</span><span class="sxs-lookup"><span data-stu-id="7f058-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="7f058-292">*Modifica della domanda*</span><span class="sxs-lookup"><span data-stu-id="7f058-292">*Editing the question*</span></span>
3. <span data-ttu-id="7f058-293">Verrà visualizzato un cursore.</span><span class="sxs-lookup"><span data-stu-id="7f058-293">A cursor should appear.</span></span> <span data-ttu-id="7f058-294">Sostituire il testo originale con *quello che si verifica quando scrivo una domanda più lunga?* e quindi premere **ESC** per uscire dalla modalità progettazione.</span><span class="sxs-lookup"><span data-stu-id="7f058-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="7f058-295">![Domanda modificata](visual-studio-2013-web-tools/_static/image29.png "Domanda modificata")</span><span class="sxs-lookup"><span data-stu-id="7f058-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="7f058-296">*Domanda modificata*</span><span class="sxs-lookup"><span data-stu-id="7f058-296">*Question edited*</span></span>
4. <span data-ttu-id="7f058-297">Tornare a Visual Studio e aprire **index. cshtml**, se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="7f058-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="7f058-298">Si noti che il testo interno dell'elemento **&lt;p&gt;** è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="7f058-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="7f058-299">![Domanda aggiornata nella pagina HTML](visual-studio-2013-web-tools/_static/image30.png "Domanda aggiornata nella pagina HTML")</span><span class="sxs-lookup"><span data-stu-id="7f058-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="7f058-300">*Domanda aggiornata nella pagina HTML*</span><span class="sxs-lookup"><span data-stu-id="7f058-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="7f058-301">Attività 5-revisione degli avvisi correlati a SEO</span><span class="sxs-lookup"><span data-stu-id="7f058-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="7f058-302">**Ottimizzazione del motore di ricerca** (SEO) è il processo che consente di aumentare le dimensioni di un sito Web nell'elenco dei risultati di un motore di ricerca.</span><span class="sxs-lookup"><span data-stu-id="7f058-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="7f058-303">Maggiore è il numero di classificazioni del sito e più coerente è elencato, più i visitatori otterranno il sito dal motore di ricerca.</span><span class="sxs-lookup"><span data-stu-id="7f058-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="7f058-304">Web Essentials incorpora uno strumento analitico che esamina il codice HTML, segnala i problemi rilevati e fornisce assistenza per correggerli.</span><span class="sxs-lookup"><span data-stu-id="7f058-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="7f058-305">Passare al menu **Visualizza** e fare clic su **Elenco errori** per aprire la finestra di **Elenco errori** .</span><span class="sxs-lookup"><span data-stu-id="7f058-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="7f058-306">![Elenco errori dal menu Visualizza](visual-studio-2013-web-tools/_static/image31.png "Elenco errori dal menu Visualizza")</span><span class="sxs-lookup"><span data-stu-id="7f058-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="7f058-307">*Elenco errori dal menu Visualizza*</span><span class="sxs-lookup"><span data-stu-id="7f058-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="7f058-308">Si noti che è presente un avviso di SEO che informa che manca un tag **&lt;meta&gt;** per la descrizione della pagina.</span><span class="sxs-lookup"><span data-stu-id="7f058-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="7f058-309">Fare doppio clic sulla voce di avviso SEO per correggerla.</span><span class="sxs-lookup"><span data-stu-id="7f058-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="7f058-310">![Finestra Elenco errori](visual-studio-2013-web-tools/_static/image32.png "finestra Elenco errori")</span><span class="sxs-lookup"><span data-stu-id="7f058-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="7f058-311">*Finestra Elenco errori*</span><span class="sxs-lookup"><span data-stu-id="7f058-311">*Error List window*</span></span>
3. <span data-ttu-id="7f058-312">Nella finestra di dialogo **Web Essentials** fare clic su **Sì** per inserire una descrizione &lt;tag meta&gt;.</span><span class="sxs-lookup"><span data-stu-id="7f058-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="7f058-313">![Finestra di dialogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Finestra di dialogo Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="7f058-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="7f058-314">*Finestra di dialogo Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="7f058-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="7f058-315">Viene aperto l'editor per **\_layout. cshtml** e il tag **&lt;meta&gt;** viene aggiunto automaticamente alla sezione **Head** del file HTML.</span><span class="sxs-lookup"><span data-stu-id="7f058-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="7f058-316">![Tag meta aggiunto automaticamente nella pagina _Layout](visual-studio-2013-web-tools/_static/image34.png "Tag meta aggiunto automaticamente nella pagina _Layout")</span><span class="sxs-lookup"><span data-stu-id="7f058-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="7f058-317">*Tag meta aggiunto automaticamente alla pagina layout \_*</span><span class="sxs-lookup"><span data-stu-id="7f058-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="7f058-318">Modificare il valore dell'attributo **Content** in *GeekQuiz* e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7f058-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="7f058-319">Esercizio 2: uso dei frammenti di codice e di IntelliSense</span><span class="sxs-lookup"><span data-stu-id="7f058-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="7f058-320">Con Web Essentials, l'editor HTML è stato esteso con funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="7f058-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="7f058-321">In questo esercizio verranno visualizzate alcune nuove funzionalità utili per lo sviluppo di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="7f058-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="7f058-322">Attività 1-uso di IntelliSense nei documenti HTML</span><span class="sxs-lookup"><span data-stu-id="7f058-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="7f058-323">La prima nuova funzionalità che verrà visualizzata in questa attività è denominata **IntelliSense dinamico**.</span><span class="sxs-lookup"><span data-stu-id="7f058-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="7f058-324">IntelliSense dinamico legge altri tag e attributi per dedurre gli ID possibili che si utilizzeranno.</span><span class="sxs-lookup"><span data-stu-id="7f058-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="7f058-325">In questa attività verrà creato un nuovo elemento modulo HTML che contiene un'etichetta e un campo di input.</span><span class="sxs-lookup"><span data-stu-id="7f058-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="7f058-326">Si aggiungerà quindi un attributo **for** all'etichetta per associarlo all'input e si vedranno suggerimenti IntelliSense basati sugli ID degli input nell'ambito.</span><span class="sxs-lookup"><span data-stu-id="7f058-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="7f058-327">Aprire **Visual Studio Express 2013 per il Web** e la soluzione **Begin. sln** disponibile nella cartella **source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** .</span><span class="sxs-lookup"><span data-stu-id="7f058-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="7f058-328">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="7f058-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="7f058-329">In **Esplora soluzioni**aprire il file **index. cshtml** che si trova nella cartella **views** | **Home** .</span><span class="sxs-lookup"><span data-stu-id="7f058-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="7f058-330">Aggiungere il formato seguente all'interno della **sezione&lt;&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="7f058-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="7f058-331">(Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *form*)</span><span class="sxs-lookup"><span data-stu-id="7f058-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="7f058-332">Il tag di input deve essere preceduto da un'etichetta con una descrizione del campo.</span><span class="sxs-lookup"><span data-stu-id="7f058-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="7f058-333">Aggiungere l'etichetta seguente prima del tag di input.</span><span class="sxs-lookup"><span data-stu-id="7f058-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="7f058-334">L'attributo **for** di un' **etichetta&lt;&gt;** specifica l'elemento del form a cui è associata un'etichetta.</span><span class="sxs-lookup"><span data-stu-id="7f058-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="7f058-335">Il valore dell'attributo deve essere uguale all'ID dell'elemento correlato.</span><span class="sxs-lookup"><span data-stu-id="7f058-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="7f058-336">Aggiungere l'attributo **for** all'elemento **Label&lt;&gt;** .</span><span class="sxs-lookup"><span data-stu-id="7f058-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="7f058-337">Come illustrato nella figura seguente, il valore del nome &quot;&quot; viene visualizzato nella casella IntelliSense, in base all'ID degli elementi all'interno dello stesso ambito (il **modulo di&lt;** di inclusione&gt;).</span><span class="sxs-lookup"><span data-stu-id="7f058-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="7f058-338">![Visualizzazione dell'ID in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Visualizzazione dell'ID in IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="7f058-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="7f058-339">*Visualizzazione dell'ID in IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="7f058-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="7f058-340">Eliminare il **form&lt;** aggiunto di recente&gt;elemento e il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="7f058-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="7f058-341">Attività 2-uso di frammenti di codice HTML</span><span class="sxs-lookup"><span data-stu-id="7f058-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="7f058-342">HTML5 ha introdotto più di 25 nuovi tag semantici.</span><span class="sxs-lookup"><span data-stu-id="7f058-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="7f058-343">In Visual Studio è già disponibile il supporto IntelliSense per questi tag, ma Visual Studio 2013 rende più veloce e semplice scrivere il markup aggiungendo nuovi frammenti di codice.</span><span class="sxs-lookup"><span data-stu-id="7f058-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="7f058-344">Sebbene questi tag non siano complicati, presentano alcune piccole sottigliezze, ad esempio l'aggiunta dei fallback di codec corretti per il tag *audio* .</span><span class="sxs-lookup"><span data-stu-id="7f058-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="7f058-345">In questa attività verranno visualizzati i frammenti di codice HTML per il tag audio.</span><span class="sxs-lookup"><span data-stu-id="7f058-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="7f058-346">Nel file **index. cshtml** Digitare **&lt;aud** nella **sezione&lt;&gt;** elemento, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="7f058-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="7f058-347">![Inserimento di un elemento audio](visual-studio-2013-web-tools/_static/image36.png "Inserimento di un elemento audio")</span><span class="sxs-lookup"><span data-stu-id="7f058-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="7f058-348">*Inserimento di un elemento audio*</span><span class="sxs-lookup"><span data-stu-id="7f058-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="7f058-349">Premere **Tab** due volte e notare come viene aggiunto il codice seguente nella pagina e il cursore viene posizionato sull'attributo **src** della prima origine.</span><span class="sxs-lookup"><span data-stu-id="7f058-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="7f058-350">Premendo il tasto **Tab** due volte, viene inserito il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="7f058-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="7f058-351">Il frammento audio Mostra l'utilizzo standard del tag *audio* , con due file di origine per un supporto migliorato.</span><span class="sxs-lookup"><span data-stu-id="7f058-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="7f058-352">Eliminare la seconda riga e aggiornare l'origine della prima riga con il collegamento seguente a WebCampsTV Katana Show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="7f058-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="7f058-353">Il codice risultante è riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7f058-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="7f058-354">Il file di origine *KatanaProject. mp3* viene usato come esempio.</span><span class="sxs-lookup"><span data-stu-id="7f058-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="7f058-355">Se si preferisce, è possibile usare un'altra origine.</span><span class="sxs-lookup"><span data-stu-id="7f058-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="7f058-356">Premere **CTRL** + **S** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7f058-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="7f058-357">Premere **CTRL** + **F5** per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7f058-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="7f058-358">Si noti che un lettore audio è stato aggiunto all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7f058-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="7f058-359">![Lettore audio in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Lettore audio in Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="7f058-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="7f058-360">*Lettore audio in Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="7f058-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="7f058-361">![Lettore audio in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Lettore audio in Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="7f058-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="7f058-362">*Lettore audio in Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="7f058-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="7f058-363">Non chiudere i browser.</span><span class="sxs-lookup"><span data-stu-id="7f058-363">Do not close the browsers.</span></span> <span data-ttu-id="7f058-364">Verranno utilizzati nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="7f058-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="7f058-365">Attività 3-uso di IntelliSense nei documenti JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f058-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="7f058-366">Con Web Essentials 2013, i fogli di stile e le pagine HTML producono un elenco di ID e nomi di classe.</span><span class="sxs-lookup"><span data-stu-id="7f058-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="7f058-367">In questa attività si apprenderà come questi elenchi migliorano il supporto IntelliSense per JavaScript in Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="7f058-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="7f058-368">Nel file **index. cshtml** aggiungere il codice seguente per definire un tag di **script** per il codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7f058-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="7f058-369">Aggiungere il codice seguente all'interno del tag **script** per definire la funzione di callback pronta.</span><span class="sxs-lookup"><span data-stu-id="7f058-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="7f058-370">(Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="7f058-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="7f058-371">Posizionare il cursore nel tag **script** e premere **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="7f058-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="7f058-372">per aprire il menu del suggerimento.</span><span class="sxs-lookup"><span data-stu-id="7f058-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="7f058-373">Fare clic su **Estrai nel file**.</span><span class="sxs-lookup"><span data-stu-id="7f058-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="7f058-374">![Suggerimento sull'estrazione da JavaScript a file](visual-studio-2013-web-tools/_static/image39.png "Suggerimento sull'estrazione da JavaScript a file")</span><span class="sxs-lookup"><span data-stu-id="7f058-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="7f058-375">*Suggerimento sull'estrazione da JavaScript a file*</span><span class="sxs-lookup"><span data-stu-id="7f058-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="7f058-376">Nella finestra **Salva con** nome selezionare la cartella **script** , denominare il file **init. js** e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="7f058-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="7f058-377">![Salva come finestra](visual-studio-2013-web-tools/_static/image40.png "Salva come finestra")</span><span class="sxs-lookup"><span data-stu-id="7f058-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="7f058-378">*Salva come finestra*</span><span class="sxs-lookup"><span data-stu-id="7f058-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-379">Il file **init. js** viene creato e il contenuto dello script viene spostato nel file.</span><span class="sxs-lookup"><span data-stu-id="7f058-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="7f058-380">![File init. js creato con il contenuto incluso](visual-studio-2013-web-tools/_static/image41.png "File init. js creato con il contenuto incluso")</span><span class="sxs-lookup"><span data-stu-id="7f058-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="7f058-381">*File init. js creato con il contenuto incluso*</span><span class="sxs-lookup"><span data-stu-id="7f058-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="7f058-382">Aprire il file **index. cshtml** e verificare che il tag script sia stato sostituito da un riferimento al file **init. js** .</span><span class="sxs-lookup"><span data-stu-id="7f058-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="7f058-383">![Riferimento HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Riferimento HTML init. js")</span><span class="sxs-lookup"><span data-stu-id="7f058-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="7f058-384">*Riferimento HTML init. js*</span><span class="sxs-lookup"><span data-stu-id="7f058-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="7f058-385">Passare alla **Esplora soluzioni** e notare che il file **init. js** è stato incluso automaticamente nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="7f058-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="7f058-386">![File init. js incluso nella soluzione](visual-studio-2013-web-tools/_static/image43.png "File init. js incluso nella soluzione")</span><span class="sxs-lookup"><span data-stu-id="7f058-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="7f058-387">*File init. js incluso nella soluzione*</span><span class="sxs-lookup"><span data-stu-id="7f058-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="7f058-388">Tornare al file **init. js** per aggiornare il callback della funzione **pronto** .</span><span class="sxs-lookup"><span data-stu-id="7f058-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="7f058-389">All'interno della definizione di callback della funzione passata a *Ready*, aggiungere il codice seguente per ottenere tutti gli elementi in base a un attributo di classe specifico.</span><span class="sxs-lookup"><span data-stu-id="7f058-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="7f058-390">Premere **CTRL** + **spazio** tra le virgolette all'interno della chiamata di funzione **getElementsByClassName** .</span><span class="sxs-lookup"><span data-stu-id="7f058-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="7f058-391">![Visualizzazione di IntelliSense per la funzione getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Visualizzazione di IntelliSense per la funzione getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="7f058-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="7f058-392">*Visualizzazione di IntelliSense per la funzione getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="7f058-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-393">Si noti che IntelliSense mostra le classi definite nei fogli di stile del progetto.</span><span class="sxs-lookup"><span data-stu-id="7f058-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="7f058-394">Sostituire la riga creata con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="7f058-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="7f058-395">Posizionare il cursore dopo **au** all'interno delle virgolette nella funzione **GetElementsByTagName** e premere **CTRL** + **spazio**.</span><span class="sxs-lookup"><span data-stu-id="7f058-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="7f058-396">![Visualizzazione di IntelliSense per il metodo getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Visualizzazione di IntelliSense per il metodo getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="7f058-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="7f058-397">*Visualizzazione di IntelliSense per il metodo getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="7f058-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="7f058-398">Selezionare **&quot;&quot;audio** dall'elenco e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="7f058-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="7f058-399">Il risultato è illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="7f058-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="7f058-400">![Recupero di elementi audio](visual-studio-2013-web-tools/_static/image46.png "Recupero di elementi audio")</span><span class="sxs-lookup"><span data-stu-id="7f058-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="7f058-401">*Recupero di elementi audio*</span><span class="sxs-lookup"><span data-stu-id="7f058-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="7f058-402">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul file **init. js** nella cartella **Scripts** e scegliere **minimizzare JavaScript file** dal menu **Web Essentials** .</span><span class="sxs-lookup"><span data-stu-id="7f058-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="7f058-403">![File minimizzare JavaScript](visual-studio-2013-web-tools/_static/image47.png "Minimizzare file JavaScript")</span><span class="sxs-lookup"><span data-stu-id="7f058-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="7f058-404">*File minimizzare JavaScript*</span><span class="sxs-lookup"><span data-stu-id="7f058-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="7f058-405">Quando viene richiesto di abilitare il minification automatico quando si modifica il file di origine, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="7f058-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="7f058-406">![Abilitazione dell'avviso minification automatico](visual-studio-2013-web-tools/_static/image48.png "Abilitazione dell'avviso minification automatico")</span><span class="sxs-lookup"><span data-stu-id="7f058-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="7f058-407">*Abilitazione dell'avviso minification automatico*</span><span class="sxs-lookup"><span data-stu-id="7f058-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7f058-408">**Init. min. js** viene creato e aggiunto come dipendenza del file **init. js** .</span><span class="sxs-lookup"><span data-stu-id="7f058-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="7f058-409">![Il file init. min. js è stato creato](visual-studio-2013-web-tools/_static/image49.png "Il file init. min. js è stato creato")</span><span class="sxs-lookup"><span data-stu-id="7f058-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="7f058-410">*Il file init. min. js è stato creato*</span><span class="sxs-lookup"><span data-stu-id="7f058-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="7f058-411">Aprire il file **init. min. js** e notare che il file è minimizzati.</span><span class="sxs-lookup"><span data-stu-id="7f058-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="7f058-412">![Contenuto del file init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenuto del file init. min. js")</span><span class="sxs-lookup"><span data-stu-id="7f058-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="7f058-413">*Contenuto del file init. min. js*</span><span class="sxs-lookup"><span data-stu-id="7f058-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="7f058-414">Nel file **init. js** aggiungere il codice seguente sotto la chiamata di funzione **GetElementsByTagName** per riprodurre tutti gli elementi audio.</span><span class="sxs-lookup"><span data-stu-id="7f058-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="7f058-415">(Frammento di codice- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="7f058-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="7f058-416">Fare clic su **CTRL** + **S** per salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7f058-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="7f058-417">Poiché il file minimizzati è già aperto, viene visualizzata una finestra di dialogo che informa che il file è stato modificato all'esterno dell'editor di origine.</span><span class="sxs-lookup"><span data-stu-id="7f058-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="7f058-418">Scegliere **Sì**.</span><span class="sxs-lookup"><span data-stu-id="7f058-418">Click **Yes**.</span></span>

    <span data-ttu-id="7f058-419">![Avviso Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Avviso Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="7f058-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="7f058-420">*Avviso Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="7f058-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="7f058-421">Tornare al file **init. min. js** per verificare che il file sia stato aggiornato con il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="7f058-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="7f058-422">![Il file init. min. js è stato aggiornato](visual-studio-2013-web-tools/_static/image52.png "Il file init. min. js è stato aggiornato")</span><span class="sxs-lookup"><span data-stu-id="7f058-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="7f058-423">*Il file init. min. js è stato aggiornato*</span><span class="sxs-lookup"><span data-stu-id="7f058-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="7f058-424">Fare clic sul pulsante **browser link Aggiorna** .</span><span class="sxs-lookup"><span data-stu-id="7f058-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="7f058-425">Una volta aggiornati entrambi i browser, i lettori audio visualizzati nell'attività precedente inizieranno a riprodursi automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7f058-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="7f058-426">![Lettore audio incluso nella visualizzazione](visual-studio-2013-web-tools/_static/image53.png "Lettore audio incluso nella visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="7f058-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="7f058-427">*Lettore audio incluso nella visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="7f058-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7f058-428">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7f058-428">Summary</span></span>

<span data-ttu-id="7f058-429">Completando questa esercitazione pratica si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="7f058-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="7f058-430">Usare le nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio frammenti di codice HTML5 avanzati e codifica Zen</span><span class="sxs-lookup"><span data-stu-id="7f058-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="7f058-431">Usare le nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione colori e la descrizione comando della matrice del browser</span><span class="sxs-lookup"><span data-stu-id="7f058-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="7f058-432">Usare le nuove funzionalità dell'editor JavaScript incluse in Web Essentials, ad esempio Extract to file e IntelliSense per tutti gli elementi HTML</span><span class="sxs-lookup"><span data-stu-id="7f058-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="7f058-433">Scambiare dati tra il browser e Visual Studio usando Browser Link</span><span class="sxs-lookup"><span data-stu-id="7f058-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
