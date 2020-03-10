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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dipendenze del ricevitore webhook ASP.NET

Microsoft ASP.NET webhook è progettato con l'inserimento di dipendenze. La maggior parte delle dipendenze nel sistema può essere sostituita con implementazioni alternative usando un motore di inserimento delle dipendenze.

Per un elenco delle dipendenze del ricevitore, vedere [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) . Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita. Per un elenco delle implementazioni predefinite, vedere [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .
