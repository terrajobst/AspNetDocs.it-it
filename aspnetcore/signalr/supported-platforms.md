---
title: Piattaforme supportate di ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: e4e84baf0120036b473eac256107b46a4accfe37
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57053388"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="131f4-103">Piattaforme supportate di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="131f4-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="131f4-104">Requisiti di sistema di server</span><span class="sxs-lookup"><span data-stu-id="131f4-104">Server system requirements</span></span>

<span data-ttu-id="131f4-105">SignalR per ASP.NET Core supporta qualsiasi piattaforma di server che supporta ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="131f4-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="131f4-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="131f4-106">JavaScript client</span></span>

<span data-ttu-id="131f4-107">Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito su NodeJS 8 e versioni successive e i browser seguenti:</span><span class="sxs-lookup"><span data-stu-id="131f4-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="131f4-108">Browser</span><span class="sxs-lookup"><span data-stu-id="131f4-108">Browser</span></span>                         | <span data-ttu-id="131f4-109">Versione</span><span class="sxs-lookup"><span data-stu-id="131f4-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="131f4-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="131f4-110">Microsoft Edge</span></span>                  | <span data-ttu-id="131f4-111">corrente</span><span class="sxs-lookup"><span data-stu-id="131f4-111">current</span></span> |
| <span data-ttu-id="131f4-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="131f4-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="131f4-113">corrente</span><span class="sxs-lookup"><span data-stu-id="131f4-113">current</span></span> |
| <span data-ttu-id="131f4-114">Google Chrome; include Android</span><span class="sxs-lookup"><span data-stu-id="131f4-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="131f4-115">corrente</span><span class="sxs-lookup"><span data-stu-id="131f4-115">current</span></span> |
| <span data-ttu-id="131f4-116">Safari; include iOS</span><span class="sxs-lookup"><span data-stu-id="131f4-116">Safari; includes iOS</span></span>            | <span data-ttu-id="131f4-117">corrente</span><span class="sxs-lookup"><span data-stu-id="131f4-117">current</span></span> |
| <span data-ttu-id="131f4-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="131f4-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="131f4-119">11</span><span class="sxs-lookup"><span data-stu-id="131f4-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="131f4-120">Client .NET</span><span class="sxs-lookup"><span data-stu-id="131f4-120">.NET client</span></span>

<span data-ttu-id="131f4-121">Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="131f4-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="131f4-122">Ad esempio, [gli sviluppatori di Xamarin possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per la creazione di App per Android usando xamarin. Android 8.4.0.1 e versioni successive e le app iOS con xamarin. IOS 11.14.0.4 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="131f4-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="131f4-123">Se il server è in esecuzione IIS, il trasporto WebSocket richiede IIS 8.0 o versione successiva in Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="131f4-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="131f4-124">Sono supportati altri tipi di trasporto in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="131f4-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="131f4-125">Client Java</span><span class="sxs-lookup"><span data-stu-id="131f4-125">Java client</span></span>

<span data-ttu-id="131f4-126">Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="131f4-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="131f4-127">Client non supportati</span><span class="sxs-lookup"><span data-stu-id="131f4-127">Unsupported clients</span></span>

<span data-ttu-id="131f4-128">I client seguenti sono disponibili ma sono sperimentali o non ufficiali.</span><span class="sxs-lookup"><span data-stu-id="131f4-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="131f4-129">Essi non sono attualmente supportati e non può mai essere.</span><span class="sxs-lookup"><span data-stu-id="131f4-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="131f4-130">Client C++</span><span class="sxs-lookup"><span data-stu-id="131f4-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="131f4-131">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="131f4-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
