---
title: Scegliere tra ASP.NET 4.x e ASP.NET Core
author: rick-anderson
description: Vengono illustrati ASP.NET Core e ASP.NET 4.x e si spiega come scegliere tra le due soluzioni.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: eb216bdac7dd029c3d985f2edd9e70eb91f42883
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040988"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="0464b-103">Scegliere tra ASP.NET 4.x e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="0464b-104">ASP.NET Core è una riprogettazione di ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="0464b-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="0464b-105">Questo articolo elenca le differenze tra i due.</span><span class="sxs-lookup"><span data-stu-id="0464b-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="0464b-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-106">ASP.NET Core</span></span>

<span data-ttu-id="0464b-107">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="0464b-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="0464b-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="0464b-108">ASP.NET 4.x</span></span>

<span data-ttu-id="0464b-109">ASP.NET 4.x è un framework consolidato che offre i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.</span><span class="sxs-lookup"><span data-stu-id="0464b-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="0464b-110">Selezione del framework</span><span class="sxs-lookup"><span data-stu-id="0464b-110">Framework selection</span></span>

<span data-ttu-id="0464b-111">La tabella seguente mette a confronto ASP.NET Core e ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="0464b-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="0464b-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-112">ASP.NET Core</span></span> | <span data-ttu-id="0464b-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="0464b-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="0464b-114">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="0464b-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="0464b-115">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="0464b-115">Build for Windows</span></span>|
|<span data-ttu-id="0464b-116">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0464b-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="0464b-117">Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="0464b-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="0464b-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="0464b-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="0464b-119">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="0464b-119">Multiple versions per machine</span></span>|<span data-ttu-id="0464b-120">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="0464b-120">One version per machine</span></span>|
|<span data-ttu-id="0464b-121">Sviluppare con Visual Studio, [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="0464b-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="0464b-122">Sviluppare con Visual Studio tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="0464b-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="0464b-123">Prestazioni più elevate rispetto ad ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="0464b-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="0464b-124">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="0464b-124">Good performance</span></span>|
|[<span data-ttu-id="0464b-125">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="0464b-126">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0464b-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="0464b-127">Vedere [ASP.NET Core per .NET Framework](xref:index#target-framework) per informazioni sul supporto per ASP.NET Core 2.x in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0464b-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="0464b-128">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="0464b-129">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0464b-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="0464b-130">Siti Web</span><span class="sxs-lookup"><span data-stu-id="0464b-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="0464b-131">API</span><span class="sxs-lookup"><span data-stu-id="0464b-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="0464b-132">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="0464b-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="0464b-133">Distribuire un'app ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="0464b-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="0464b-134">Scenari ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="0464b-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="0464b-135">Siti Web</span><span class="sxs-lookup"><span data-stu-id="0464b-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="0464b-136">API</span><span class="sxs-lookup"><span data-stu-id="0464b-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="0464b-137">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="0464b-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="0464b-138">Creare un'app Web ASP.NET 4.x in Azure</span><span class="sxs-lookup"><span data-stu-id="0464b-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="0464b-139">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0464b-139">Additional resources</span></span>

* [<span data-ttu-id="0464b-140">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0464b-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="0464b-141">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0464b-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>