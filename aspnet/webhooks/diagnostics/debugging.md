---
uid: webhooks/diagnostics/debugging
title: Debug di Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Come eseguire il debug di Webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640868"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="cdb51-103">Debug di Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cdb51-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="cdb51-104">Debug in Azure</span><span class="sxs-lookup"><span data-stu-id="cdb51-104">Debugging in Azure</span></span>

<span data-ttu-id="cdb51-105">Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi relativi a un'app Web nel servizio app Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="cdb51-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="cdb51-106">Debug con l'origine e i simboli</span><span class="sxs-lookup"><span data-stu-id="cdb51-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="cdb51-107">Oltre a eseguire il debug del codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET webhook e, di fatto, .NET.</span><span class="sxs-lookup"><span data-stu-id="cdb51-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="cdb51-108">Questa operazione funziona indipendentemente dal fatto che il debug venga eseguito localmente o in remoto.</span><span class="sxs-lookup"><span data-stu-id="cdb51-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="cdb51-109">Per prima cosa, configurare Visual Studio per trovare l'origine e i simboli passando a **debug** , quindi **Opzioni e impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="cdb51-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="cdb51-110">Impostare le opzioni come segue:</span><span class="sxs-lookup"><span data-stu-id="cdb51-110">Set the options like this:</span></span>

![Opzioni e impostazioni](_static/SourceSymbols.png)

<span data-ttu-id="cdb51-112">Aggiungere quindi un collegamento a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli.</span><span class="sxs-lookup"><span data-stu-id="cdb51-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="cdb51-113">Passare alla scheda **simboli** del menu sopra e aggiungere quanto segue come posizione dei simboli:</span><span class="sxs-lookup"><span data-stu-id="cdb51-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="cdb51-114">Assicurarsi inoltre che la directory della cache abbia un nome breve. in caso contrario, i nomi di file possono essere troppo lunghi e il caricamento dei simboli non verrà caricato.</span><span class="sxs-lookup"><span data-stu-id="cdb51-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="cdb51-115">Un percorso di esempio è:</span><span class="sxs-lookup"><span data-stu-id="cdb51-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="cdb51-116">Le impostazioni dovrebbero essere simili a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cdb51-116">The settings should look similar to this:</span></span>

![Esempio di percorso del file di simboli opzioni](_static/SymSource.png)
