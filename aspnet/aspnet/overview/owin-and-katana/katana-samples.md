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
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379072"
---
# <a name="katana-samples"></a>Esempi di Katana

by [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Esempi di Katana

**ASP.NET esegue il routing campione** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
In alcune applicazioni si dovrà associare i componenti OWIN nella tabella di route Asp.Net affiancato con componenti non OWIN. In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute fornita da systemweb.

**Creazione di rami pipeline di esempio** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Le pipeline di elaborazione della richiesta OWIN non deve necessariamente essere lineare, può essere sottoposto a branching per elaborare le richieste in modi diversi. Questo esempio viene illustrato come creare una pipeline con rami basata su percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni. Questi componenti sono disponibili nel pacchetto nuget Microsoft.Owin.Mapping.

**Esempio di Server personalizzati** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Viene illustrato come utilizzare un server personalizzato OWIN self-hosting OWIN.

**Esempio incorporato** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Alcuni server OWIN può essere eseguito all'interno di un processo personalizzato (&quot;self-hosted&quot;). In questo esempio viene illustrato come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto nuget di owin.

**Esempio HelloWorld** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN è un server HTTP astrazione di API che consente la portabilità delle applicazioni tra server diversi. In questo esempio viene illustrato come scrivere un'applicazione Hello World usando alcuni **semplici wrapper** tutto l'astrazione di OWIN e l'esecuzione non elaborato in un server web, ad esempio ASP.NET.

**Esempio Hello World non elaborati OWIN** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
In questo esempio viene illustrato come scrivere un'applicazione Hello World tramite il **raw** astrazione OWIN ed eseguirlo in un server web come Asp.Net.

**Esempio di SignalR** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Viene illustrato come self-hosting OWIN tramite SignalR / Katana. Per altre informazioni su SignalR self-hosting, vedere [esercitazione: Hosting indipendente di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Esempio di file statici** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Viene illustrato come supportare le richieste HTTP per i file statici Usa OWIN / Katana.

**API Web** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere l'API Web alla pipeline OWIN.

**Web Socket campione** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Viene illustrato come supportare WebSocket in OWIN usando la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
