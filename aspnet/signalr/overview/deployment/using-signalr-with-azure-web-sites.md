---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Uso di SignalR con app Web nel servizio app Azure | Microsoft Docs
author: bradygaster
description: Questo documento descrive come configurare un'applicazione SignalR eseguita in Microsoft Azure. Versioni del software usate nell'esercitazione Visual Studio 2013 o Vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558702"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="282c8-104">Uso di SignalR con app Web in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="282c8-105">di [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="282c8-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="282c8-106">Questo documento descrive come configurare un'applicazione SignalR eseguita in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="282c8-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="282c8-107">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="282c8-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="282c8-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="282c8-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="282c8-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="282c8-109">.NET 4.5</span></span>
> - <span data-ttu-id="282c8-110">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="282c8-110">SignalR version 2</span></span>
> - <span data-ttu-id="282c8-111">Azure SDK 2,3 per Visual Studio 2013 o 2012</span><span class="sxs-lookup"><span data-stu-id="282c8-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="282c8-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="282c8-112">Questions and comments</span></span>
>
> <span data-ttu-id="282c8-113">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="282c8-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="282c8-114">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/)o nei [Forum Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="282c8-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="282c8-115">Sommario</span><span class="sxs-lookup"><span data-stu-id="282c8-115">Table of Contents</span></span>

- [<span data-ttu-id="282c8-116">Introduzione</span><span class="sxs-lookup"><span data-stu-id="282c8-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="282c8-117">Distribuzione di un'app Web SignalR nel servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="282c8-118">Abilitazione di WebSocket nel servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="282c8-119">Uso del backplane della cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="282c8-120">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="282c8-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="282c8-121">Introduzione</span><span class="sxs-lookup"><span data-stu-id="282c8-121">Introduction</span></span>

<span data-ttu-id="282c8-122">ASP.NET SignalR può essere usato per creare un nuovo livello di interattività tra i server e i client Web o .NET.</span><span class="sxs-lookup"><span data-stu-id="282c8-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="282c8-123">Quando sono ospitati in Azure, le applicazioni SignalR possono sfruttare l'ambiente a disponibilità elevata, scalabile ed efficiente in esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="282c8-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="282c8-124">Distribuzione di un'app Web SignalR nel servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="282c8-125">SignalR non aggiunge particolari complicazioni alla distribuzione di un'applicazione in Azure rispetto alla distribuzione in un server locale.</span><span class="sxs-lookup"><span data-stu-id="282c8-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="282c8-126">Un'applicazione che usa SignalR può essere ospitata in Azure senza apportare modifiche alla configurazione o ad altre impostazioni (anche se per il supporto di WebSocket, vedere [Abilitazione di WebSocket nel servizio app Azure](#websocket) di seguito). Per questa esercitazione, si distribuirà l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.</span><span class="sxs-lookup"><span data-stu-id="282c8-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="282c8-127">**Prerequisiti**</span><span class="sxs-lookup"><span data-stu-id="282c8-127">**Prerequisites**</span></span>

- <span data-ttu-id="282c8-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="282c8-128">Visual Studio 2013.</span></span> <span data-ttu-id="282c8-129">Se non si dispone di Visual Studio, Visual Studio 2013 Express per il Web è incluso nell'installazione di Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="282c8-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="282c8-130">[Azure sdk 2,3 per Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2,3 per Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="282c8-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="282c8-131">Per completare l'esercitazione, sarà necessaria una sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="282c8-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="282c8-132">È possibile [attivare i benefici per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)o [iscriversi per ottenere una sottoscrizione di valutazione](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="282c8-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="282c8-133">Distribuzione di un'app Web SignalR in Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="282c8-134">Completare l' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md)oppure scaricare il progetto terminato da [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="282c8-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="282c8-135">In Visual Studio selezionare **Compila**, **pubblica la chat di SignalR**.</span><span class="sxs-lookup"><span data-stu-id="282c8-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="282c8-136">Nella finestra di dialogo "Pubblica Web" selezionare "siti Web di Microsoft Azure".</span><span class="sxs-lookup"><span data-stu-id="282c8-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Selezionare siti Web di Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="282c8-138">Se non è stato effettuato l'accesso alla account Microsoft, fare clic su **Accedi...** nella finestra di dialogo "Seleziona sito Web esistente" ed effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="282c8-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Seleziona sito Web esistente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Accedere ad Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="282c8-141">Nella finestra di dialogo "Seleziona sito Web esistente" fare clic su **nuovo**.</span><span class="sxs-lookup"><span data-stu-id="282c8-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nuovo sito Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="282c8-143">Nella finestra di dialogo "Crea sito in Microsoft Azure" immettere un nome univoco per l'app.</span><span class="sxs-lookup"><span data-stu-id="282c8-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="282c8-144">Selezionare l'area geografica più vicina nell'elenco a discesa Region (area).</span><span class="sxs-lookup"><span data-stu-id="282c8-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="282c8-145">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="282c8-145">Click **Create**.</span></span>

    ![Creazione del sito in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="282c8-147">Nella finestra di dialogo "Pubblica Web" fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="282c8-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Pubblica sito](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="282c8-149">Al termine della pubblicazione dell'app, l'applicazione di chat SignalR ospitata in app Azure app Web del servizio si aprirà in un browser.</span><span class="sxs-lookup"><span data-stu-id="282c8-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Apertura del sito in un browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="282c8-151">Abilitazione di WebSocket nelle app Web del servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="282c8-152">I WebSocket devono essere abilitati in modo esplicito nell'app Web per poter essere usati in un'applicazione SignalR; in caso contrario, verranno usati altri protocolli. per informazioni dettagliate, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports) .</span><span class="sxs-lookup"><span data-stu-id="282c8-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="282c8-153">Per usare WebSocket nelle app Web del servizio app Azure, abilitarlo nella sezione di configurazione dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="282c8-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="282c8-154">A tale scopo, aprire l'app Web nel [portale di gestione di Azure](https://manage.windowsazure.com/)e selezionare Configura.</span><span class="sxs-lookup"><span data-stu-id="282c8-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Scheda Configura](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="282c8-156">Nella parte superiore della pagina di configurazione assicurarsi che .NET 4,5 venga usato per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="282c8-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Impostazione di .NET Framework versione 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="282c8-158">Nella pagina Configurazione selezionare **on**nell'impostazione **WebSocket** .</span><span class="sxs-lookup"><span data-stu-id="282c8-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Impostazione WebSocket: on](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="282c8-160">Nella parte inferiore della pagina di configurazione selezionare **Salva** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="282c8-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Salva impostazioni](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="282c8-162">Uso del backplane della cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="282c8-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="282c8-163">Se si usano più istanze per l'app Web e gli utenti di tali istanze devono interagire tra loro, in modo che, ad esempio, i messaggi di chat creati in un'istanza possano raggiungere gli utenti connessi ad altre istanze, il [backplane della cache Redis di Azure](../performance/scaleout-with-redis.md) deve essere implementato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="282c8-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="282c8-164">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="282c8-164">Next Steps</span></span>

<span data-ttu-id="282c8-165">Per altre informazioni sulle app Web nel servizio app Azure, vedere [Panoramica delle app Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="282c8-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
