---
uid: webhooks/receiving/dependencies
title: Dipendenze del ricevitore webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze nei webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637284"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="90c21-103">Dipendenze del ricevitore webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="90c21-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="90c21-104">Microsoft ASP.NET webhook è progettato con l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="90c21-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="90c21-105">La maggior parte delle dipendenze nel sistema può essere sostituita con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="90c21-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="90c21-106">Per un elenco delle dipendenze del ricevitore, vedere [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) .</span><span class="sxs-lookup"><span data-stu-id="90c21-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="90c21-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="90c21-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="90c21-108">Per un elenco delle implementazioni predefinite, vedere [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .</span><span class="sxs-lookup"><span data-stu-id="90c21-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
