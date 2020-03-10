---
uid: webhooks/diagnostics/logging
title: Registrazione webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Come eseguire la registrazione nei webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547880"
---
# <a name="aspnet-webhooks-logging"></a>Registrazione webhook ASP.NET

Microsoft ASP.NET webhook usa la registrazione come modo per segnalare problemi e problemi. Per impostazione predefinita, i log vengono scritti usando [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) , dove possono essere modificati usando i [listener di traccia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) come qualsiasi altro flusso di log.

Quando si distribuisce un'applicazione Web come app Web di Azure, i log vengono selezionati automaticamente e possono essere gestiti insieme a qualsiasi altra registrazione [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) . Per informazioni dettagliate, vedere [abilitare la registrazione diagnostica per le app Web nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Inoltre, i log possono essere ottenuti direttamente dall'interno di Visual Studio, come descritto in [risolvere i problemi di un'app Web nel servizio app Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Reindirizzamento dei log

Anziché scrivere log in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), è possibile fornire un'implementazione di registrazione alternativa che possa accedere direttamente a un gestore di log, ad esempio [log4net](http://logging.apache.org/log4net/) e [NLog](http://nlog-project.org/). È sufficiente fornire un'implementazione di [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrarla con un motore di inserimento delle dipendenze a scelta, che verrà prelevata da Microsoft ASP.NET webhook. Per informazioni dettagliate, vedere [inserimento delle dipendenze nella API Web ASP.NET 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) .
