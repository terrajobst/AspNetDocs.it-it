---
uid: webhooks/receiving/dependencies
title: Dipendenze del ricevitore Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze in ASP.NET i Webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048728"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="74fb9-103">Dipendenze del ricevitore Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="74fb9-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="74fb9-104">Microsoft ASP.NET WebHooks è progettato con l'inserimento di dipendenza presente.</span><span class="sxs-lookup"><span data-stu-id="74fb9-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="74fb9-105">La maggior parte delle dipendenze nel sistema possono essere sostituite con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="74fb9-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="74fb9-106">Vedi [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore.</span><span class="sxs-lookup"><span data-stu-id="74fb9-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="74fb9-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="74fb9-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="74fb9-108">Vedi [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="74fb9-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
