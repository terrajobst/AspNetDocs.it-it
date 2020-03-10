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
# <a name="aspnet-webhooks-debugging"></a>Debug di Webhook ASP.NET  

## <a name="debugging-in-azure"></a>Debug in Azure

Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi relativi a un'app Web nel servizio app Azure usando Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debug con l'origine e i simboli

Oltre a eseguire il debug del codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET webhook e, di fatto, .NET. Questa operazione funziona indipendentemente dal fatto che il debug venga eseguito localmente o in remoto. Per prima cosa, configurare Visual Studio per trovare l'origine e i simboli passando a **debug** , quindi **Opzioni e impostazioni**. Impostare le opzioni come segue:

![Opzioni e impostazioni](_static/SourceSymbols.png)

Aggiungere quindi un collegamento a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli. Passare alla scheda **simboli** del menu sopra e aggiungere quanto segue come posizione dei simboli:

```
http://srv.symbolsource.org/pdb/Public
```

Assicurarsi inoltre che la directory della cache abbia un nome breve. in caso contrario, i nomi di file possono essere troppo lunghi e il caricamento dei simboli non verrà caricato. Un percorso di esempio è:

```
C:\SymCache
```

Le impostazioni dovrebbero essere simili a quanto segue:

![Esempio di percorso del file di simboli opzioni](_static/SymSource.png)
