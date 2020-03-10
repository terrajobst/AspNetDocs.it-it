---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creare un nuovo progetto MVC ASP.NET | Microsoft Docs
author: microsoft
description: Il passaggio 1 Mostra come posizionare la struttura dell'applicazione NerdDinner di base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580934"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="08f34-103">Creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="08f34-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="08f34-104">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="08f34-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="08f34-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="08f34-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="08f34-106">Questo è il passaggio 1 di un' [applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuita che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="08f34-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="08f34-107">Il passaggio 1 Mostra come posizionare la struttura dell'applicazione NerdDinner di base.</span><span class="sxs-lookup"><span data-stu-id="08f34-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="08f34-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="08f34-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="08f34-109">NerdDinner passaggio 1: file-&gt;nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="08f34-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="08f34-110">Si inizierà l'applicazione NerdDinner selezionando la voce di menu **nuovo progetto&gt;file** all'interno di visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="08f34-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="08f34-111">Verrà visualizzata la finestra di dialogo "nuovo progetto".</span><span class="sxs-lookup"><span data-stu-id="08f34-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="08f34-112">Per creare una nuova applicazione MVC ASP.NET, selezionare il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "applicazione Web MVC ASP.NET" a destra:</span><span class="sxs-lookup"><span data-stu-id="08f34-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="08f34-113">*Importante: assicurarsi di aver scaricato e installato ASP.NET MVC. in caso contrario, non verrà visualizzato nella finestra di dialogo nuovo progetto. È possibile usare la versione V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se non è ancora stata installata (ASP.NET MVC è disponibile all'interno della sezione "Web Platform-&gt;Frameworks and Runtimes").*</span><span class="sxs-lookup"><span data-stu-id="08f34-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="08f34-114">Il nuovo progetto verrà creato con il nome "NerdDinner" e quindi fare clic sul pulsante "OK" per crearlo.</span><span class="sxs-lookup"><span data-stu-id="08f34-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="08f34-115">Quando si fa clic su "OK", Visual Studio Visualizza una finestra di dialogo aggiuntiva che richiede di creare facoltativamente un progetto di unit test anche per la nuova applicazione.</span><span class="sxs-lookup"><span data-stu-id="08f34-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="08f34-116">Questo progetto unit test consente di creare test automatizzati che verificano le funzionalità e il comportamento dell'applicazione (cosa verrà illustrato più avanti in questa esercitazione).</span><span class="sxs-lookup"><span data-stu-id="08f34-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="08f34-117">L'elenco a discesa "Framework di test" nella finestra di dialogo precedente viene popolato con tutti i modelli di progetto unit test MVC ASP.NET installati nel computer.</span><span class="sxs-lookup"><span data-stu-id="08f34-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="08f34-118">È possibile scaricare le versioni per NUnit, MBUnit e XUnit.</span><span class="sxs-lookup"><span data-stu-id="08f34-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="08f34-119">È supportato anche il Framework di unit test predefinito di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08f34-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="08f34-120">*Nota: il Framework per unit test di Visual Studio è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si utilizza VS 2008 Standard Edition o Visual Web Developer 2008 Express, sarà necessario scaricare e installare NUnit, MBUnit o XUnit Extensions for ASP.NET MVC per visualizzare la finestra di dialogo. Se non è installato alcun framework di test, la finestra di dialogo non viene visualizzata.*</span><span class="sxs-lookup"><span data-stu-id="08f34-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="08f34-121">Verrà usato il nome predefinito "NerdDinner. tests" per il progetto di test creato e verrà usata l'opzione del Framework "unit test di Visual Studio".</span><span class="sxs-lookup"><span data-stu-id="08f34-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="08f34-122">Quando si fa clic sul pulsante "OK", Visual Studio creerà una soluzione per Microsoft con due progetti, uno per l'applicazione Web e uno per gli unit test:</span><span class="sxs-lookup"><span data-stu-id="08f34-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="08f34-123">Esame della struttura di directory NerdDinner</span><span class="sxs-lookup"><span data-stu-id="08f34-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="08f34-124">Quando si crea una nuova applicazione MVC ASP.NET con Visual Studio, vengono aggiunti automaticamente un numero di file e directory al progetto:</span><span class="sxs-lookup"><span data-stu-id="08f34-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="08f34-125">Per impostazione predefinita, i progetti MVC ASP.NET hanno sei directory di primo livello:</span><span class="sxs-lookup"><span data-stu-id="08f34-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="08f34-126">**Directory**</span><span class="sxs-lookup"><span data-stu-id="08f34-126">**Directory**</span></span> | <span data-ttu-id="08f34-127">**Scopo**</span><span class="sxs-lookup"><span data-stu-id="08f34-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="08f34-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="08f34-128">**/Controllers**</span></span> | <span data-ttu-id="08f34-129">Dove si inseriscono le classi controller che gestiscono le richieste URL</span><span class="sxs-lookup"><span data-stu-id="08f34-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="08f34-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="08f34-130">**/Models**</span></span> | <span data-ttu-id="08f34-131">Posizione in cui inserire le classi che rappresentano e modificano i dati</span><span class="sxs-lookup"><span data-stu-id="08f34-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="08f34-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="08f34-132">**/Views**</span></span> | <span data-ttu-id="08f34-133">Posizione in cui inserire i file di modello dell'interfaccia utente responsabili del rendering dell'output</span><span class="sxs-lookup"><span data-stu-id="08f34-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="08f34-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="08f34-134">**/Scripts**</span></span> | <span data-ttu-id="08f34-135">Dove si inseriscono script e file di libreria JavaScript (. js)</span><span class="sxs-lookup"><span data-stu-id="08f34-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="08f34-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="08f34-136">**/Content**</span></span> | <span data-ttu-id="08f34-137">Posizione in cui inserire i file di immagine e CSS e altri contenuti non dinamici/non JavaScript</span><span class="sxs-lookup"><span data-stu-id="08f34-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="08f34-138">**/App\_dati**</span><span class="sxs-lookup"><span data-stu-id="08f34-138">**/App\_Data**</span></span> | <span data-ttu-id="08f34-139">Dove vengono archiviati i file di dati che si desidera leggere/scrivere.</span><span class="sxs-lookup"><span data-stu-id="08f34-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="08f34-140">ASP.NET MVC non richiede questa struttura.</span><span class="sxs-lookup"><span data-stu-id="08f34-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="08f34-141">Infatti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere partizionano l'applicazione in più progetti per renderla più gestibile (ad esempio, le classi del modello di dati vengono spesso inserite in un progetto di libreria di classi separato dall'applicazione Web).</span><span class="sxs-lookup"><span data-stu-id="08f34-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="08f34-142">La struttura di progetto predefinita, tuttavia, fornisce una valida convenzione di directory predefinita che è possibile utilizzare per garantire la pulizia dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08f34-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="08f34-143">Quando si espande la directory/Controllers si noterà che Visual Studio ha aggiunto due classi controller, HomeController e AccountController, per impostazione predefinita al progetto:</span><span class="sxs-lookup"><span data-stu-id="08f34-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="08f34-144">Quando si espande la directory/views, sono state aggiunte al progetto anche tre sottodirectory, ovvero/Home,/account e/documenti condivisi, oltre a diversi file di modello al suo interno, per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="08f34-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="08f34-145">Quando si espandono le directory/content e/scripts, si troverà un file site. CSS usato per applicare lo stile a tutto il codice HTML nel sito, nonché le librerie JavaScript che possono abilitare il supporto per ASP.NET AJAX e jQuery all'interno dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="08f34-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="08f34-146">Quando si espande il progetto NerdDinner. tests, sono disponibili due classi che contengono unit test per le classi controller:</span><span class="sxs-lookup"><span data-stu-id="08f34-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="08f34-147">Questi file predefiniti aggiunti da Visual Studio forniscono una struttura di base per un'applicazione funzionante, completa con home page, informazioni sulla pagina, le pagine di accesso/disconnessione/registrazione degli account e una pagina di errore non gestita (tutti cablati e funzionanti).</span><span class="sxs-lookup"><span data-stu-id="08f34-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="08f34-148">Esecuzione dell'applicazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="08f34-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="08f34-149">È possibile eseguire il progetto scegliendo Debug **-&gt;Avvia debug** o **debug-&gt;avvia senza eseguire debug** voci di menu:</span><span class="sxs-lookup"><span data-stu-id="08f34-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="08f34-150">Verrà avviato il server Web ASP.NET incorporato incluso in Visual Studio ed eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="08f34-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="08f34-151">Di seguito sono riportati i home page per il nuovo progetto (URL: "/") quando viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="08f34-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="08f34-152">Facendo clic sulla scheda "about" viene visualizzata una pagina About (URL: "/Home/About"):</span><span class="sxs-lookup"><span data-stu-id="08f34-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="08f34-153">Se si fa clic sul collegamento "Accedi" nella parte superiore destra, viene visitata una pagina di accesso (URL: "/Account/LogOn")</span><span class="sxs-lookup"><span data-stu-id="08f34-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="08f34-154">Se non si dispone di un account di accesso, è possibile fare clic sul collegamento Register (URL: "/Account/Register") per crearne uno:</span><span class="sxs-lookup"><span data-stu-id="08f34-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="08f34-155">Il codice per implementare la funzionalità Home, about e logout/Register precedente è stato aggiunto per impostazione predefinita al momento della creazione del nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="08f34-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="08f34-156">Verrà usato come punto di partenza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08f34-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="08f34-157">Test dell'applicazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="08f34-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="08f34-158">Se si usa la versione Professional Edition o una versione successiva di Visual Studio 2008, è possibile usare il supporto integrato dell'IDE di testing unità in Visual Studio per testare il progetto:</span><span class="sxs-lookup"><span data-stu-id="08f34-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="08f34-159">Se si sceglie una delle opzioni precedenti, verrà aperto il riquadro "Risultati test" all'interno dell'IDE e verrà fornito lo stato superato/non superato nei 27 unit test inclusi nel nuovo progetto che coprono la funzionalità incorporata:</span><span class="sxs-lookup"><span data-stu-id="08f34-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="08f34-160">Più avanti in questa esercitazione parleremo di test automatizzati e aggiungono unit test aggiuntivi che coprono le funzionalità dell'applicazione implementate.</span><span class="sxs-lookup"><span data-stu-id="08f34-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="08f34-161">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="08f34-161">Next Step</span></span>

<span data-ttu-id="08f34-162">A questo punto è disponibile una struttura di base dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08f34-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="08f34-163">Verrà ora [creato un database per archiviare i dati dell'applicazione](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="08f34-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="08f34-164">[Precedente](introducing-the-nerddinner-tutorial.md)
> [Successivo](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="08f34-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
