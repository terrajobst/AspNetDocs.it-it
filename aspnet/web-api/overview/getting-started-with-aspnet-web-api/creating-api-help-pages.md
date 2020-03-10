---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Questa esercitazione con il codice Mostra come creare pagine della Guida per API Web ASP.NET in ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556875"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="fac44-103">Creazione di pagine della Guida per API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fac44-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="fac44-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fac44-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fac44-105">Questa esercitazione con il codice Mostra come creare pagine della Guida per API Web ASP.NET in ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="fac44-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="fac44-106">Quando si crea un'API Web, è spesso utile creare una pagina della guida, in modo che altri sviluppatori sappiano come chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="fac44-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="fac44-107">È possibile creare tutta la documentazione manualmente, ma è preferibile eseguire la generazione del più possibile.</span><span class="sxs-lookup"><span data-stu-id="fac44-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="fac44-108">Per semplificare questa attività, API Web ASP.NET fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fac44-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="fac44-109">Creazione di pagine della Guida dell'API</span><span class="sxs-lookup"><span data-stu-id="fac44-109">Creating API Help Pages</span></span>

<span data-ttu-id="fac44-110">Installare [ASP.NET and Web Tools aggiornamento 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="fac44-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="fac44-111">Questo aggiornamento integra le pagine della guida nel modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="fac44-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="fac44-112">Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="fac44-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="fac44-113">Il modello di progetto crea un esempio di controller API denominato `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="fac44-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="fac44-114">Il modello crea anche le pagine della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="fac44-114">The template also creates the API help pages.</span></span> <span data-ttu-id="fac44-115">Tutti i file di codice per la pagina della guida vengono inseriti nella cartella aree del progetto.</span><span class="sxs-lookup"><span data-stu-id="fac44-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="fac44-116">Quando si esegue l'applicazione, il home page contiene un collegamento alla pagina della Guida dell'API.</span><span class="sxs-lookup"><span data-stu-id="fac44-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="fac44-117">Dal home page, il percorso relativo è/help.</span><span class="sxs-lookup"><span data-stu-id="fac44-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="fac44-118">Questo collegamento consente di portare a una pagina di riepilogo dell'API.</span><span class="sxs-lookup"><span data-stu-id="fac44-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="fac44-119">La visualizzazione MVC per questa pagina è definita in areas/HelpPage/views/help/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="fac44-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="fac44-120">È possibile modificare questa pagina per modificare il layout, l'introduzione, il titolo, gli stili e così via.</span><span class="sxs-lookup"><span data-stu-id="fac44-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="fac44-121">La parte principale della pagina è una tabella di API, raggruppate per controller.</span><span class="sxs-lookup"><span data-stu-id="fac44-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="fac44-122">Le voci della tabella vengono generate in modo dinamico, usando l'interfaccia **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="fac44-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="fac44-123">(Parlerò di questa interfaccia più avanti). Se si aggiunge un nuovo controller API, la tabella viene aggiornata automaticamente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fac44-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="fac44-124">La colonna "API" elenca il metodo HTTP e l'URI relativo.</span><span class="sxs-lookup"><span data-stu-id="fac44-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="fac44-125">La colonna "Description" contiene la documentazione relativa a ogni API.</span><span class="sxs-lookup"><span data-stu-id="fac44-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="fac44-126">Inizialmente, la documentazione è semplicemente testo segnaposto.</span><span class="sxs-lookup"><span data-stu-id="fac44-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="fac44-127">Nella sezione successiva verrà illustrato come aggiungere la documentazione dai commenti XML.</span><span class="sxs-lookup"><span data-stu-id="fac44-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="fac44-128">Ogni API dispone di un collegamento a una pagina con informazioni più dettagliate, inclusi i corpi di richiesta e risposta di esempio.</span><span class="sxs-lookup"><span data-stu-id="fac44-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="fac44-129">Aggiunta di pagine della guida a un progetto esistente</span><span class="sxs-lookup"><span data-stu-id="fac44-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="fac44-130">È possibile aggiungere pagine della guida a un progetto API Web esistente usando Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="fac44-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="fac44-131">Questa opzione è utile per iniziare da un modello di progetto diverso da quello del modello "API Web".</span><span class="sxs-lookup"><span data-stu-id="fac44-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="fac44-132">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="fac44-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="fac44-133">Nella finestra [console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Digitare uno dei comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fac44-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="fac44-134">Per un' **C#** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="fac44-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="fac44-135">Per un'applicazione **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="fac44-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="fac44-136">Sono disponibili due pacchetti, uno per C# e uno per Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="fac44-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="fac44-137">Assicurarsi di usare quello che corrisponde al progetto.</span><span class="sxs-lookup"><span data-stu-id="fac44-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="fac44-138">Questo comando installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della guida (che si trovano nella cartella areas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="fac44-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="fac44-139">È necessario aggiungere manualmente un collegamento alla pagina della guida.</span><span class="sxs-lookup"><span data-stu-id="fac44-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="fac44-140">L'URI è/help.</span><span class="sxs-lookup"><span data-stu-id="fac44-140">The URI is /Help.</span></span> <span data-ttu-id="fac44-141">Per creare un collegamento in una visualizzazione Razor, aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="fac44-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="fac44-142">Assicurarsi anche di registrare le aree.</span><span class="sxs-lookup"><span data-stu-id="fac44-142">Also, make sure to register areas.</span></span> <span data-ttu-id="fac44-143">Nel file Global. asax aggiungere il codice seguente all' **applicazione\_** metodo di avvio, se non è già presente:</span><span class="sxs-lookup"><span data-stu-id="fac44-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="fac44-144">Aggiunta della documentazione dell'API</span><span class="sxs-lookup"><span data-stu-id="fac44-144">Adding API Documentation</span></span>

<span data-ttu-id="fac44-145">Per impostazione predefinita, le pagine della guida contengono stringhe segnaposto per la documentazione.</span><span class="sxs-lookup"><span data-stu-id="fac44-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="fac44-146">È possibile utilizzare i commenti relativi alla [documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione.</span><span class="sxs-lookup"><span data-stu-id="fac44-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="fac44-147">Per abilitare questa funzionalità, aprire il file areas/HelpPage/app\_Start/HelpPageConfig. cs e rimuovere il commento dalla riga seguente:</span><span class="sxs-lookup"><span data-stu-id="fac44-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="fac44-148">Abilitare ora la documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="fac44-148">Now enable XML documentation.</span></span> <span data-ttu-id="fac44-149">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="fac44-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="fac44-150">Selezionare la pagina **Compila** .</span><span class="sxs-lookup"><span data-stu-id="fac44-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="fac44-151">In **output**selezionare **file di documentazione XML**.</span><span class="sxs-lookup"><span data-stu-id="fac44-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="fac44-152">Nella casella di modifica digitare "app\_data/XmlDocument. xml".</span><span class="sxs-lookup"><span data-stu-id="fac44-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="fac44-153">Aprire quindi il codice per il controller API `ValuesController`, che è definito in/Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="fac44-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="fac44-154">Aggiungere alcuni commenti alla documentazione ai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="fac44-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="fac44-155">Esempio:</span><span class="sxs-lookup"><span data-stu-id="fac44-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="fac44-156">Suggerimento: se si posiziona il cursore sulla riga sopra il metodo e si digita tre barre, Visual Studio inserisce automaticamente gli elementi XML.</span><span class="sxs-lookup"><span data-stu-id="fac44-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="fac44-157">È quindi possibile compilare gli spazi vuoti.</span><span class="sxs-lookup"><span data-stu-id="fac44-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="fac44-158">A questo punto, compilare ed eseguire nuovamente l'applicazione e passare alle pagine della guida.</span><span class="sxs-lookup"><span data-stu-id="fac44-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="fac44-159">Le stringhe di documentazione dovrebbero essere visualizzate nella tabella API.</span><span class="sxs-lookup"><span data-stu-id="fac44-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="fac44-160">La pagina della guida legge le stringhe dal file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fac44-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="fac44-161">Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.</span><span class="sxs-lookup"><span data-stu-id="fac44-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="fac44-162">Dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="fac44-162">Under the Hood</span></span>

<span data-ttu-id="fac44-163">Le pagine della guida si basano sulla classe **ApiExplorer** , che fa parte del Framework API Web.</span><span class="sxs-lookup"><span data-stu-id="fac44-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="fac44-164">La classe **ApiExplorer** fornisce il materiale non elaborato per la creazione di una pagina della guida.</span><span class="sxs-lookup"><span data-stu-id="fac44-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="fac44-165">Per ogni API, **ApiExplorer** contiene un **ApiDescription** che descrive l'API.</span><span class="sxs-lookup"><span data-stu-id="fac44-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="fac44-166">A questo scopo, un'"API" viene definita come la combinazione di metodo HTTP e URI relativo.</span><span class="sxs-lookup"><span data-stu-id="fac44-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="fac44-167">Ad esempio, di seguito sono riportate alcune API distinte:</span><span class="sxs-lookup"><span data-stu-id="fac44-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="fac44-168">OTTENERE/api/Products</span><span class="sxs-lookup"><span data-stu-id="fac44-168">GET /api/Products</span></span>
- <span data-ttu-id="fac44-169">OTTENERE/api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="fac44-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="fac44-170">POST/api/Products</span><span class="sxs-lookup"><span data-stu-id="fac44-170">POST /api/Products</span></span>

<span data-ttu-id="fac44-171">Se un'azione del controller supporta più metodi HTTP, **ApiExplorer** considera ogni metodo come un'API distinta.</span><span class="sxs-lookup"><span data-stu-id="fac44-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="fac44-172">Per nascondere un'API da **ApiExplorer**, aggiungere l'attributo **ApiExplorerSettings** all'azione e impostare *IgnoreApi* su true.</span><span class="sxs-lookup"><span data-stu-id="fac44-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="fac44-173">È inoltre possibile aggiungere questo attributo al controller, in modo da escludere l'intero controller.</span><span class="sxs-lookup"><span data-stu-id="fac44-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="fac44-174">La classe ApiExplorer ottiene le stringhe di documentazione dall'interfaccia **IDocumentationProvider** .</span><span class="sxs-lookup"><span data-stu-id="fac44-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="fac44-175">Come illustrato in precedenza, la libreria pagine della guida fornisce un **IDocumentationProvider** che ottiene la documentazione dalle stringhe di documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="fac44-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="fac44-176">Il codice si trova in/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="fac44-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="fac44-177">È possibile ottenere la documentazione da un'altra origine scrivendo il proprio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="fac44-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="fac44-178">Per eseguire il collegamento, chiamare il metodo di estensione **SetDocumentationProvider** , definito in **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="fac44-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="fac44-179">**ApiExplorer** chiama automaticamente nell'interfaccia **IDocumentationProvider** per ottenere le stringhe di documentazione per ogni API.</span><span class="sxs-lookup"><span data-stu-id="fac44-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="fac44-180">Li archivia nella proprietà **Documentation** degli oggetti **ApiDescription** e **ApiParameterDescription** .</span><span class="sxs-lookup"><span data-stu-id="fac44-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fac44-181">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fac44-181">Next Steps</span></span>

<span data-ttu-id="fac44-182">Non si è limitati alle pagine della Guida visualizzate qui.</span><span class="sxs-lookup"><span data-stu-id="fac44-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="fac44-183">In realtà, **ApiExplorer** non è limitato alla creazione di pagine della guida.</span><span class="sxs-lookup"><span data-stu-id="fac44-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="fac44-184">Yao Huang Lin ha scritto alcuni ottimi post di Blog per iniziare a pensarci:</span><span class="sxs-lookup"><span data-stu-id="fac44-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="fac44-185">Aggiunta di un client di test semplice alla pagina della Guida di API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fac44-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="fac44-186">Creazione di API Web ASP.NET pagina della Guida lavoro sui servizi indipendenti</span><span class="sxs-lookup"><span data-stu-id="fac44-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="fac44-187">Generazione in fase di progettazione della pagina della guida (o client) per API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fac44-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="fac44-188">Personalizzazioni della pagina della guida avanzate</span><span class="sxs-lookup"><span data-stu-id="fac44-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
