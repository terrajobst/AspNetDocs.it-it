---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "Usare OWIN per l'hosting indipendente di ASP.NET Web API: ASP.NET 4.x"
author: rick-anderson
description: Esercitazione con il codice che illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131654"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="57ccb-103">Usare OWIN per l'hosting indipendente di API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="57ccb-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="57ccb-104">Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web.</span><span class="sxs-lookup"><span data-stu-id="57ccb-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="57ccb-105">[Open Web Interface for .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="57ccb-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="57ccb-106">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="57ccb-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="57ccb-107">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="57ccb-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="57ccb-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="57ccb-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="57ccb-109">Web API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="57ccb-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="57ccb-110">È possibile trovare il codice sorgente completo per questa esercitazione in [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="57ccb-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="57ccb-111">Creare un'applicazione console</span><span class="sxs-lookup"><span data-stu-id="57ccb-111">Create a console application</span></span>

<span data-ttu-id="57ccb-112">Nel **File** dal menu **New**, quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="57ccb-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="57ccb-113">Dal **Installed**, in **Visual C#** , selezionare **Desktop di Windows** e quindi selezionare **App Console (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="57ccb-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="57ccb-114">Denominare il progetto "OwinSelfhostSample" e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="57ccb-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="57ccb-115">Aggiungere i pacchetti di API Web e OWIN</span><span class="sxs-lookup"><span data-stu-id="57ccb-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="57ccb-116">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="57ccb-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="57ccb-117">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="57ccb-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="57ccb-118">Verranno installati il pacchetto selfhost WebAPI OWIN e tutti i pacchetti necessari di OWIN.</span><span class="sxs-lookup"><span data-stu-id="57ccb-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="57ccb-119">Configurare Web API per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="57ccb-119">Configure Web API for self-host</span></span>

<span data-ttu-id="57ccb-120">In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="57ccb-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="57ccb-121">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="57ccb-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="57ccb-122">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ccb-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="57ccb-123">Aggiungere un controller API Web</span><span class="sxs-lookup"><span data-stu-id="57ccb-123">Add a Web API controller</span></span>

<span data-ttu-id="57ccb-124">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="57ccb-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="57ccb-125">In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="57ccb-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="57ccb-126">Assegnare alla classe il nome `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="57ccb-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="57ccb-127">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ccb-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="57ccb-128">Avviare l'Host OWIN e inviare una richiesta con HttpClient</span><span class="sxs-lookup"><span data-stu-id="57ccb-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="57ccb-129">Sostituire tutto il codice boilerplate nel file Program.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="57ccb-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="57ccb-130">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="57ccb-130">Run the application</span></span>

<span data-ttu-id="57ccb-131">Per eseguire l'applicazione, premere F5 in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57ccb-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="57ccb-132">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="57ccb-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="57ccb-133">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="57ccb-133">Additional resources</span></span>

[<span data-ttu-id="57ccb-134">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="57ccb-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="57ccb-135">Hosting dell'API Web ASP.NET in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="57ccb-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
