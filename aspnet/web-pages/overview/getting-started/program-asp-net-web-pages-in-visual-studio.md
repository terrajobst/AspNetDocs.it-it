---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione Pagine Web ASP.NET (Razor) con Visual Studio | Microsoft Docs
author: Rick-Anderson
description: In questa appendice viene illustrato come è possibile utilizzare Visual Studio 2010 o Visual Web Developer 2010 Express per programmare Pagine Web ASP.NET con il sintassi Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633511"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="50865-103">Programmazione Pagine Web ASP.NET (Razor) con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50865-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="50865-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="50865-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="50865-105">Questo articolo illustra come è possibile usare Visual Studio o Visual Web Developer Express per i siti Web di programma Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="50865-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="50865-106">Contenuto dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="50865-106">What you'll learn</span></span>
>
> - <span data-ttu-id="50865-107">Cosa è necessario installare (se necessario) per lavorare con Pagine Web ASP.NET nella versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="50865-108">Come aggiungere il supporto per Pagine Web ASP.NET a Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="50865-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="50865-109">Come usare le funzionalità di Visual Studio per lavorare con le pagine Razor di ASP.NET, tra cui IntelliSense e il debugger.</span><span class="sxs-lookup"><span data-stu-id="50865-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="50865-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="50865-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="50865-111">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="50865-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="50865-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="50865-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="50865-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="50865-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="50865-114">Questa esercitazione funziona anche con Pagine Web ASP.NET 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="50865-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="50865-115">È possibile programmare le pagine Web di ASP.NET con sintassi Razor usando WebMatrix o molti altri editor di codice.</span><span class="sxs-lookup"><span data-stu-id="50865-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="50865-116">È anche possibile usare Microsoft Visual Studio che è un Integrated Development Environment completo (IDE) che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web).</span><span class="sxs-lookup"><span data-stu-id="50865-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="50865-117">Per lavorare con ASP.NET Razor Pages, è possibile usare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="50865-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="50865-118">Due funzionalità particolarmente utili fornite da Visual Studio per la programmazione con le pagine Web di ASP.NET Razor sono:</span><span class="sxs-lookup"><span data-stu-id="50865-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="50865-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="50865-119">*IntelliSense*.</span></span> <span data-ttu-id="50865-120">La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="50865-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="50865-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="50865-121">*Debugger*.</span></span> <span data-ttu-id="50865-122">Il debugger consente di risolvere i problemi del codice arrestando un programma mentre è in esecuzione, esaminando le variabili ed eseguendo il codice riga per riga.</span><span class="sxs-lookup"><span data-stu-id="50865-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="50865-123">Uso di Visual Studio con versioni diverse di Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="50865-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="50865-124">Per sviluppare app Web ASP.NET in Visual Studio 2017, installare il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="50865-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="50865-125">Visual Studio 2012 e Visual Studio 2013 includono il supporto per Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="50865-126">I pacchetti necessari per supportare Pagine Web ASP.NET vengono installati quando si installa Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="50865-127">Per impostazione predefinita, in Visual Studio 2010 non è incluso il supporto per Pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="50865-128">Per usare Pagine Web ASP.NET con Visual Studio 2010, è necessario installare il pacchetto MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="50865-129">Per ottenere Pagine Web ASP.NET 2, installare ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="50865-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="50865-130">Nella tabella seguente viene riepilogato il supporto per Pagine Web ASP.NET in versioni diverse di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="50865-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="50865-131">Visual Studio 2010</span></span> | <span data-ttu-id="50865-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="50865-132">Visual Studio 2012</span></span> | <span data-ttu-id="50865-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="50865-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="50865-134">**Pagine Web ASP.NET 2**</span><span class="sxs-lookup"><span data-stu-id="50865-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="50865-135">Installare ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="50865-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="50865-136">Incluso</span><span class="sxs-lookup"><span data-stu-id="50865-136">(Included)</span></span> | <span data-ttu-id="50865-137">Incluso</span><span class="sxs-lookup"><span data-stu-id="50865-137">(Included)</span></span> |
| <span data-ttu-id="50865-138">**Pagine Web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="50865-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="50865-139">Eseguire l'aggiornamento a Pagine Web ASP.NET 3 tramite NuGet</span><span class="sxs-lookup"><span data-stu-id="50865-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="50865-140">Incluso</span><span class="sxs-lookup"><span data-stu-id="50865-140">(Included)</span></span> |

<span data-ttu-id="50865-141">Per lavorare con Visual Studio 2010, vedere [installazione del supporto per pagine Web ASP.NET in Visual studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="50865-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="50865-142">Avvio di Visual Studio da WebMatrix</span><span class="sxs-lookup"><span data-stu-id="50865-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="50865-143">Se è stato avviato un progetto in WebMatrix e si vuole passare a Visual Studio, WebMatrix fornisce un pulsante per aprire facilmente il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="50865-144">Per abilitare questo pulsante è necessario che Visual Studio sia installato nel computer.</span><span class="sxs-lookup"><span data-stu-id="50865-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="50865-145">La figura seguente mostra il pulsante in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="50865-145">The following image shows the button in WebMatrix.</span></span>

![avvia Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="50865-147">Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="50865-148">È possibile passare da WebMatrix a Visual Studio e viceversa senza problemi.</span><span class="sxs-lookup"><span data-stu-id="50865-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="50865-149">Si riceverà una notifica se i file sono stati modificati nell'altro ambiente ed è necessario ricaricarli per ottenere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="50865-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="50865-150">Creazione del sito Razor di ASP.NET in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50865-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="50865-151">Per creare un sito Web ASP.NET Razor in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="50865-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="50865-152">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="50865-153">Scegliere **nuovo sito Web**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="50865-153">In the **File** menu, click **New Web Site**.</span></span>

    ![Crea nuovo sito Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="50865-155">Nella finestra di dialogo **nuovo sito Web** selezionare la lingua da utilizzare (Visual C# o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="50865-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="50865-156">Selezionare il modello di **sito Web ASP.NET (Razor)** .</span><span class="sxs-lookup"><span data-stu-id="50865-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sito Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="50865-158">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="50865-158">Click **OK**.</span></span>

<span data-ttu-id="50865-159">Il nuovo progetto esiste e viene popolato con alcune pagine Web predefinite che consentono di iniziare.</span><span class="sxs-lookup"><span data-stu-id="50865-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="50865-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="50865-160">Using IntelliSense</span></span>

<span data-ttu-id="50865-161">Ora che è stato creato un sito, è possibile vedere come funziona IntelliSense in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="50865-162">Nel sito Web appena creato aprire la pagina *default. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="50865-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="50865-163">Dopo i tag `<h3>` nella pagina digitare `@ServerInfo.` (incluso il punto).</span><span class="sxs-lookup"><span data-stu-id="50865-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="50865-164">Si noti che IntelliSense Visualizza i metodi disponibili per l'helper `ServerInfo` in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="50865-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="50865-166">Selezionare il metodo `GetHtml` dall'elenco e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="50865-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="50865-167">IntelliSense compila automaticamente il metodo.</span><span class="sxs-lookup"><span data-stu-id="50865-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="50865-168">Come con qualsiasi metodo in C#, è necessario aggiungere `()` caratteri dopo il metodo. Il codice completo per il metodo `GetHtml` è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="50865-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="50865-169">Premere CTRL + F5 per eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="50865-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="50865-170">Questo è l'aspetto della pagina quando viene visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="50865-170">This is what the page looks like when displayed in a browser:</span></span>

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="50865-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="50865-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="50865-173">Uso del debugger</span><span class="sxs-lookup"><span data-stu-id="50865-173">Using the Debugger</span></span>

1. <span data-ttu-id="50865-174">Nella parte superiore della pagina *default. cshtml* , dopo la riga che inizia con `Page.Title`, aggiungere la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="50865-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="50865-175">Nel margine grigio dell'editor a sinistra del codice, fare clic su Avanti accanto alla nuova riga per aggiungere un punto di *interruzione*.</span><span class="sxs-lookup"><span data-stu-id="50865-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="50865-176">Un punto di interruzione è un indicatore che indica al debugger di arrestare l'esecuzione del programma in quel momento, in modo da poter vedere cosa accade.</span><span class="sxs-lookup"><span data-stu-id="50865-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Imposta punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="50865-178">Rimuovere la chiamata al metodo `ServerInfo.GetHtml` e aggiungere una chiamata alla variabile `@myTime` al suo posto.</span><span class="sxs-lookup"><span data-stu-id="50865-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="50865-179">Questa chiamata consente di visualizzare il valore dell'ora corrente restituito dalla nuova riga di codice.</span><span class="sxs-lookup"><span data-stu-id="50865-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="50865-180">Premere F5 per eseguire la pagina nel debugger.</span><span class="sxs-lookup"><span data-stu-id="50865-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="50865-181">La pagina viene arrestata in corrispondenza del punto di interruzione impostato.</span><span class="sxs-lookup"><span data-stu-id="50865-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="50865-182">La figura seguente mostra l'aspetto della pagina nell'editor con il punto di interruzione (in giallo).</span><span class="sxs-lookup"><span data-stu-id="50865-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![punto di interruzione debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="50865-184">Sulla barra degli strumenti Debug fare clic sul pulsante Esegui **istruzione** (o premere F11) per eseguire la riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="50865-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="50865-185">Ogni volta che si fa clic su questo pulsante, l'esecuzione viene spostata alla riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="50865-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Pulsante Esegui istruzione](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="50865-187">Esaminare il valore della variabile `myTime` tenendo premuto il puntatore del mouse o controllando i valori visualizzati nelle finestre **variabili locali** e **stack di chiamate** .</span><span class="sxs-lookup"><span data-stu-id="50865-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="50865-188">Visual Studio Visualizza il valore della variabile.</span><span class="sxs-lookup"><span data-stu-id="50865-188">Visual Studio display the value of the variable.</span></span>

    ![Mostra valore ora](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="50865-190">Quando si è terminato di esaminare la variabile ed eseguire il codice istruzione per istruzione, premere F5 per continuare a eseguire la pagina senza fermarsi a ogni riga.</span><span class="sxs-lookup"><span data-stu-id="50865-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="50865-191">Al termine dell'esecuzione del codice, il browser Visualizza la pagina.</span><span class="sxs-lookup"><span data-stu-id="50865-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="50865-192">Per ulteriori informazioni sul debugger e su come eseguire il debug di codice in Visual Studio, vedere [procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="50865-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="50865-193">Uso di Razor nei progetti MVC ASP.NET con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50865-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="50865-194">Il sintassi Razor viene inoltre ampiamente utilizzato nei progetti MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="50865-195">MVC è un metodo potente basato su modelli per la creazione di siti Web dinamici.</span><span class="sxs-lookup"><span data-stu-id="50865-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="50865-196">Se il sito di Pagine Web ASP.NET diventa difficile da gestire, è consigliabile provare a convertirlo in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="50865-197">Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="50865-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="50865-198">Installazione del supporto per Pagine Web ASP.NET in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="50865-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="50865-199">In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di Pagine Web ASP.NET per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50865-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="50865-200">Se non si dispone ancora dell'installazione guidata piattaforma Web, scaricarla dall'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="50865-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="50865-201">Eseguire l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="50865-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="50865-202">Fare clic sulla scheda **prodotti** .</span><span class="sxs-lookup"><span data-stu-id="50865-202">Click the **Products** tab.</span></span>

    ![Scheda prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="50865-204">Cercare **ASP.NET MVC 4** (per pagine Web ASP.NET 2) e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="50865-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="50865-205">Questi prodotti includono Visual Studio Tools per la creazione di siti Web Razor ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50865-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="50865-207">Fare clic su **Installa** per completare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="50865-207">Click **Install** to complete the installation.</span></span>
