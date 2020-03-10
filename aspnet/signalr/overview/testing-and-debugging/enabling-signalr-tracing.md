---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Abilitazione della traccia di SignalR | Microsoft Docs
author: bradygaster
description: Questo documento descrive come abilitare e configurare la traccia per i server e i client SignalR. La traccia consente di visualizzare le informazioni di diagnostica sugli eventi...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578834"
---
# <a name="enabling-signalr-tracing"></a>Abilitazione della traccia di SignalR

di [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento descrive come abilitare e configurare la traccia per i server e i client SignalR. La traccia consente di visualizzare le informazioni diagnostiche sugli eventi nell'applicazione SignalR.
>
> Questo argomento è stato scritto in origine da Patrick Fletcher.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - SignalR versione 2
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

Quando la traccia è abilitata, un'applicazione SignalR crea voci di log per gli eventi. È possibile registrare gli eventi sia dal client che dal server. Traccia sulla connessione dei log del server, sul provider di scalabilità orizzontale e sugli eventi del bus di messaggi. La traccia nel client registra gli eventi di connessione. In SignalR 2,1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata dell'hub.

## <a name="contents"></a>Contenuto

- [Abilitazione della traccia nel server](#server)

    - [Registrazione degli eventi del server in file di testo](#server_text)
    - [Registrazione degli eventi del server nel registro eventi](#server_eventlog)
- [Abilitazione della traccia nel client .NET (app desktop di Windows)](#net_client)

    - [Registrazione degli eventi del client desktop nella console](#desktop_console)
    - [Registrazione degli eventi del client desktop in un file di testo](#desktop_text)
- [Abilitazione della traccia nei client Windows Phone 8](#phone)

    - [Registrazione di Windows Phone eventi client nell'interfaccia utente](#phone_ui)
    - [Registrazione di Windows Phone eventi client nella console di debug](#phone_debug)
- [Abilitazione della traccia nel client JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Abilitazione della traccia nel server

È possibile abilitare la traccia nel server nel file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto). Specificare le categorie di eventi che si desidera registrare. Nel file di configurazione è inoltre possibile specificare se registrare gli eventi in un file di testo, nel registro eventi di Windows o in un log personalizzato utilizzando un'implementazione di [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Le categorie di eventi del server includono i seguenti tipi di messaggi:

| Origine | Messaggi |
| --- | --- |
| SignalR.SqlMessageBus | Impostazione del provider di scalabilità del bus di messaggi SQL, operazione di database, errori ed eventi di timeout |
| SignalR.ServiceBusMessageBus | Creazione di argomenti del provider di scalabilità orizzontale del bus di servizio e sottoscrizione, errori ed eventi di messaggistica |
| SignalR.RedisMessageBus | Connessione del provider di scalabilità orizzontale Redis, disconnessione ed eventi di errore |
| SignalR.ScaleoutMessageBus | Eventi di messaggistica con scalabilità orizzontale |
| SignalR.Transports.WebSocketTransport | Connessione di trasporto WebSocket, disconnessione, messaggistica ed eventi di errore |
| SignalR.Transports.ServerSentEventsTransport | Connessione trasporto ServerSentEvents, disconnessione, messaggistica ed eventi di errore |
| SignalR.Transports.ForeverFrameTransport | Connessione trasporto ForeverFrame, disconnessione, messaggistica ed eventi di errore |
| SignalR.Transports.LongPollingTransport | Connessione trasporto LongPolling, disconnessione, messaggistica ed eventi di errore |
| SignalR.Transports.TransportHeartBeat | Eventi di connessione di trasporto, disconnessione e KeepAlive |
| SignalR.ReflectedHubDescriptorProvider | Eventi di individuazione Hub |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registrazione degli eventi del server in file di testo

Nel codice seguente viene illustrato come abilitare la traccia per ogni categoria di evento. In questo esempio l'applicazione viene configurata per registrare gli eventi in file di testo.

**Codice server XML per l'abilitazione della traccia**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Nel codice precedente la voce `SignalRSwitch` specifica l'oggetto [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usato per gli eventi inviati al log specificato. In questo caso, è impostato su `Verbose`, che indica che tutti i messaggi di debug e di traccia vengono registrati.

L'output seguente mostra le voci del file di `transports.log.txt` per un'applicazione usando il file di configurazione precedente. Mostra una nuova connessione, una connessione rimossa e gli eventi heartbeat del trasporto.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registrazione degli eventi del server nel registro eventi

Per registrare gli eventi nel registro eventi anziché in un file di testo, modificare i valori per le voci nel nodo `sharedListeners`. Nel codice seguente viene illustrato come registrare gli eventi del server nel registro eventi:

**Codice server XML per la registrazione degli eventi nel registro eventi**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Gli eventi vengono registrati nel registro applicazioni e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:

![Visualizzatore eventi visualizzazione dei log di SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Quando si utilizza il registro eventi, impostare **TraceLevel** su **errore** per limitare il numero di messaggi gestibili.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Abilitazione della traccia nel client .NET (app desktop di Windows)

Il client .NET può registrare gli eventi nella console, in un file di testo o in un log personalizzato usando un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Per abilitare la registrazione nel client .NET, impostare la proprietà `TraceLevel` della connessione su un valore [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) e la proprietà `TraceWriter` su un'istanza [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) valida.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registrazione degli eventi del client desktop nella console

Il codice C# seguente illustra come registrare gli eventi nel client .NET nella console:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registrazione degli eventi del client desktop in un file di testo

Il codice C# seguente illustra come registrare gli eventi nel client .NET in un file di testo:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

L'output seguente mostra le voci del file di `ClientLog.txt` per un'applicazione usando il file di configurazione precedente. Mostra il client che si connette al server e l'hub che richiama un metodo client denominato `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Abilitazione della traccia nei client Windows Phone 8

Le applicazioni SignalR per le app Windows Phone usano lo stesso client .NET come app desktop, ma [Console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili. Al contrario, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registrazione di Windows Phone eventi client nell'interfaccia utente

La [codebase di SignalR](https://github.com/SignalR/SignalR/archive/master.zip) include un Windows Phone esempio che scrive l'output di traccia in un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un'implementazione [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personalizzata denominata `TextBlockWriter`. Questa classe si trova nel progetto **Samples/Microsoft. AspNet. SignalR. client. WP8. Samples** . Quando si crea un'istanza di `TextBlockWriter`, passare l'oggetto [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)corrente e un elemento [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da usare per l'output di traccia:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

L'output di traccia verrà quindi scritto in un nuovo elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creato nell'elemento [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) passato:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registrazione di Windows Phone eventi client nella console di debug

Per inviare l'output alla console di debug anziché all'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarla alla proprietà [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) della connessione:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Le informazioni di traccia verranno quindi scritte nella finestra di debug di Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Abilitazione della traccia nel client JavaScript

Per abilitare la registrazione lato client su una connessione, impostare la proprietà `logging` sull'oggetto connessione prima di chiamare il metodo `start` per stabilire la connessione.

**Codice JavaScript client per l'abilitazione della traccia nella console del browser (con il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Codice JavaScript client per abilitare la traccia nella console del browser (senza il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Quando la traccia è abilitata, il client JavaScript registra gli eventi nella console del browser. Per accedere alla console del browser, vedere [monitoraggio dei trasporti](../getting-started/introduction-to-signalr.md#MonitoringTransports).

La schermata seguente mostra un client JavaScript SignalR con la traccia abilitata. Mostra gli eventi di connessione e di chiamata dell'hub nella console del browser:

![Eventi di traccia di SignalR nella console del browser](enabling-signalr-tracing/_static/image4.png)
