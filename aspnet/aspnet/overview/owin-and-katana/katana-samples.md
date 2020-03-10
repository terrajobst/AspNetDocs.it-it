---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584560"
---
# <a name="katana-samples"></a><span data-ttu-id="27cdf-102">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="27cdf-102">Katana Samples</span></span>

<span data-ttu-id="27cdf-103">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="27cdf-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="27cdf-104">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="27cdf-104">Katana Samples</span></span>

<span data-ttu-id="27cdf-105">**Esempio di route ASP.NET** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="27cdf-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="27cdf-106">In alcune applicazioni è necessario associare i componenti OWIN nella tabella di route Asp.Net affiancati a componenti non OWIN.</span><span class="sxs-lookup"><span data-stu-id="27cdf-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="27cdf-107">Questo esempio illustra come usare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute forniti da Microsoft. Owin. host. SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="27cdf-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="27cdf-108">**Esempio di pipeline di branching** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="27cdf-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="27cdf-109">Le pipeline di elaborazione delle richieste OWIN non devono essere lineari, possono essere diramate per elaborare le richieste in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="27cdf-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="27cdf-110">Questo esempio illustra come creare una pipeline di branching basata su percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="27cdf-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="27cdf-111">Questi componenti sono disponibili nel pacchetto NuGet Microsoft. Owin. mapping.</span><span class="sxs-lookup"><span data-stu-id="27cdf-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="27cdf-112">**Esempio di server personalizzato** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="27cdf-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="27cdf-113">Viene illustrato come usare un server OWIN personalizzato quando si esegue l'hosting self-service OWIN.</span><span class="sxs-lookup"><span data-stu-id="27cdf-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="27cdf-114">**Esempio** di [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) | incorporato</span><span class="sxs-lookup"><span data-stu-id="27cdf-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="27cdf-115">Alcuni server OWIN possono essere eseguiti all'interno del proprio processo (&quot;&quot;self-hosted).</span><span class="sxs-lookup"><span data-stu-id="27cdf-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="27cdf-116">Questo esempio illustra come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto NuGet Microsoft. Owin. Hosting.</span><span class="sxs-lookup"><span data-stu-id="27cdf-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="27cdf-117">**Esempio HelloWorld** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="27cdf-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="27cdf-118">OWIN è un'astrazione API del server HTTP che consente la portabilità delle applicazioni in diversi server.</span><span class="sxs-lookup"><span data-stu-id="27cdf-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="27cdf-119">In questo esempio viene illustrato come scrivere un'applicazione Hello World usando alcuni **semplici wrapper** per l'astrazione OWIN non elaborata ed eseguirla in un server web come ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27cdf-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="27cdf-120">**Esempio di Hello World OWIN Raw** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="27cdf-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="27cdf-121">Questo esempio illustra come scrivere un'applicazione Hello World usando l'astrazione OWIN non **elaborata** ed eseguirla in un server web come ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27cdf-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="27cdf-122">**Esempio di signalr** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="27cdf-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="27cdf-123">Viene illustrato come ospitare autonomamente SignalR usando OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="27cdf-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="27cdf-124">Per altre informazioni sull'hosting automatico di SignalR, vedere [esercitazione: Self-host SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="27cdf-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="27cdf-125">**File statici esempio** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="27cdf-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="27cdf-126">Mostra come supportare le richieste HTTP per i file statici usando OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="27cdf-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="27cdf-127">**API Web** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="27cdf-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="27cdf-128">Questo esempio illustra come ospitare OWIN in IIS e aggiungere l'API Web alla pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="27cdf-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="27cdf-129">Esempio di [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) | di **Web socket** </span><span class="sxs-lookup"><span data-stu-id="27cdf-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="27cdf-130">Mostra come supportare i socket Web in OWIN usando la classe [System .NET. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="27cdf-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
