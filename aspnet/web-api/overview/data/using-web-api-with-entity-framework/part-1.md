---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Uso dell'API Web 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web con un back-end API Web ASP.NET. L'esercitazione USA Entity Framework 6 per i dati Lay...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622479"
---
# <a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="9594a-104">Uso dell'API Web 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9594a-104">Using Web API 2 with Entity Framework 6</span></span>

[<span data-ttu-id="9594a-105">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="9594a-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="9594a-106">Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web con un back-end API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9594a-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="9594a-107">L'esercitazione USA Entity Framework 6 per il livello dati ed knockout. js per l'applicazione JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="9594a-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="9594a-108">L'esercitazione illustra anche come distribuire l'app in app Web del servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="9594a-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9594a-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="9594a-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="9594a-110">API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="9594a-110">Web API 2.1</span></span>
> - <span data-ttu-id="9594a-111">Visual Studio 2017 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="9594a-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="9594a-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9594a-112">Entity Framework 6</span></span>
> - <span data-ttu-id="9594a-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="9594a-113">.NET 4.7</span></span>
> - <span data-ttu-id="9594a-114">[Knockout. js](http://knockoutjs.com/) 3,1</span><span class="sxs-lookup"><span data-stu-id="9594a-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="9594a-115">Questa esercitazione USA API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione Web che manipola un database back-end.</span><span class="sxs-lookup"><span data-stu-id="9594a-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="9594a-116">Di seguito è riportata una schermata dell'applicazione che verrà creata.</span><span class="sxs-lookup"><span data-stu-id="9594a-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="9594a-117">L'app usa una progettazione di applicazioni a singola pagina (SPA).</span><span class="sxs-lookup"><span data-stu-id="9594a-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="9594a-118">"Applicazione a pagina singola" è il termine generale per un'applicazione Web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, anziché caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="9594a-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="9594a-119">Al termine del caricamento iniziale della pagina, l'app comunica con il server tramite richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="9594a-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="9594a-120">Le richieste AJAX restituiscono i dati JSON, usati dall'app per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="9594a-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="9594a-121">AJAX non è una novità, ma oggi sono disponibili Framework JavaScript che semplificano la creazione e la gestione di un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="9594a-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="9594a-122">Questa esercitazione USA [knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9594a-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="9594a-123">Ecco i principali blocchi predefiniti per questa app:</span><span class="sxs-lookup"><span data-stu-id="9594a-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="9594a-124">ASP.NET MVC crea la pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="9594a-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="9594a-125">API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="9594a-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="9594a-126">Dati knockout. js: associa gli elementi HTML ai dati JSON.</span><span class="sxs-lookup"><span data-stu-id="9594a-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="9594a-127">Entity Framework comunica con il database.</span><span class="sxs-lookup"><span data-stu-id="9594a-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="9594a-128">Vedi questa app in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="9594a-128">See this app running on Azure</span></span>

<span data-ttu-id="9594a-129">Si desidera visualizzare il sito finito in esecuzione come app Web Live?</span><span class="sxs-lookup"><span data-stu-id="9594a-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="9594a-130">È possibile distribuire una versione completa dell'app nell'account Azure selezionando il pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="9594a-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="9594a-131">Per distribuire questa soluzione in Azure, è necessario un account Azure.</span><span class="sxs-lookup"><span data-stu-id="9594a-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="9594a-132">Se non si dispone già di un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9594a-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="9594a-133">[Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.</span><span class="sxs-lookup"><span data-stu-id="9594a-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9594a-134">[Attiva i vantaggi per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : l'abbonamento MSDN ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="9594a-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="9594a-135">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="9594a-135">Create the project</span></span>

<span data-ttu-id="9594a-136">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9594a-136">Open Visual Studio.</span></span> <span data-ttu-id="9594a-137">Scegliere **nuovo**dal menu **file** , quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="9594a-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="9594a-138">(Oppure selezionare **nuovo progetto** nella pagina iniziale).</span><span class="sxs-lookup"><span data-stu-id="9594a-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="9594a-139">Nella finestra di dialogo **nuovo progetto** selezionare **Web** nel riquadro a sinistra e **ASP.NET Web Application (.NET Framework)** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="9594a-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="9594a-140">Assegnare al progetto il nome **BookService** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="9594a-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="9594a-141">Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **API Web** .</span><span class="sxs-lookup"><span data-stu-id="9594a-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

<span data-ttu-id="9594a-142">Selezionare **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="9594a-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="9594a-143">Configurare le impostazioni di Azure (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="9594a-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="9594a-144">Dopo aver creato il progetto, è possibile scegliere di eseguire la distribuzione in app Azure app Web del servizio in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="9594a-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="9594a-145">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="9594a-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="9594a-146">Nella finestra visualizzata selezionare **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="9594a-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="9594a-147">Verrà visualizzata la finestra di dialogo **Seleziona destinazione di pubblicazione** .</span><span class="sxs-lookup"><span data-stu-id="9594a-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="9594a-148">Selezionare **Crea profilo**.</span><span class="sxs-lookup"><span data-stu-id="9594a-148">Select **Create Profile**.</span></span> <span data-ttu-id="9594a-149">Viene visualizzata la finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="9594a-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="9594a-150">Accettare le impostazioni predefinite o immettere valori diversi per il nome dell'applicazione, il gruppo di risorse, il piano di hosting, la sottoscrizione di Azure e l'area geografica.</span><span class="sxs-lookup"><span data-stu-id="9594a-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="9594a-151">Selezionare **Crea un database SQL**.</span><span class="sxs-lookup"><span data-stu-id="9594a-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="9594a-152">Verrà visualizzata la finestra di dialogo **configura SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="9594a-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="9594a-153">Accettare le impostazioni predefinite o immettere valori diversi.</span><span class="sxs-lookup"><span data-stu-id="9594a-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="9594a-154">Immettere un **nome utente amministratore** e una **password amministratore** per il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="9594a-154">Enter an **Administrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="9594a-155">Al termine, fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="9594a-155">Select **OK** when you're done.</span></span> <span data-ttu-id="9594a-156">Viene nuovamente visualizzata la pagina **Crea servizio app** .</span><span class="sxs-lookup"><span data-stu-id="9594a-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="9594a-157">Selezionare **Crea** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="9594a-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="9594a-158">Viene visualizzato un messaggio nell'angolo in basso a destra che indica che la distribuzione è in corso.</span><span class="sxs-lookup"><span data-stu-id="9594a-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="9594a-159">Dopo un breve periodo di tempo, la finestra di **pubblicazione** viene nuovamente visualizzata.</span><span class="sxs-lookup"><span data-stu-id="9594a-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="9594a-160">Il profilo creato per la distribuzione dell'app è ora disponibile.</span><span class="sxs-lookup"><span data-stu-id="9594a-160">The profile you created to deploy the app is now available.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="9594a-161">avanti</span><span class="sxs-lookup"><span data-stu-id="9594a-161">Next</span></span>](part-2.md)
