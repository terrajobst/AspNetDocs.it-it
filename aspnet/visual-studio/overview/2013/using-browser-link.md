---
uid: visual-studio/overview/2013/using-browser-link
title: Utilizzo di Browser Link in Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622906"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="9b708-102">Utilizzo di Browser Link in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9b708-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="9b708-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9b708-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9b708-104">Browser Link è una nuova funzionalità di Visual Studio 2013 che consente di creare un canale di comunicazione tra l'ambiente di sviluppo e uno o più Web browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="9b708-105">È possibile utilizzare Browser Link per aggiornare l'applicazione Web in diversi browser contemporaneamente, operazione utile per i test tra browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="9b708-106">Aggiornamento del browser</span><span class="sxs-lookup"><span data-stu-id="9b708-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="9b708-107">Visualizzazione del Dashboard Browser Link</span><span class="sxs-lookup"><span data-stu-id="9b708-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="9b708-108">Abilitazione di Browser Link per i file HTML statici</span><span class="sxs-lookup"><span data-stu-id="9b708-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="9b708-109">Disabilitazione di Browser Link</span><span class="sxs-lookup"><span data-stu-id="9b708-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="9b708-110">Come funziona?</span><span class="sxs-lookup"><span data-stu-id="9b708-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="9b708-111">Aggiornamento del browser</span><span class="sxs-lookup"><span data-stu-id="9b708-111">Browser Refresh</span></span>

<span data-ttu-id="9b708-112">Con l'aggiornamento del browser è possibile aggiornare più browser connessi a Visual Studio tramite Browser Link.</span><span class="sxs-lookup"><span data-stu-id="9b708-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="9b708-113">Per usare l'aggiornamento del browser, creare prima un'applicazione ASP.NET usando uno dei modelli di progetto.</span><span class="sxs-lookup"><span data-stu-id="9b708-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="9b708-114">Eseguire il debug dell'applicazione premendo F5 o facendo clic sull'icona della freccia sulla barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="9b708-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="9b708-115">È anche possibile usare l'elenco a discesa per selezionare un browser specifico per il debug.</span><span class="sxs-lookup"><span data-stu-id="9b708-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="9b708-116">Per eseguire il debug con più browser, selezionare **Sfoglia con**.</span><span class="sxs-lookup"><span data-stu-id="9b708-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="9b708-117">Nella finestra di dialogo **Sfoglia con** tenere premuto il tasto CTRL per selezionare più di un browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="9b708-118">Fare clic su **Sfoglia** per eseguire il debug con i browser selezionati.</span><span class="sxs-lookup"><span data-stu-id="9b708-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="9b708-119">Browser Link funziona anche se si avvia un browser dall'esterno di Visual Studio e si passa all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b708-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="9b708-120">I controlli Browser Link si trovano nell'elenco a discesa con l'icona a forma di freccia circolare.</span><span class="sxs-lookup"><span data-stu-id="9b708-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="9b708-121">L'icona a freccia è il pulsante **Aggiorna** .</span><span class="sxs-lookup"><span data-stu-id="9b708-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="9b708-122">Per visualizzare i browser connessi, passare il puntatore del mouse sul pulsante di **aggiornamento** durante il debug.</span><span class="sxs-lookup"><span data-stu-id="9b708-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="9b708-123">I browser connessi vengono visualizzati in una finestra di descrizione comando.</span><span class="sxs-lookup"><span data-stu-id="9b708-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="9b708-124">Per aggiornare i browser connessi, fare clic sul pulsante **Aggiorna** oppure premere CTRL + ALT + INVIO.</span><span class="sxs-lookup"><span data-stu-id="9b708-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="9b708-125">Ad esempio, lo screenshot seguente mostra un progetto ASP.NET, creato con il modello di progetto MVC 5.</span><span class="sxs-lookup"><span data-stu-id="9b708-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="9b708-126">È possibile visualizzare l'applicazione in esecuzione in due browser nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="9b708-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="9b708-127">Nella parte inferiore il progetto è aperto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b708-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="9b708-128">In Visual Studio è stata modificata l'intestazione &lt;H1&gt; per il home page:</span><span class="sxs-lookup"><span data-stu-id="9b708-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="9b708-129">Quando si è fatto clic sul pulsante **Aggiorna** , la modifica viene visualizzata in entrambe le finestre del browser:</span><span class="sxs-lookup"><span data-stu-id="9b708-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="9b708-130">**Note**</span><span class="sxs-lookup"><span data-stu-id="9b708-130">**Notes**</span></span>

- <span data-ttu-id="9b708-131">Per abilitare Browser Link, impostare `debug=true` nell'elemento [&gt;di compilazione&lt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) nel file Web. config del progetto.</span><span class="sxs-lookup"><span data-stu-id="9b708-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="9b708-132">L'applicazione deve essere in esecuzione su localhost.</span><span class="sxs-lookup"><span data-stu-id="9b708-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="9b708-133">L'applicazione deve essere destinata a .NET 4,0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="9b708-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="9b708-134">Visualizzazione del Dashboard Browser Link</span><span class="sxs-lookup"><span data-stu-id="9b708-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="9b708-135">Il Dashboard Browser Link Visualizza le informazioni sulle connessioni Browser Link.</span><span class="sxs-lookup"><span data-stu-id="9b708-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="9b708-136">Per visualizzare il dashboard, selezionare il menu a discesa Browser Link (la piccola freccia accanto al pulsante di **aggiornamento** ).</span><span class="sxs-lookup"><span data-stu-id="9b708-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="9b708-137">Quindi fare clic su **browser link dashboard**.</span><span class="sxs-lookup"><span data-stu-id="9b708-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="9b708-138">Il dashboard elenca i browser connessi e l'URL a cui è stato spostato ogni browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="9b708-139">La sezione **prerequisiti** illustra tutti i passaggi necessari per abilitare browser link per il progetto.</span><span class="sxs-lookup"><span data-stu-id="9b708-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="9b708-140">Ad esempio, nella schermata seguente viene illustrato un progetto in cui "debug" è impostato su false nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="9b708-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="9b708-141">Abilitazione di Browser Link per i file HTML statici</span><span class="sxs-lookup"><span data-stu-id="9b708-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="9b708-142">Per abilitare Browser Link per i file HTML statici, aggiungere il codice seguente al file Web. config.</span><span class="sxs-lookup"><span data-stu-id="9b708-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="9b708-143">Per motivi di prestazioni, rimuovere questa impostazione quando si pubblica il progetto.</span><span class="sxs-lookup"><span data-stu-id="9b708-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="9b708-144">Disabilitazione di Browser Link</span><span class="sxs-lookup"><span data-stu-id="9b708-144">Disabling Browser Link</span></span>

<span data-ttu-id="9b708-145">Browser Link è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9b708-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="9b708-146">Esistono diversi modi per disabilitarlo:</span><span class="sxs-lookup"><span data-stu-id="9b708-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="9b708-147">Nel menu a discesa Browser Link deselezionare **abilita browser link**.</span><span class="sxs-lookup"><span data-stu-id="9b708-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="9b708-148">Nel file Web. config aggiungere una chiave denominata "vs: EnableBrowserLink" con il valore "false" nella sezione appSettings.</span><span class="sxs-lookup"><span data-stu-id="9b708-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="9b708-149">Nel file Web. config, impostare debug su false.</span><span class="sxs-lookup"><span data-stu-id="9b708-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="9b708-150">Come funziona?</span><span class="sxs-lookup"><span data-stu-id="9b708-150">How Does It Work?</span></span>

<span data-ttu-id="9b708-151">Browser Link USA [SignalR](../../../signalr/index.md) per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="9b708-152">Quando Browser Link è abilitato, Visual Studio funge da server SignalR a cui possono connettersi più client (browser).</span><span class="sxs-lookup"><span data-stu-id="9b708-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="9b708-153">Browser Link registra anche un modulo HTTP con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b708-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="9b708-154">Questo modulo inserisce uno script di &lt;speciale&gt; riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="9b708-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="9b708-155">È possibile visualizzare i riferimenti agli script selezionando "Visualizza origine" nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="9b708-156">I file di origine non vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="9b708-156">Your source files are not modified.</span></span> <span data-ttu-id="9b708-157">Il modulo HTTP inserisce i riferimenti allo script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="9b708-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="9b708-158">Poiché il codice sul lato browser è tutto JavaScript, funziona su tutti i browser [supportati da SignalR](../../../signalr/overview/getting-started/supported-platforms.md), senza richiedere alcun plug-in del browser.</span><span class="sxs-lookup"><span data-stu-id="9b708-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
