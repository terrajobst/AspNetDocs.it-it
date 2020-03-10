---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Uso di Controllo pagina in ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538017"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="4398c-104">Utilizzo di Controllo pagina in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4398c-104">Using Page Inspector in ASP.NET MVC</span></span>

<span data-ttu-id="4398c-105">di Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="4398c-105">by Tim Ammann</span></span>

> <span data-ttu-id="4398c-106">Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="4398c-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="4398c-107">Selezionare qualsiasi elemento nel browser integrato e Controllo pagina evidenzia immediatamente l'origine e il CSS dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="4398c-108">È possibile esplorare qualsiasi visualizzazione MVC, trovare rapidamente le origini del markup di cui è stato eseguito il rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4398c-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="4398c-109">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="4398c-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="4398c-110">Questa esercitazione illustra come abilitare modalità Controllo e quindi individuare e modificare rapidamente markup e CSS nel progetto Web.</span><span class="sxs-lookup"><span data-stu-id="4398c-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="4398c-111">Nell'esercitazione viene usato un progetto MVC, ma è anche possibile usare Controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4398c-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="4398c-112">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4398c-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="4398c-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4398c-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="4398c-114">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="4398c-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="4398c-115">Usare Controllo pagina per passare a una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="4398c-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="4398c-116">Abilita modalità Controllo</span><span class="sxs-lookup"><span data-stu-id="4398c-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="4398c-117">Usare Controllo pagina per apportare modifiche al markup</span><span class="sxs-lookup"><span data-stu-id="4398c-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="4398c-118">modalità Controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="4398c-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="4398c-119">Anteprima delle modifiche CSS nella finestra Stili</span><span class="sxs-lookup"><span data-stu-id="4398c-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="4398c-120">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="4398c-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="4398c-121">Uso della selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="4398c-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="4398c-122">Mapping di elementi pagina dinamici a JavaScript</span><span class="sxs-lookup"><span data-stu-id="4398c-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="4398c-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4398c-123">Prerequisites</span></span>

- <span data-ttu-id="4398c-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="4398c-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="4398c-125">Per ottenere la versione più recente di Controllo pagina, utilizzare l' [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="4398c-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="4398c-126">Controllo pagina viene fornito con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="4398c-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="4398c-127">La versione più recente è la 1,3.</span><span class="sxs-lookup"><span data-stu-id="4398c-127">The latest version is 1.3.</span></span> <span data-ttu-id="4398c-128">Per verificare quale versione è disponibile, eseguire Visual Studio e selezionare **informazioni Microsoft Visual Studio** **dal menu?** .</span><span class="sxs-lookup"><span data-stu-id="4398c-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="4398c-129">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="4398c-129">Create a Web Application</span></span>

<span data-ttu-id="4398c-130">Per prima cosa, creare un'applicazione Web che si userà Controllo pagina con.</span><span class="sxs-lookup"><span data-stu-id="4398c-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="4398c-131">In Visual Studio scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4398c-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="4398c-132">A sinistra espandere **C#Visual**, selezionare **Web**e quindi selezionare **ASP.NET MVC4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="4398c-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="4398c-134">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="4398c-134">Click **OK**.</span></span>

<span data-ttu-id="4398c-135">Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="4398c-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="4398c-136">Lasciare **Razor** come motore di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="4398c-136">Leave **Razor** as the default view engine.</span></span>

![Nuovo progetto MVC ASP.NET-applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="4398c-138">L'applicazione verrà aperta nella visualizzazione **origine** .</span><span class="sxs-lookup"><span data-stu-id="4398c-138">The application opens in **Source** view.</span></span>

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="4398c-140">Ora che si dispone di un'applicazione da usare, è possibile usare Controllo pagina per esaminarla e modificarla.</span><span class="sxs-lookup"><span data-stu-id="4398c-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="4398c-141">Usare Controllo pagina per passare a una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="4398c-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="4398c-142">In Visual Studio 2012 è possibile fare clic con il pulsante destro del mouse su qualsiasi visualizzazione nel progetto, selezionare **Visualizza in controllo pagina**e controllo pagina rileverà la route e visualizzerà la pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="4398c-143">In **Esplora soluzioni**espandere la cartella **visualizzazioni** e quindi la cartella **Home** .</span><span class="sxs-lookup"><span data-stu-id="4398c-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="4398c-144">Fare clic con il pulsante destro del mouse sul file index. cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="4398c-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Visualizzare index. cshtml in Controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="4398c-146">Per impostazione predefinita, Controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4398c-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="4398c-147">Se si preferisce, è possibile ancorarlo altrove oppure disancorare la finestra.</span><span class="sxs-lookup"><span data-stu-id="4398c-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="4398c-148">Vedere [procedura: disporre e ancorare le finestre](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="4398c-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="4398c-149">Il riquadro superiore della finestra Controllo pagina mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="4398c-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="4398c-150">Il riquadro inferiore mostra la pagina nel markup HTML, insieme ad alcune schede che consentono di controllare diversi aspetti della pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="4398c-151">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="4398c-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Applicazione MVC ASP.NET in Controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="4398c-153">In questa esercitazione si useranno le schede **HTML** e **stili** per spostarsi rapidamente e apportare modifiche all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4398c-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="4398c-154">Modalità EnableInspection</span><span class="sxs-lookup"><span data-stu-id="4398c-154">EnableInspection Mode</span></span>

<span data-ttu-id="4398c-155">Per inserire Controllo pagina modalità Controllo, fare clic sul pulsante **Controlla** .</span><span class="sxs-lookup"><span data-stu-id="4398c-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="4398c-156">In modalità Controllo, quando si posiziona il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito il rendering, viene evidenziato il codice o il markup di origine corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4398c-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Imposta/Nascondi modalità Controllo](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="4398c-158">Ora spostare il puntatore del mouse su diverse parti della pagina all'interno Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="4398c-159">Come si fa, il puntatore del mouse assume la forma di un segno più grande e l'elemento sottostante è evidenziato:</span><span class="sxs-lookup"><span data-stu-id="4398c-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="4398c-161">Quando si sposta il puntatore del mouse, Visual Studio evidenzia i sintassi Razor corrispondenti nel file di origine.</span><span class="sxs-lookup"><span data-stu-id="4398c-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="4398c-162">Se l'elemento HTML deriva da un altro file di origine, Visual Studio apre automaticamente il file.</span><span class="sxs-lookup"><span data-stu-id="4398c-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="4398c-163">In Controllo pagina, nella scheda **HTML** viene visualizzato il codice HTML generato dall'sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="4398c-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="4398c-164">Quando si sposta il puntatore del mouse, gli elementi HTML vengono evidenziati.</span><span class="sxs-lookup"><span data-stu-id="4398c-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="4398c-165">Nella scheda **stili** vengono visualizzate le regole CSS per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="4398c-166">Usare Controllo pagina per apportare modifiche al markup</span><span class="sxs-lookup"><span data-stu-id="4398c-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="4398c-167">Controllo pagina consente di trovare il markup il cui percorso potrebbe non essere ovvio.</span><span class="sxs-lookup"><span data-stu-id="4398c-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="4398c-168">È quindi possibile modificare il markup e visualizzare le modifiche risultanti.</span><span class="sxs-lookup"><span data-stu-id="4398c-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="4398c-169">Per verificarlo, fare clic su **Controlla** , quindi scorrere fino alla fine della pagina nella finestra controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="4398c-170">Quando si sposta il puntatore del mouse nell'area del piè di pagina, Controllo pagina apre il file \_layout. cshtml ed evidenzia la sezione della pagina di layout selezionata.</span><span class="sxs-lookup"><span data-stu-id="4398c-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="4398c-171">Come si può notare, il piè di pagina è definito nel file di layout e non nella vista stessa.</span><span class="sxs-lookup"><span data-stu-id="4398c-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="4398c-173">Spostare il puntatore del mouse sulla riga con la nota sul <a id="a"> </a>copyright.</span><span class="sxs-lookup"><span data-stu-id="4398c-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="4398c-174">Nella pagina \_layout. cshtml la riga corrispondente è evidenziata.</span><span class="sxs-lookup"><span data-stu-id="4398c-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Linea di copyright del piè di pagina evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="4398c-176">Aggiungere testo alla fine della riga nel file \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="4398c-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="4398c-177">&lt;p&gt;copia &amp;; @DateTime.Now.Year-My ASP.NET MVC Application Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="4398c-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="4398c-178">A questo punto, premere CTRL + ALT + INVIO oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="4398c-180">Il piè di pagina è stato definito in index. cshtml, ma si è rivelato che si trovava nel \_layout. cshtml ed è stato individuato da Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="4398c-181">modalità Controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="4398c-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="4398c-182">Successivamente, sarà possibile esaminare rapidamente la finestra HTML e il modo in cui viene eseguito il mapping degli elementi.</span><span class="sxs-lookup"><span data-stu-id="4398c-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="4398c-183">Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="4398c-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="4398c-184">Fare clic sulla parte superiore della pagina che indica "il logo qui".</span><span class="sxs-lookup"><span data-stu-id="4398c-184">Click the top part of the page that says "Your logo here".</span></span> <span data-ttu-id="4398c-185">Si sta esaminando un particolare elemento in modo più dettagliato, quindi la visualizzazione nella finestra del browser non cambia più quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="4398c-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="4398c-186">Spostare quindi il puntatore del mouse nella finestra **HTML** .</span><span class="sxs-lookup"><span data-stu-id="4398c-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="4398c-187">Quando si sposta il puntatore del mouse, Controllo pagina delinea l'elemento all'interno della finestra **HTML** ed evidenzia l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="4398c-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="4398c-189">Come in precedenza, Controllo pagina apre il file layout. cshtml di \_in una scheda temporanea. fare clic sulla scheda \_layout. cshtml Temporary e il markup corrispondente verrà evidenziato nella sezione&gt; intestazione &lt;:</span><span class="sxs-lookup"><span data-stu-id="4398c-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="4398c-191">Anteprima delle modifiche CSS nella finestra Stili</span><span class="sxs-lookup"><span data-stu-id="4398c-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="4398c-192">Si userà quindi la finestra **stili** controllo pagina per visualizzare in anteprima le modifiche apportate a CSS.</span><span class="sxs-lookup"><span data-stu-id="4398c-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="4398c-193">Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="4398c-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="4398c-194">Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="4398c-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="4398c-196">Fare clic una volta all'interno della sezione div. Content-wrapper, quindi spostare il puntatore del mouse nella finestra **stili** .</span><span class="sxs-lookup"><span data-stu-id="4398c-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="4398c-197">Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="4398c-198">Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="4398c-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="4398c-199">A questo punto deselezionare la casella di controllo per la proprietà colore sfondo.</span><span class="sxs-lookup"><span data-stu-id="4398c-199">Now clear the checkbox for the background-color property.</span></span>

![Cancella colore di sfondo](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="4398c-201">Si noti il modo in cui le modifiche vengono visualizzate immediatamente nella finestra del browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="4398c-202">Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarlo in rosso.</span><span class="sxs-lookup"><span data-stu-id="4398c-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="4398c-203">La modifica viene visualizzata immediatamente:</span><span class="sxs-lookup"><span data-stu-id="4398c-203">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="4398c-205">La finestra **stili** rende più semplice testare e visualizzare in anteprima le modifiche CSS prima di eseguire il commit delle modifiche nel foglio di stile.</span><span class="sxs-lookup"><span data-stu-id="4398c-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="4398c-206">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="4398c-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="4398c-207">Questa funzionalità richiede la versione 1,3 di Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-207">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="4398c-208">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e di visualizzare immediatamente le modifiche nel browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="4398c-209">Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="4398c-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="4398c-210">Nel browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="4398c-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="4398c-211">Fare clic una volta per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-211">Click once to select this element.</span></span>

<span data-ttu-id="4398c-212">Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="4398c-213">Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="4398c-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="4398c-214">Fare clic su ". optional. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="4398c-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="4398c-215">Controllo pagina apre il file CSS che definisce lo stile (site. CSS) ed evidenzia lo stile CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4398c-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="4398c-216">Modificare ora il valore di `background-color` in "Red".</span><span class="sxs-lookup"><span data-stu-id="4398c-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="4398c-217">La modifica viene visualizzata immediatamente nel browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="4398c-218">Uso della selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="4398c-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="4398c-219">L'editor CSS in Visual Studio 2012 dispone di una selezione colori che semplifica la scelta e l'inserimento dei colori.</span><span class="sxs-lookup"><span data-stu-id="4398c-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="4398c-220">La selezione colori include una tavolozza di colori standard, supporta i nomi di colore standard, i codici hash, i colori RGB, RGBA, HSL e HSLA e mantiene un elenco dei colori usati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="4398c-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="4398c-221">Nella sezione precedente è stato modificato il valore della proprietà `background-color`.</span><span class="sxs-lookup"><span data-stu-id="4398c-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="4398c-222">Per richiamare la selezione colori, posizionare il punto di inserimento dopo il nome della proprietà e digitare **#** o **RGB (** .</span><span class="sxs-lookup"><span data-stu-id="4398c-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Barra di selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="4398c-224">Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi usare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="4398c-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="4398c-225">Quando si visita un colore, il valore esadecimale corrispondente viene visualizzato in anteprima:</span><span class="sxs-lookup"><span data-stu-id="4398c-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valore della proprietà del colore di sfondo in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="4398c-227">Se la barra dei colori non ha il colore esatto desiderato, è possibile usare il popup della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="4398c-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="4398c-228">Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori oppure premere la freccia giù una o due volte sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="4398c-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Popup selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="4398c-230">Fare clic su un colore dalla barra verticale a destra.</span><span class="sxs-lookup"><span data-stu-id="4398c-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="4398c-231">Viene visualizzata una sfumatura per quel colore nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="4398c-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="4398c-232">Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su un punto qualsiasi nella finestra principale per scegliere con maggiore precisione.</span><span class="sxs-lookup"><span data-stu-id="4398c-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="4398c-233">Se nella schermata del computer è presente un colore che si desidera utilizzare (non è necessario che sia all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando lo strumento contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="4398c-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="4398c-234">È anche possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="4398c-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="4398c-235">In questo modo, i valori dei colori vengono modificati in valori RGBA, in quanto il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="4398c-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="4398c-236">Dopo aver scelto un colore, premere INVIO, quindi digitare un punto e virgola per completare la voce relativa al colore di sfondo nel file *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="4398c-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="4398c-237">La barra di aggiornamento Controllo pagina</span><span class="sxs-lookup"><span data-stu-id="4398c-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="4398c-238">Controllo pagina rileva immediatamente la modifica apportata al file *site. CSS* e visualizza un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="4398c-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="4398c-240">Per salvare tutti i file e aggiornare il browser Controllo pagina, premere CTRL + ALT + INVIO o fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="4398c-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="4398c-241">La modifica apportata al colore di evidenziazione viene visualizzata nel browser.</span><span class="sxs-lookup"><span data-stu-id="4398c-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="4398c-242">Mapping di elementi pagina dinamici a JavaScript</span><span class="sxs-lookup"><span data-stu-id="4398c-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="4398c-243">Nelle applicazioni Web moderne gli elementi della pagina vengono spesso generati dinamicamente con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4398c-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="4398c-244">Ciò significa che non esiste alcun markup statico (HTML o Razor) che corrisponde a questi elementi della pagina.</span><span class="sxs-lookup"><span data-stu-id="4398c-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="4398c-245">Con la versione 1,3, Controllo pagina ora possibile eseguire il mapping degli elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4398c-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="4398c-246">Per illustrare questa funzionalità, verrà usato il [modello applicazione a pagina singola (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="4398c-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4398c-247">Il modello di SPA richiede l'aggiornamento di [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .</span><span class="sxs-lookup"><span data-stu-id="4398c-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>

<span data-ttu-id="4398c-248">In Visual Studio scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4398c-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="4398c-249">A sinistra espandere **C#Visual**, selezionare **Web**e quindi selezionare **ASP.NET MVC4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="4398c-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="4398c-250">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="4398c-250">Click **OK**.</span></span>

<span data-ttu-id="4398c-251">Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **applicazione a pagina singola**.</span><span class="sxs-lookup"><span data-stu-id="4398c-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="4398c-252">In Esplora soluzioni espandere la cartella **visualizzazioni** e quindi la cartella **Home** .</span><span class="sxs-lookup"><span data-stu-id="4398c-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="4398c-253">Fare clic con il pulsante destro del mouse sul file index. cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="4398c-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="4398c-254">La prima cosa visualizzata in Controllo pagina browser è una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="4398c-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="4398c-255">Fare clic su "Iscriviti" e creare un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="4398c-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="4398c-256">Una volta eseguita l'iscrizione, l'applicazione esegue l'accesso e crea un elenco attività con alcuni elementi di esempio.</span><span class="sxs-lookup"><span data-stu-id="4398c-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="4398c-257">Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="4398c-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="4398c-258">Nel browser Controllo pagina fare clic su uno degli elementi attività.</span><span class="sxs-lookup"><span data-stu-id="4398c-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="4398c-259">Si noti che anziché essere evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="4398c-260">Indica che l'elemento è stato creato in modo dinamico tramite script.</span><span class="sxs-lookup"><span data-stu-id="4398c-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="4398c-261">Viene inoltre visualizzata una sottolineatura arancione nella scheda **stack di chiamate** . Indica che nel riquadro **stack di chiamate** sono presenti ulteriori informazioni sull'elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="4398c-262">Fare clic sulla scheda **stack di chiamate** . Il riquadro **stack di chiamate** Mostra lo stack di chiamate per la chiamata JavaScript che ha creato l'elemento.</span><span class="sxs-lookup"><span data-stu-id="4398c-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="4398c-263">Le chiamate a librerie esterne come jQuery sono compresse, in modo che sia possibile visualizzare facilmente le chiamate allo script dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4398c-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="4398c-264">Per visualizzare lo stack completo, incluse le chiamate alle librerie esterne, è possibile espandere i nodi con etichetta "External Libraries":</span><span class="sxs-lookup"><span data-stu-id="4398c-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="4398c-265">Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice ed evidenzia lo script corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4398c-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="4398c-266">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="4398c-266">See Also</span></span>

<span data-ttu-id="4398c-267">[Introduzione a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="4398c-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="4398c-268">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video Channel 9)</span><span class="sxs-lookup"><span data-stu-id="4398c-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="4398c-269">[Messaggi di errore controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="4398c-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
