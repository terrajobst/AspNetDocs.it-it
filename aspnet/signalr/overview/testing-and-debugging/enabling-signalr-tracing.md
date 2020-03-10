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
# <a name="enabling-signalr-tracing"></a><span data-ttu-id="45872-104">Abilitazione della traccia di SignalR</span><span class="sxs-lookup"><span data-stu-id="45872-104">Enabling SignalR Tracing</span></span>

<span data-ttu-id="45872-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="45872-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="45872-106">Questo documento descrive come abilitare e configurare la traccia per i server e i client SignalR.</span><span class="sxs-lookup"><span data-stu-id="45872-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="45872-107">La traccia consente di visualizzare le informazioni diagnostiche sugli eventi nell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="45872-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="45872-108">Questo argomento è stato scritto in origine da Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="45872-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="45872-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="45872-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="45872-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="45872-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="45872-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="45872-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="45872-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="45872-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="45872-113">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="45872-113">Questions and comments</span></span>
>
> <span data-ttu-id="45872-114">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="45872-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="45872-115">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="45872-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="45872-116">Quando la traccia è abilitata, un'applicazione SignalR crea voci di log per gli eventi.</span><span class="sxs-lookup"><span data-stu-id="45872-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="45872-117">È possibile registrare gli eventi sia dal client che dal server.</span><span class="sxs-lookup"><span data-stu-id="45872-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="45872-118">Traccia sulla connessione dei log del server, sul provider di scalabilità orizzontale e sugli eventi del bus di messaggi.</span><span class="sxs-lookup"><span data-stu-id="45872-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="45872-119">La traccia nel client registra gli eventi di connessione.</span><span class="sxs-lookup"><span data-stu-id="45872-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="45872-120">In SignalR 2,1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata dell'hub.</span><span class="sxs-lookup"><span data-stu-id="45872-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="45872-121">Contenuto</span><span class="sxs-lookup"><span data-stu-id="45872-121">Contents</span></span>

- [<span data-ttu-id="45872-122">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="45872-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="45872-123">Registrazione degli eventi del server in file di testo</span><span class="sxs-lookup"><span data-stu-id="45872-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="45872-124">Registrazione degli eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="45872-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="45872-125">Abilitazione della traccia nel client .NET (app desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="45872-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="45872-126">Registrazione degli eventi del client desktop nella console</span><span class="sxs-lookup"><span data-stu-id="45872-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="45872-127">Registrazione degli eventi del client desktop in un file di testo</span><span class="sxs-lookup"><span data-stu-id="45872-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="45872-128">Abilitazione della traccia nei client Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="45872-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="45872-129">Registrazione di Windows Phone eventi client nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="45872-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="45872-130">Registrazione di Windows Phone eventi client nella console di debug</span><span class="sxs-lookup"><span data-stu-id="45872-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="45872-131">Abilitazione della traccia nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="45872-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="45872-132">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="45872-132">Enabling tracing on the server</span></span>

<span data-ttu-id="45872-133">È possibile abilitare la traccia nel server nel file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto). Specificare le categorie di eventi che si desidera registrare.</span><span class="sxs-lookup"><span data-stu-id="45872-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="45872-134">Nel file di configurazione è inoltre possibile specificare se registrare gli eventi in un file di testo, nel registro eventi di Windows o in un log personalizzato utilizzando un'implementazione di [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="45872-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="45872-135">Le categorie di eventi del server includono i seguenti tipi di messaggi:</span><span class="sxs-lookup"><span data-stu-id="45872-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="45872-136">Origine</span><span class="sxs-lookup"><span data-stu-id="45872-136">Source</span></span> | <span data-ttu-id="45872-137">Messaggi</span><span class="sxs-lookup"><span data-stu-id="45872-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="45872-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="45872-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="45872-139">Impostazione del provider di scalabilità del bus di messaggi SQL, operazione di database, errori ed eventi di timeout</span><span class="sxs-lookup"><span data-stu-id="45872-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="45872-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="45872-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="45872-141">Creazione di argomenti del provider di scalabilità orizzontale del bus di servizio e sottoscrizione, errori ed eventi di messaggistica</span><span class="sxs-lookup"><span data-stu-id="45872-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="45872-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="45872-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="45872-143">Connessione del provider di scalabilità orizzontale Redis, disconnessione ed eventi di errore</span><span class="sxs-lookup"><span data-stu-id="45872-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="45872-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="45872-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="45872-145">Eventi di messaggistica con scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="45872-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="45872-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="45872-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="45872-147">Connessione di trasporto WebSocket, disconnessione, messaggistica ed eventi di errore</span><span class="sxs-lookup"><span data-stu-id="45872-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="45872-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="45872-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="45872-149">Connessione trasporto ServerSentEvents, disconnessione, messaggistica ed eventi di errore</span><span class="sxs-lookup"><span data-stu-id="45872-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="45872-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="45872-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="45872-151">Connessione trasporto ForeverFrame, disconnessione, messaggistica ed eventi di errore</span><span class="sxs-lookup"><span data-stu-id="45872-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="45872-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="45872-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="45872-153">Connessione trasporto LongPolling, disconnessione, messaggistica ed eventi di errore</span><span class="sxs-lookup"><span data-stu-id="45872-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="45872-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="45872-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="45872-155">Eventi di connessione di trasporto, disconnessione e KeepAlive</span><span class="sxs-lookup"><span data-stu-id="45872-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="45872-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="45872-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="45872-157">Eventi di individuazione Hub</span><span class="sxs-lookup"><span data-stu-id="45872-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="45872-158">Registrazione degli eventi del server in file di testo</span><span class="sxs-lookup"><span data-stu-id="45872-158">Logging server events to text files</span></span>

<span data-ttu-id="45872-159">Nel codice seguente viene illustrato come abilitare la traccia per ogni categoria di evento.</span><span class="sxs-lookup"><span data-stu-id="45872-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="45872-160">In questo esempio l'applicazione viene configurata per registrare gli eventi in file di testo.</span><span class="sxs-lookup"><span data-stu-id="45872-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="45872-161">**Codice server XML per l'abilitazione della traccia**</span><span class="sxs-lookup"><span data-stu-id="45872-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="45872-162">Nel codice precedente la voce `SignalRSwitch` specifica l'oggetto [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usato per gli eventi inviati al log specificato.</span><span class="sxs-lookup"><span data-stu-id="45872-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="45872-163">In questo caso, è impostato su `Verbose`, che indica che tutti i messaggi di debug e di traccia vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="45872-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="45872-164">L'output seguente mostra le voci del file di `transports.log.txt` per un'applicazione usando il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="45872-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="45872-165">Mostra una nuova connessione, una connessione rimossa e gli eventi heartbeat del trasporto.</span><span class="sxs-lookup"><span data-stu-id="45872-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="45872-166">Registrazione degli eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="45872-166">Logging server events to the event log</span></span>

<span data-ttu-id="45872-167">Per registrare gli eventi nel registro eventi anziché in un file di testo, modificare i valori per le voci nel nodo `sharedListeners`.</span><span class="sxs-lookup"><span data-stu-id="45872-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="45872-168">Nel codice seguente viene illustrato come registrare gli eventi del server nel registro eventi:</span><span class="sxs-lookup"><span data-stu-id="45872-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="45872-169">**Codice server XML per la registrazione degli eventi nel registro eventi**</span><span class="sxs-lookup"><span data-stu-id="45872-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="45872-170">Gli eventi vengono registrati nel registro applicazioni e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="45872-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Visualizzatore eventi visualizzazione dei log di SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="45872-172">Quando si utilizza il registro eventi, impostare **TraceLevel** su **errore** per limitare il numero di messaggi gestibili.</span><span class="sxs-lookup"><span data-stu-id="45872-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="45872-173">Abilitazione della traccia nel client .NET (app desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="45872-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="45872-174">Il client .NET può registrare gli eventi nella console, in un file di testo o in un log personalizzato usando un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="45872-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="45872-175">Per abilitare la registrazione nel client .NET, impostare la proprietà `TraceLevel` della connessione su un valore [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) e la proprietà `TraceWriter` su un'istanza [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) valida.</span><span class="sxs-lookup"><span data-stu-id="45872-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="45872-176">Registrazione degli eventi del client desktop nella console</span><span class="sxs-lookup"><span data-stu-id="45872-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="45872-177">Il codice C# seguente illustra come registrare gli eventi nel client .NET nella console:</span><span class="sxs-lookup"><span data-stu-id="45872-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="45872-178">Registrazione degli eventi del client desktop in un file di testo</span><span class="sxs-lookup"><span data-stu-id="45872-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="45872-179">Il codice C# seguente illustra come registrare gli eventi nel client .NET in un file di testo:</span><span class="sxs-lookup"><span data-stu-id="45872-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="45872-180">L'output seguente mostra le voci del file di `ClientLog.txt` per un'applicazione usando il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="45872-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="45872-181">Mostra il client che si connette al server e l'hub che richiama un metodo client denominato `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="45872-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="45872-182">Abilitazione della traccia nei client Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="45872-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="45872-183">Le applicazioni SignalR per le app Windows Phone usano lo stesso client .NET come app desktop, ma [Console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="45872-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="45872-184">Al contrario, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia.</span><span class="sxs-lookup"><span data-stu-id="45872-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="45872-185">Registrazione di Windows Phone eventi client nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="45872-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="45872-186">La [codebase di SignalR](https://github.com/SignalR/SignalR/archive/master.zip) include un Windows Phone esempio che scrive l'output di traccia in un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un'implementazione [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personalizzata denominata `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="45872-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="45872-187">Questa classe si trova nel progetto **Samples/Microsoft. AspNet. SignalR. client. WP8. Samples** .</span><span class="sxs-lookup"><span data-stu-id="45872-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="45872-188">Quando si crea un'istanza di `TextBlockWriter`, passare l'oggetto [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)corrente e un elemento [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da usare per l'output di traccia:</span><span class="sxs-lookup"><span data-stu-id="45872-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="45872-189">L'output di traccia verrà quindi scritto in un nuovo elemento [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creato nell'elemento [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) passato:</span><span class="sxs-lookup"><span data-stu-id="45872-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="45872-190">Registrazione di Windows Phone eventi client nella console di debug</span><span class="sxs-lookup"><span data-stu-id="45872-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="45872-191">Per inviare l'output alla console di debug anziché all'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarla alla proprietà [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) della connessione:</span><span class="sxs-lookup"><span data-stu-id="45872-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="45872-192">Le informazioni di traccia verranno quindi scritte nella finestra di debug di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="45872-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="45872-193">Abilitazione della traccia nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="45872-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="45872-194">Per abilitare la registrazione lato client su una connessione, impostare la proprietà `logging` sull'oggetto connessione prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="45872-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="45872-195">**Codice JavaScript client per l'abilitazione della traccia nella console del browser (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="45872-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="45872-196">**Codice JavaScript client per abilitare la traccia nella console del browser (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="45872-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="45872-197">Quando la traccia è abilitata, il client JavaScript registra gli eventi nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="45872-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="45872-198">Per accedere alla console del browser, vedere [monitoraggio dei trasporti](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="45872-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="45872-199">La schermata seguente mostra un client JavaScript SignalR con la traccia abilitata.</span><span class="sxs-lookup"><span data-stu-id="45872-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="45872-200">Mostra gli eventi di connessione e di chiamata dell'hub nella console del browser:</span><span class="sxs-lookup"><span data-stu-id="45872-200">It shows connection and hub invocation events in the browser console:</span></span>

![Eventi di traccia di SignalR nella console del browser](enabling-signalr-tracing/_static/image4.png)
