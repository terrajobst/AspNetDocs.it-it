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
# <a name="katana-samples"></a>Esempi di Katana

[Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Esempi di Katana

**Esempio di route ASP.NET** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
In alcune applicazioni è necessario associare i componenti OWIN nella tabella di route Asp.Net affiancati a componenti non OWIN. Questo esempio illustra come usare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute forniti da Microsoft. Owin. host. SystemWeb.

**Esempio di pipeline di branching** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Le pipeline di elaborazione delle richieste OWIN non devono essere lineari, possono essere diramate per elaborare le richieste in modi diversi. Questo esempio illustra come creare una pipeline di branching basata su percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni. Questi componenti sono disponibili nel pacchetto NuGet Microsoft. Owin. mapping.

**Esempio di server personalizzato** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Viene illustrato come usare un server OWIN personalizzato quando si esegue l'hosting self-service OWIN.

**Esempio** di [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) | incorporato  
Alcuni server OWIN possono essere eseguiti all'interno del proprio processo (&quot;&quot;self-hosted). Questo esempio illustra come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto NuGet Microsoft. Owin. Hosting.

**Esempio HelloWorld** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN è un'astrazione API del server HTTP che consente la portabilità delle applicazioni in diversi server. In questo esempio viene illustrato come scrivere un'applicazione Hello World usando alcuni **semplici wrapper** per l'astrazione OWIN non elaborata ed eseguirla in un server web come ASP.NET.

**Esempio di Hello World OWIN Raw** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
Questo esempio illustra come scrivere un'applicazione Hello World usando l'astrazione OWIN non **elaborata** ed eseguirla in un server web come ASP.NET.

**Esempio di signalr** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Viene illustrato come ospitare autonomamente SignalR usando OWIN/Katana. Per altre informazioni sull'hosting automatico di SignalR, vedere [esercitazione: Self-host SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**File statici esempio** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Mostra come supportare le richieste HTTP per i file statici usando OWIN/Katana.

**API Web** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Questo esempio illustra come ospitare OWIN in IIS e aggiungere l'API Web alla pipeline OWIN.

Esempio di [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) | di **Web socket**   
Mostra come supportare i socket Web in OWIN usando la classe [System .NET. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .
