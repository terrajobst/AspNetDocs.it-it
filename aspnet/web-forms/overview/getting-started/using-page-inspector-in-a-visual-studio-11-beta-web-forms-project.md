---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Uso di Controllo pagina per Visual Studio 2012 in ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e Controllo pagina...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575922"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="26713-104">Uso di Controllo pagina per Visual Studio 2012 in Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26713-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="26713-105">di Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="26713-105">by Tim Ammann</span></span>

> <span data-ttu-id="26713-106">Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo Web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="26713-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="26713-107">Selezionare qualsiasi elemento nel browser integrato e Controllo pagina evidenzia immediatamente l'origine e il CSS dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="26713-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="26713-108">È possibile esplorare qualsiasi pagina dell'applicazione, trovare rapidamente le origini del markup di cui è stato eseguito il rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26713-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="26713-109">Questa esercitazione illustra come abilitare modalità Controllo e quindi individuare e modificare rapidamente le regole e il testo CSS nel progetto Web.</span><span class="sxs-lookup"><span data-stu-id="26713-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="26713-110">Nell'esercitazione viene utilizzato un progetto di applicazione Web Form, ma è inoltre possibile utilizzare Controllo pagina per progetti di siti Web e applicazioni [MVC](https://go.microsoft.com/?linkid=9802002) .</span><span class="sxs-lookup"><span data-stu-id="26713-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="26713-111">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="26713-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="26713-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="26713-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="26713-113">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="26713-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="26713-114">Usare Controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="26713-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="26713-115">Abilita modalità Controllo</span><span class="sxs-lookup"><span data-stu-id="26713-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="26713-116">Usare Controllo pagina per apportare modifiche al markup</span><span class="sxs-lookup"><span data-stu-id="26713-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="26713-117">modalità Controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="26713-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="26713-118">Anteprima delle modifiche CSS nella finestra Stili</span><span class="sxs-lookup"><span data-stu-id="26713-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="26713-119">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="26713-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="26713-120">Uso della selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="26713-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="26713-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="26713-121">Prerequisites</span></span>

- <span data-ttu-id="26713-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="26713-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="26713-123">Per ottenere la versione più recente di Controllo pagina, usare l' [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="26713-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="26713-124">Controllo pagina viene fornito con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="26713-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="26713-125">La versione più recente è la 1,3.</span><span class="sxs-lookup"><span data-stu-id="26713-125">The latest version is 1.3.</span></span> <span data-ttu-id="26713-126">Per verificare quale versione è disponibile, eseguire Visual Studio e selezionare **informazioni Microsoft Visual Studio** **dal menu?** .</span><span class="sxs-lookup"><span data-stu-id="26713-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="26713-127">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="26713-127">Create a Web Application</span></span>

<span data-ttu-id="26713-128">In primo luogo, si creerà un'applicazione Web che sarà utilizzata Controllo pagina con.</span><span class="sxs-lookup"><span data-stu-id="26713-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="26713-129">In Visual Studio scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="26713-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="26713-130">A sinistra espandere **Visual C#** , selezionare **Web**, quindi selezionare **ASP.NET Web Forms Application**.</span><span class="sxs-lookup"><span data-stu-id="26713-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="26713-132">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="26713-132">Click **OK**.</span></span>

<span data-ttu-id="26713-133">L'applicazione verrà aperta nella visualizzazione **origine** .</span><span class="sxs-lookup"><span data-stu-id="26713-133">The application opens in **Source** view.</span></span>

![Nuova applicazione Web Form nella visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="26713-135">Ora che si dispone di un'applicazione da usare, è possibile usare Controllo pagina per esaminarla e modificarla.</span><span class="sxs-lookup"><span data-stu-id="26713-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="26713-136">Usare Controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="26713-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="26713-137">Si procederà quindi alla visualizzazione dell'applicazione con Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="26713-138">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="26713-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Visualizza in Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="26713-140">Per impostazione predefinita, quando Controllo pagina viene avviato per la prima volta, viene ancorato come finestra stretta sul lato sinistro dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26713-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="26713-141">Lasciarla ancorata sul lato sinistro e impostarla su una larghezza comoda per l'utente oppure ancorarla in una delle aree degli strumenti nella parte superiore, inferiore o destra:</span><span class="sxs-lookup"><span data-stu-id="26713-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Controllo pagina le posizioni di ancoraggio](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="26713-143">Se si annulla l'ancoraggio della finestra di Controllo pagina, è possibile inserirla all'esterno di Visual Studio o anche in un secondo monitoraggio, se presente.</span><span class="sxs-lookup"><span data-stu-id="26713-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="26713-144">Tuttavia, per premere ALT + TAB tra Controllo pagina e Visual Studio quando la finestra di Controllo pagina non è ancorata, passare a **strumenti** &gt; **opzioni** &gt; **ambiente** &gt; **schede e finestre**e in area **scheda**deselezionare la casella di controllo denominata **finestre degli strumenti mobili sempre nella parte superiore della finestra principale**:</span><span class="sxs-lookup"><span data-stu-id="26713-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Deselezionare la casella di controllo finestre degli strumenti mobili per ALT + TAB tra Visual Studio e la finestra di Controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="26713-146">Il riquadro superiore della finestra Controllo pagina mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="26713-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="26713-147">Il riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare diversi aspetti della pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="26713-148">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="26713-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="26713-149">Tuttavia, a differenza degli strumenti di sviluppo, è possibile usare Controllo pagina direttamente all'interno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26713-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="26713-151">In questa esercitazione si userà il riquadro browser Controllo pagina e le schede **HTML** e **stili** per facilitare la navigazione e l'applicazione di modifiche.</span><span class="sxs-lookup"><span data-stu-id="26713-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="26713-152">Abilita modalità Controllo</span><span class="sxs-lookup"><span data-stu-id="26713-152">Enable Inspection Mode</span></span>

<span data-ttu-id="26713-153">Successivamente, verrà illustrato il funzionamento di Controllo pagina modalità Controllo.</span><span class="sxs-lookup"><span data-stu-id="26713-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="26713-154">Nella finestra di Controllo pagina fare clic sul pulsante **Controlla** .</span><span class="sxs-lookup"><span data-stu-id="26713-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="26713-156">Per visualizzare la modalità di ispezione in azione, spostare il mouse su diverse parti della pagina all'interno della finestra del browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="26713-157">Come si fa, il puntatore del mouse assume la forma di un segno più grande e l'elemento sottostante è evidenziato:</span><span class="sxs-lookup"><span data-stu-id="26713-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Passaggio del mouse su div. Content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="26713-159">Quando si sposta il puntatore del mouse, si noti che</span><span class="sxs-lookup"><span data-stu-id="26713-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="26713-160">Il contenuto nella visualizzazione **origine** cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="26713-161">Il markup pertinente è evidenziato.</span><span class="sxs-lookup"><span data-stu-id="26713-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="26713-162">Se l'origine si trova in un altro file, il file viene aperto nella visualizzazione origine con il markup pertinente evidenziato.</span><span class="sxs-lookup"><span data-stu-id="26713-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="26713-163">Il markup visualizzato nella scheda **HTML** in controllo pagina cambia anche in modo che corrisponda all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="26713-164">Nella scheda **HTML** viene descritto il markup pertinente.</span><span class="sxs-lookup"><span data-stu-id="26713-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="26713-165">Nella scheda **stili** vengono visualizzate le regole CSS rilevanti per la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="26713-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="26713-166">Usare Controllo pagina per apportare modifiche al markup</span><span class="sxs-lookup"><span data-stu-id="26713-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="26713-167">Verrà ora illustrato come utilizzare Controllo pagina per individuare e modificare il markup o il testo il cui percorso potrebbe non essere immediatamente ovvio.</span><span class="sxs-lookup"><span data-stu-id="26713-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="26713-168">Inserire Controllo pagina in modalità Controllo e scorrere fino alla fine della home page.</span><span class="sxs-lookup"><span data-stu-id="26713-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="26713-169">Non appena si immette l'area piè di pagina, Controllo pagina apre il file di layout *site. master* nella visualizzazione **origine** in una scheda temporanea a destra delle altre schede ed evidenzia la sezione della pagina master selezionata.</span><span class="sxs-lookup"><span data-stu-id="26713-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="26713-170">Viene illustrato come Controllo pagina possibile trovare e visualizzare contenuto in una pagina che può effettivamente provenire da un file diverso da quello che è stato originariamente aperto.</span><span class="sxs-lookup"><span data-stu-id="26713-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Evidenziazioni del piè di pagina in modalità Controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="26713-172">Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla riga con la nota sul <a id="a"> </a>copyright.</span><span class="sxs-lookup"><span data-stu-id="26713-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="26713-173">Nella pagina *site. master* viene evidenziata la riga corrispondente.</span><span class="sxs-lookup"><span data-stu-id="26713-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Linea di copyright del piè di pagina evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="26713-175">Aggiungere testo alla fine della riga nel file *site. master* .</span><span class="sxs-lookup"><span data-stu-id="26713-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="26713-176">&lt;p&gt;copia &amp;; &lt;%: DateTime. Now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="26713-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="26713-177">A questo punto, premere CTRL + ALT + INVIO oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="26713-179">Si potrebbe pensare che il piè di pagina si trovasse nella pagina *default. aspx* , ma si è rivelato nella pagina del layout master e controllo pagina trovato.</span><span class="sxs-lookup"><span data-stu-id="26713-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="26713-180">modalità Controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="26713-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="26713-181">Successivamente, sarà possibile esaminare rapidamente la finestra HTML e il modo in cui viene eseguito il mapping degli elementi.</span><span class="sxs-lookup"><span data-stu-id="26713-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="26713-182">Inserire Controllo pagina in modalità Controllo.</span><span class="sxs-lookup"><span data-stu-id="26713-182">Put Page Inspector in Inspection Mode.</span></span>

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="26713-184">Fare clic sulla parte superiore della pagina che indica "il logo qui".</span><span class="sxs-lookup"><span data-stu-id="26713-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="26713-185">Si sta esaminando un particolare elemento in modo più dettagliato, quindi la visualizzazione nella finestra del browser non cambia più quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="26713-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="26713-186">Spostare quindi il puntatore del mouse nella finestra **HTML** .</span><span class="sxs-lookup"><span data-stu-id="26713-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="26713-187">Quando si sposta il puntatore del mouse, Controllo pagina delinea l'elemento all'interno della finestra **HTML** ed evidenzia l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="26713-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="26713-189">Come in precedenza, Controllo pagina apre il file *site. master* in una scheda temporanea. fare clic sulla scheda Site. master e il markup corrispondente viene evidenziato nell'intestazione &lt;&gt; sezione:</span><span class="sxs-lookup"><span data-stu-id="26713-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="26713-191">Anteprima delle modifiche CSS nella finestra Stili</span><span class="sxs-lookup"><span data-stu-id="26713-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="26713-192">Successivamente, verrà illustrato come utilizzare la finestra **stili** controllo pagina per visualizzare in anteprima le modifiche apportate a CSS.</span><span class="sxs-lookup"><span data-stu-id="26713-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="26713-193">Fare clic sul pulsante **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="26713-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="26713-194">Nella finestra del browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="26713-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Passaggio del mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="26713-196">Fare clic una volta all'interno della sezione div. Content-wrapper, quindi spostare il puntatore del mouse nella finestra **stili** .</span><span class="sxs-lookup"><span data-stu-id="26713-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="26713-197">Nel selettore di classe. Featured. Content-wrapper deselezionare e selezionare la casella di controllo per la proprietà colore sfondo.</span><span class="sxs-lookup"><span data-stu-id="26713-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Cancella colore di sfondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="26713-199">Si noti il modo in cui le modifiche vengono visualizzate immediatamente nella finestra del browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="26713-200">Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarlo in `red`.</span><span class="sxs-lookup"><span data-stu-id="26713-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="26713-201">La modifica viene visualizzata immediatamente:</span><span class="sxs-lookup"><span data-stu-id="26713-201">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="26713-203">La finestra **stili** rende più semplice testare e visualizzare in anteprima le modifiche CSS prima di eseguire il commit delle modifiche nel foglio di stile.</span><span class="sxs-lookup"><span data-stu-id="26713-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="26713-204">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="26713-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="26713-205">Questa funzionalità richiede la versione 1,3 di Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="26713-206">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e di visualizzare immediatamente le modifiche nel browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="26713-207">Fare clic su **Controlla** per inserire Controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="26713-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="26713-208">Nel browser Controllo pagina spostare il puntatore del mouse sulla sezione "Home page" fino a quando non viene visualizzata l'etichetta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="26713-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="26713-209">Fare clic una volta per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="26713-209">Click once to select this element.</span></span>

<span data-ttu-id="26713-210">Nella finestra **stili** vengono visualizzate tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="26713-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="26713-211">Scorrere verso il basso per trovare il selettore di classi. Featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="26713-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="26713-212">Fare clic su ". optional. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="26713-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="26713-213">Controllo pagina apre il file CSS che definisce lo stile (site. CSS) ed evidenzia lo stile CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="26713-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="26713-215">Modificare ora il valore di `background-color` in "Red".</span><span class="sxs-lookup"><span data-stu-id="26713-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="26713-216">La modifica viene visualizzata immediatamente nel browser Controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="26713-216">The change appears immediately in the Page Inspector browser.</span></span>

![Browser Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="26713-218">Uso della selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="26713-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="26713-219">Si apprenderà quindi come usare Controllo pagina per trovare e modificare rapidamente il CSS per il testo evidenziato nell'applicazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="26713-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="26713-220">In questo esempio si è deciso di non usare l'evidenziazione blu e si vuole modificarla in un altro colore.</span><span class="sxs-lookup"><span data-stu-id="26713-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="26713-221">Fare clic sul pulsante **Controlla** .</span><span class="sxs-lookup"><span data-stu-id="26713-221">Click the **Inspect** button.</span></span>

![Controlla elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="26713-223">Nella finestra del browser Controllo pagina spostare il puntatore del mouse sul testo "video, esercitazioni ed esempi" evidenziato in modo che venga visualizzata l'etichetta CSS "Mark".</span><span class="sxs-lookup"><span data-stu-id="26713-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passaggio del mouse sull'elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="26713-225">Fare clic sul testo per selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="26713-225">Click the text to select it.</span></span> <span data-ttu-id="26713-226">Il selettore del contrassegno CSS corrispondente viene visualizzato nella parte inferiore della finestra **stili** .</span><span class="sxs-lookup"><span data-stu-id="26713-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![contrassegnare il selettore nella finestra Stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="26713-228">Fare clic sul selettore di contrassegno.</span><span class="sxs-lookup"><span data-stu-id="26713-228">Click the mark selector.</span></span> <span data-ttu-id="26713-229">Verrà aperto il file *site. CSS* per l'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="26713-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="26713-230">Fare clic sulla scheda Site. CSS e viene evidenziato il CSS corrispondente per il selettore:</span><span class="sxs-lookup"><span data-stu-id="26713-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![contrassegna il selettore nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="26713-232">Selezionare e rimuovere la riga con la proprietà colore sfondo.</span><span class="sxs-lookup"><span data-stu-id="26713-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="26713-233">A questo punto si userà la nuova selezione colori CSS di Visual Studio 2012 per scegliere un nuovo colore per la proprietà **contrassegno** sfondo-colore.</span><span class="sxs-lookup"><span data-stu-id="26713-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="26713-234">Uso della selezione colori CSS di Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="26713-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="26713-235">L'editor CSS in Visual Studio 2012 dispone di una selezione colori che semplifica la scelta e l'inserimento dei colori.</span><span class="sxs-lookup"><span data-stu-id="26713-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="26713-236">Dispone di una barra di colore semplice e di un selettore popup che offre un controllo più preciso.</span><span class="sxs-lookup"><span data-stu-id="26713-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="26713-237">La selezione colori include una tavolozza di colori standard, supporta i nomi di colore standard, i codici hash, i colori RGB, RGBA, HSL e HSLA e mantiene un elenco dei colori usati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="26713-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="26713-238">Nella riga in cui si trova la proprietà colore sfondo, digitare "BC" e premere la freccia giù una volta.</span><span class="sxs-lookup"><span data-stu-id="26713-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="26713-239">Quando si digita il primo carattere di ogni parola in una proprietà con valori delimitati da trattini, ad esempio "background-color", IntelliSense filtra l'elenco in modo da visualizzare solo le proprietà corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="26713-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valori filtrati IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="26713-241">A questo punto, digitare i due punti.</span><span class="sxs-lookup"><span data-stu-id="26713-241">Now type a colon.</span></span> <span data-ttu-id="26713-242">Quando si esegue questa operazione, viene inserito il nome completo della proprietà del colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="26713-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="26713-243">Digitare **#** o **RGB (** e viene visualizzata la barra di selezione dei colori:</span><span class="sxs-lookup"><span data-stu-id="26713-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Barra di selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="26713-245">Per vedere come funziona la barra di selezione colori, fare clic sui relativi colori con il puntatore del mouse oppure premere il tasto freccia giù e quindi usare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="26713-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="26713-246">Quando si visita un colore, viene visualizzato l'anteprima del valore corrispondente per la proprietà colore di sfondo:</span><span class="sxs-lookup"><span data-stu-id="26713-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valore della proprietà del colore di sfondo in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="26713-248">A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce CSS.</span><span class="sxs-lookup"><span data-stu-id="26713-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="26713-249">Per il momento, passare alla sezione successiva per vedere come funziona il popup della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="26713-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="26713-250">Uso del popup della selezione colori</span><span class="sxs-lookup"><span data-stu-id="26713-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="26713-251">Quando la barra dei colori non ha il colore esatto che si sta cercando, è possibile usare il popup della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="26713-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="26713-252">Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori oppure premere la freccia giù una o due volte sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="26713-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Popup selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="26713-254">Fare clic su un colore dalla barra verticale a destra.</span><span class="sxs-lookup"><span data-stu-id="26713-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="26713-255">Viene visualizzata una sfumatura per quel colore nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="26713-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="26713-256">Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su un punto qualsiasi nella finestra principale per scegliere con maggiore precisione.</span><span class="sxs-lookup"><span data-stu-id="26713-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="26713-257">Se nella schermata del computer è presente un colore che si desidera utilizzare (non è necessario che sia all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando lo strumento contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="26713-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="26713-258">È anche possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="26713-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="26713-259">In questo modo, i valori dei colori vengono modificati in valori RGBA perché il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="26713-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="26713-260">Dopo aver scelto un colore, premere INVIO, quindi digitare un punto e virgola per completare la voce relativa al colore di sfondo nel file *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="26713-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="26713-261">La barra di aggiornamento Controllo pagina</span><span class="sxs-lookup"><span data-stu-id="26713-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="26713-262">Controllo pagina rileva immediatamente la modifica apportata al file *site. CSS* (o a qualsiasi file nell'applicazione) e visualizza un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="26713-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="26713-264">Per salvare tutti i file e aggiornare il browser Controllo pagina, premere CTRL + ALT + INVIO o fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="26713-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="26713-265">La modifica apportata al colore di evidenziazione viene visualizzata nel browser:</span><span class="sxs-lookup"><span data-stu-id="26713-265">The change in the highlight color appears in the browser:</span></span>

![Colore di evidenziazione modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="26713-267">Si noti che è stato aggiornato comodamente il browser Controllo pagina direttamente dall'ambiente Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26713-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="26713-268">L'uso di Controllo pagina anziché di un browser esterno consente di rimanere nell'editor quando si sviluppano applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="26713-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="26713-269">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="26713-269">See Also</span></span>

<span data-ttu-id="26713-270">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video Channel 9)</span><span class="sxs-lookup"><span data-stu-id="26713-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="26713-271">[Messaggi di errore controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="26713-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
