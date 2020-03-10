---
uid: signalr/overview/performance/scaleout-in-signalr
title: Introduzione alla scalabilità orizzontale in SignalR | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14dc22f99a43b566903c59fb23b7d419350f4a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579233"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Introduzione a scale-out in SignalR

di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

In generale, esistono due modi *per ridimensionare*un'applicazione Web: *scalabilità verticale* e orizzontale.

- La scalabilità verticale indica l'uso di un server più grande (o di una VM più grande) con più RAM, CPU e così via.
- Scale out significa aggiungere altri server per gestire il carico.

Il problema con la scalabilità verticale è che si raggiunge rapidamente un limite per le dimensioni del computer. Oltre a questo, è necessario aumentare il livello di scalabilità orizzontale. Tuttavia, quando si aumenta il livello di scalabilità orizzontale, i client possono essere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

![](scaleout-in-signalr/_static/image1.png)

Una soluzione consiste nell'inviare messaggi tra server, usando un componente denominato *backplane*. Con un backplane abilitato, ogni istanza dell'applicazione invia messaggi al backplane e il backplane li inoltra alle altre istanze dell'applicazione. (In elettronica, un backplane è un gruppo di connettori paralleli. Analogamente, un backplane SignalR connette più server.

![](scaleout-in-signalr/_static/image2.png)

SignalR fornisce attualmente tre BACKPLAN:

- **Bus di servizio di Azure**. Il bus di servizio è un'infrastruttura di messaggistica che consente ai componenti di inviare messaggi in modo a regime di controllo libero.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello di pubblicazione/sottoscrizione ("pub/sub") per l'invio di messaggi.
- **SQL Server**. Il backplane SQL Server scrive i messaggi nelle tabelle SQL. Il backplane USA Service Broker per la messaggistica efficiente. Tuttavia, funziona anche se Service Broker non è abilitato.

Se si distribuisce l'applicazione in Azure, è consigliabile usare il backplane Redis con [cache Redis di Azure](https://azure.microsoft.com/services/cache/). Se si esegue la distribuzione in una server farm personalizzata, prendere in considerazione i BACKPLAN SQL Server o Redis.

Negli argomenti seguenti sono incluse esercitazioni dettagliate per ogni backplane:

- [Scalabilità orizzontale di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md)
- [Scalabilità orizzontale di SignalR con Redis](scaleout-with-redis.md)
- [Scalabilità orizzontale di SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementazione

In SignalR ogni messaggio viene inviato tramite un bus di messaggi. Un bus di messaggi implementa l'interfaccia [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , che fornisce un'astrazione di pubblicazione/sottoscrizione. Il backplans funziona sostituendo il **IMessageBus** predefinito con un bus progettato per tale backplane. Ad esempio, il bus di messaggi per Redis è [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)e usa il meccanismo di [pubblicazione/sottoscrizione](http://redis.io/topics/pubsub) di redis per inviare e ricevere messaggi.

Ogni istanza del server si connette al backplane tramite il bus. Quando un messaggio viene inviato, passa al backplane e il backplane lo invia a ogni server. Quando un server riceve un messaggio dal backplane, inserisce il messaggio nella cache locale. Il server recapita quindi i messaggi ai client dalla cache locale.

Per ogni connessione client, lo stato di avanzamento del client durante la lettura del flusso di messaggi viene rilevata utilizzando un cursore. (Un cursore rappresenta una posizione nel flusso di messaggi). Se un client si disconnette e quindi riconnette, chiede al bus i messaggi ricevuti dopo il valore di cursore del client. La stessa cosa si verifica quando una connessione usa il [polling prolungato](../getting-started/introduction-to-signalr.md#transports). Al termine di una richiesta di polling lungo, il client apre una nuova connessione e chiede i messaggi ricevuti dopo il cursore.

Il meccanismo del cursore funziona anche se un client viene indirizzato a un server diverso in caso di riconnessione. Il backplane è a conoscenza di tutti i server e non è rilevante a quale server si connette il client.

## <a name="limitations"></a>Limitazioni

Utilizzando un backplane, la velocità effettiva massima dei messaggi è inferiore a quella in cui i client comunicano direttamente con un singolo nodo del server. Questo perché il backplane trasmette ogni messaggio a ogni nodo, quindi il backplane può diventare un collo di bottiglia. Se questa limitazione costituisce un problema dipende dall'applicazione. Ad esempio, di seguito sono riportati alcuni scenari tipici di SignalR:

- [Broadcast del server](../getting-started/tutorial-server-broadcast-with-signalr.md) (ad esempio, Stock): i BACKPLAN sono adatti a questo scenario, perché il server controlla la velocità con cui vengono inviati i messaggi.
- [Da client a client](../getting-started/tutorial-getting-started-with-signalr.md) (ad esempio, chat): in questo scenario, il backplane può costituire un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client; ovvero, se la frequenza dei messaggi aumenta proporzionalmente Man mano che si aggiungono altri client.
- [Tempo reale ad alta frequenza](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (ad esempio, giochi in tempo reale): un backplane non è consigliato per questo scenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Abilitazione della traccia per la scalabilità orizzontale di SignalR

Per abilitare la traccia per i BACKPLAN, aggiungere le sezioni seguenti al file Web. config, sotto l'elemento di **configurazione** radice:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
