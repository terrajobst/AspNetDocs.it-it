---
uid: signalr/overview/older-versions/signalr-performance
title: Prestazioni di SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Prestazioni di SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579611"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="eeb43-103">Prestazioni di SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="eeb43-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="eeb43-104">di [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="eeb43-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="eeb43-105">Questo argomento descrive come progettare, misurare e migliorare le prestazioni in un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="eeb43-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="eeb43-106">Per una presentazione recente sulle prestazioni e il ridimensionamento di SignalR, vedere [ridimensionamento del Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="eeb43-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="eeb43-107">Questo argomento è suddiviso nelle sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="eeb43-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="eeb43-108">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="eeb43-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="eeb43-109">Ottimizzazione del server SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="eeb43-110">Risoluzione dei problemi di prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="eeb43-111">Uso dei contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="eeb43-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="eeb43-112">Utilizzo di altri contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="eeb43-113">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="eeb43-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="eeb43-114">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="eeb43-114">Design considerations</span></span>

<span data-ttu-id="eeb43-115">Questa sezione descrive i modelli che possono essere implementati durante la progettazione di un'applicazione SignalR, per garantire che le prestazioni non vengano ostacolate dalla generazione di traffico di rete non necessario.</span><span class="sxs-lookup"><span data-stu-id="eeb43-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="eeb43-116">Limitazione della frequenza del messaggio</span><span class="sxs-lookup"><span data-stu-id="eeb43-116">Throttling message frequency</span></span>

<span data-ttu-id="eeb43-117">Anche in un'applicazione che invia messaggi a frequenza elevata, ad esempio un'applicazione di gioco in tempo reale, la maggior parte delle applicazioni non deve inviare più di alcuni messaggi al secondo.</span><span class="sxs-lookup"><span data-stu-id="eeb43-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="eeb43-118">Per ridurre la quantità di traffico generato da ogni client, è possibile implementare un ciclo di messaggi che accoda e invia messaggi non più di frequente rispetto a una frequenza fissa, ovvero fino a un certo numero di messaggi che verranno inviati ogni secondo, se sono presenti messaggi nel tempo tervallo da inviare).</span><span class="sxs-lookup"><span data-stu-id="eeb43-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="eeb43-119">Per un'applicazione di esempio che limita i messaggi a una determinata velocità (sia dal client che dal server), vedere la pagina relativa alla [frequenza elevata in tempo reale con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="eeb43-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="eeb43-120">Riduzione della dimensione del messaggio</span><span class="sxs-lookup"><span data-stu-id="eeb43-120">Reducing message size</span></span>

<span data-ttu-id="eeb43-121">È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati.</span><span class="sxs-lookup"><span data-stu-id="eeb43-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="eeb43-122">Nel codice del server, se si invia un oggetto che contiene proprietà che non devono essere trasmesse, impedire che tali proprietà vengano serializzate tramite l'attributo `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="eeb43-123">Anche i nomi delle proprietà vengono archiviati nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando l'attributo `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="eeb43-124">Nell'esempio di codice riportato di seguito viene illustrato come escludere una proprietà da inviare al client e come abbreviare i nomi delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="eeb43-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="eeb43-125">**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati da inviare al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**</span><span class="sxs-lookup"><span data-stu-id="eeb43-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="eeb43-126">Per mantenere la leggibilità e la gestibilità nel codice client, è possibile rimappare i nomi abbreviati delle proprietà ai nomi descrittivi dopo la ricezione del messaggio.</span><span class="sxs-lookup"><span data-stu-id="eeb43-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="eeb43-127">Nell'esempio di codice seguente viene illustrata una possibile modalità di modifica del mapping dei nomi abbreviati a quelli più lunghi, definendo un contratto di messaggio (mapping) e utilizzando la funzione `reMap` per applicare il contratto alla classe messaggio ottimizzata:</span><span class="sxs-lookup"><span data-stu-id="eeb43-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="eeb43-128">**Codice JavaScript lato client che esegue il mapping dei nomi di proprietà abbreviati ai nomi leggibili**</span><span class="sxs-lookup"><span data-stu-id="eeb43-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="eeb43-129">I nomi possono essere abbreviati anche in messaggi dal client al server, usando lo stesso metodo.</span><span class="sxs-lookup"><span data-stu-id="eeb43-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="eeb43-130">La riduzione del footprint di memoria (ovvero la quantità di memoria utilizzata per il messaggio) dell'oggetto Message può migliorare anche le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="eeb43-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="eeb43-131">Se, ad esempio, l'intervallo completo di un `int` non è necessario, è possibile utilizzare un `short` o un `byte`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="eeb43-132">Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, la riduzione delle dimensioni dei messaggi può anche risolvere i problemi relativi alla memoria del server.</span><span class="sxs-lookup"><span data-stu-id="eeb43-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="eeb43-133">Ottimizzazione del server SignalR per le prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="eeb43-134">Le impostazioni di configurazione seguenti possono essere utilizzate per ottimizzare il server per ottenere prestazioni migliori in un'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="eeb43-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="eeb43-135">Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="eeb43-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="eeb43-136">**Impostazioni di configurazione di SignalR**</span><span class="sxs-lookup"><span data-stu-id="eeb43-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="eeb43-137">**DefaultMessageBufferSize**: per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per hub per connessione.</span><span class="sxs-lookup"><span data-stu-id="eeb43-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="eeb43-138">Se vengono usati messaggi di grandi dimensioni, è possibile che si verifichino problemi di memoria che possono essere attenuati riducendo questo valore.</span><span class="sxs-lookup"><span data-stu-id="eeb43-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="eeb43-139">Questa impostazione può essere impostata nel gestore dell'evento `Application_Start` in un'applicazione ASP.NET o nel metodo `Configuration` di una classe di avvio OWIN in un'applicazione indipendente.</span><span class="sxs-lookup"><span data-stu-id="eeb43-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="eeb43-140">Nell'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:</span><span class="sxs-lookup"><span data-stu-id="eeb43-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="eeb43-141">**Codice server .NET in Global. asax per la riduzione delle dimensioni predefinite del buffer dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="eeb43-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="eeb43-142">**Impostazioni di configurazione di IIS**</span><span class="sxs-lookup"><span data-stu-id="eeb43-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="eeb43-143">Numero **massimo di richieste simultanee per applicazione**: l'aumento del numero di richieste IIS simultanee aumenterà le risorse del server disponibili per soddisfare le richieste.</span><span class="sxs-lookup"><span data-stu-id="eeb43-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="eeb43-144">Il valore predefinito è 5000; per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:</span><span class="sxs-lookup"><span data-stu-id="eeb43-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="eeb43-145">**Impostazioni di configurazione di ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="eeb43-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="eeb43-146">Questa sezione include le impostazioni di configurazione che possono essere impostate nel file di `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="eeb43-147">Questo file si trova in una delle due posizioni, a seconda della piattaforma:</span><span class="sxs-lookup"><span data-stu-id="eeb43-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="eeb43-148">Le impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:</span><span class="sxs-lookup"><span data-stu-id="eeb43-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="eeb43-149">**Numero massimo di richieste simultanee per CPU**: aumentando questa impostazione è possibile ridurre i colli di bottiglia delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="eeb43-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="eeb43-150">Per aumentare questa impostazione, aggiungere l'impostazione di configurazione seguente al file di `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="eeb43-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="eeb43-151">**Limite coda richieste**: quando il numero totale di connessioni supera l'impostazione `maxConcurrentRequestsPerCPU`, ASP.NET avvierà la limitazione delle richieste tramite una coda.</span><span class="sxs-lookup"><span data-stu-id="eeb43-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="eeb43-152">Per aumentare le dimensioni della coda, è possibile aumentare l'impostazione della `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="eeb43-153">A tale scopo, aggiungere l'impostazione di configurazione seguente al nodo `processModel` in `config/machine.config` (anziché `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="eeb43-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="eeb43-154">Risoluzione dei problemi relativi alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="eeb43-155">In questa sezione vengono descritte le modalità di individuazione dei colli di bottiglia delle prestazioni nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eeb43-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="eeb43-156">Verifica per verificare se è in uso WebSocket</span><span class="sxs-lookup"><span data-stu-id="eeb43-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="eeb43-157">Mentre SignalR può usare un'ampia gamma di trasporti per la comunicazione tra client e server, WebSocket offre un notevole vantaggio in merito alle prestazioni e deve essere usato se il client e il server lo supportano.</span><span class="sxs-lookup"><span data-stu-id="eeb43-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="eeb43-158">Per determinare se il client e il server soddisfano i requisiti per WebSocket, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="eeb43-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="eeb43-159">Per determinare il trasporto usato nell'applicazione, è possibile usare gli strumenti di sviluppo del browser ed esaminare i log per visualizzare il trasporto usato per la connessione.</span><span class="sxs-lookup"><span data-stu-id="eeb43-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="eeb43-160">Per informazioni sull'utilizzo degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="eeb43-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="eeb43-161">Uso dei contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="eeb43-161">Using SignalR performance counters</span></span>

<span data-ttu-id="eeb43-162">Questa sezione descrive come abilitare e usare i contatori delle prestazioni di SignalR, disponibili nel pacchetto di `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="eeb43-163">Installazione di SignalR. exe</span><span class="sxs-lookup"><span data-stu-id="eeb43-163">Installing signalr.exe</span></span>

<span data-ttu-id="eeb43-164">I contatori delle prestazioni possono essere aggiunti al server utilizzando un'utilità denominata SignalR. exe.</span><span class="sxs-lookup"><span data-stu-id="eeb43-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="eeb43-165">Per installare questa utilità, attenersi alla seguente procedura:</span><span class="sxs-lookup"><span data-stu-id="eeb43-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="eeb43-166">In Visual Studio selezionare **strumenti** > **gestione pacchetti NuGet** > **Gestisci pacchetti NuGet per la soluzione**</span><span class="sxs-lookup"><span data-stu-id="eeb43-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="eeb43-167">Cercare **SignalR. utils**e selezionare Install (installa).</span><span class="sxs-lookup"><span data-stu-id="eeb43-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="eeb43-168">Accettare il contratto di licenza per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="eeb43-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="eeb43-169">SignalR. exe verrà installato per `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="eeb43-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="eeb43-170">Installazione dei contatori delle prestazioni con SignalR. exe</span><span class="sxs-lookup"><span data-stu-id="eeb43-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="eeb43-171">Per installare i contatori delle prestazioni di SignalR, eseguire SignalR. exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="eeb43-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="eeb43-172">Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR. exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="eeb43-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="eeb43-173">Contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="eeb43-173">SignalR Performance counters</span></span>

<span data-ttu-id="eeb43-174">Il pacchetto Utilities installa i contatori delle prestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="eeb43-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="eeb43-175">I contatori "Total" misurano il numero di eventi dopo l'ultimo riavvio del pool di applicazioni o del server.</span><span class="sxs-lookup"><span data-stu-id="eeb43-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="eeb43-176">**Metriche di connessione**</span><span class="sxs-lookup"><span data-stu-id="eeb43-176">**Connection metrics**</span></span>

<span data-ttu-id="eeb43-177">Le metriche seguenti misurano gli eventi di durata della connessione che si verificano.</span><span class="sxs-lookup"><span data-stu-id="eeb43-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="eeb43-178">Per ulteriori informazioni, vedere informazioni [e gestione degli eventi di durata della connessione](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="eeb43-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="eeb43-179">**Connessioni connesse**</span><span class="sxs-lookup"><span data-stu-id="eeb43-179">**Connections Connected**</span></span>
- <span data-ttu-id="eeb43-180">**Connessioni riconnesse**</span><span class="sxs-lookup"><span data-stu-id="eeb43-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="eeb43-181">**Connessioni disconnesse**</span><span class="sxs-lookup"><span data-stu-id="eeb43-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="eeb43-182">**Connessioni correnti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-182">**Connections Current**</span></span>

<span data-ttu-id="eeb43-183">**Metriche del messaggio**</span><span class="sxs-lookup"><span data-stu-id="eeb43-183">**Message metrics**</span></span>

<span data-ttu-id="eeb43-184">Le metriche seguenti misurano il traffico dei messaggi generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="eeb43-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="eeb43-185">**Totale messaggi di connessione ricevuti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="eeb43-186">**Totale messaggi di connessione inviati**</span><span class="sxs-lookup"><span data-stu-id="eeb43-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="eeb43-187">**Messaggi di connessione ricevuti/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="eeb43-188">**Messaggi di connessione inviati/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="eeb43-189">**Metriche del bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="eeb43-189">**Message bus metrics**</span></span>

<span data-ttu-id="eeb43-190">Le metriche seguenti misurano il traffico attraverso il bus di messaggi interno SignalR, la coda in cui vengono inseriti tutti i messaggi di SignalR in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="eeb43-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="eeb43-191">Un messaggio viene **pubblicato** quando viene inviato o trasmesso.</span><span class="sxs-lookup"><span data-stu-id="eeb43-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="eeb43-192">Un **Sottoscrittore** in questo contesto è una sottoscrizione sul bus di messaggi. deve corrispondere al numero di client e al server stesso.</span><span class="sxs-lookup"><span data-stu-id="eeb43-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="eeb43-193">Un ruolo di **lavoro allocato** è un componente che invia dati alle connessioni attive. un ruolo di **lavoro occupato** è quello che invia attivamente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="eeb43-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="eeb43-194">**Totale messaggi bus di messaggi ricevuti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="eeb43-195">**Messaggi bus di messaggi ricevuti/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="eeb43-196">**Totale messaggi del bus di messaggi pubblicati**</span><span class="sxs-lookup"><span data-stu-id="eeb43-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="eeb43-197">**Messaggi del bus di messaggi pubblicati/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="eeb43-198">**Sottoscrittori del bus di messaggi correnti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="eeb43-199">**Totale sottoscrittori del bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="eeb43-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="eeb43-200">**Sottoscrittori del bus di messaggi/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="eeb43-201">**Thread di lavoro allocati dal bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="eeb43-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="eeb43-202">**Thread di lavoro occupati del bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="eeb43-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="eeb43-203">**Argomenti del bus di messaggi correnti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="eeb43-204">**Metriche degli errori**</span><span class="sxs-lookup"><span data-stu-id="eeb43-204">**Error metrics**</span></span>

<span data-ttu-id="eeb43-205">Le metriche seguenti misurano gli errori generati dal traffico dei messaggi di SignalR.</span><span class="sxs-lookup"><span data-stu-id="eeb43-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="eeb43-206">Gli errori di **risoluzione dell'hub** si verificano quando non è possibile risolvere un hub o un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="eeb43-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="eeb43-207">Gli errori di **chiamata dell'hub** sono eccezioni generate durante la chiamata di un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="eeb43-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="eeb43-208">Gli errori di **trasporto** sono errori di connessione generati durante una richiesta o una risposta http.</span><span class="sxs-lookup"><span data-stu-id="eeb43-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="eeb43-209">**Errori: totale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-209">**Errors: All Total**</span></span>
- <span data-ttu-id="eeb43-210">**Errori: tutti/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="eeb43-211">**Errori: totale risoluzione Hub**</span><span class="sxs-lookup"><span data-stu-id="eeb43-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="eeb43-212">**Errori: risoluzione Hub/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="eeb43-213">**Errori: totale chiamate Hub**</span><span class="sxs-lookup"><span data-stu-id="eeb43-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="eeb43-214">**Errori: chiamata dell'hub/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="eeb43-215">**Errori: totale trasporto**</span><span class="sxs-lookup"><span data-stu-id="eeb43-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="eeb43-216">**Errori: trasporto/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="eeb43-217">**Metriche di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-217">**Scaleout metrics**</span></span>

<span data-ttu-id="eeb43-218">Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale.</span><span class="sxs-lookup"><span data-stu-id="eeb43-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="eeb43-219">Un **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene usato SQL Server, un argomento se si usa il bus di servizio e una sottoscrizione se si usa Redis.</span><span class="sxs-lookup"><span data-stu-id="eeb43-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="eeb43-220">Per impostazione predefinita, viene usato solo un flusso, ma è possibile aumentarlo tramite la configurazione SQL Server e il bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="eeb43-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="eeb43-221">Un flusso di **buffering** è uno che è entrato in uno stato di errore; Quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avranno esito negativo immediatamente finché il flusso non è più in errore.</span><span class="sxs-lookup"><span data-stu-id="eeb43-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="eeb43-222">La **lunghezza della coda di invio** è il numero di messaggi inviati ma non ancora inviati.</span><span class="sxs-lookup"><span data-stu-id="eeb43-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="eeb43-223">**Messaggi del bus di messaggi di scalabilità orizzontale ricevuti/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="eeb43-224">**Totale flussi di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="eeb43-225">**Flussi di scalabilità orizzontale aperti**</span><span class="sxs-lookup"><span data-stu-id="eeb43-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="eeb43-226">**Buffering dei flussi di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="eeb43-227">**Totale errori di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="eeb43-228">**Errori di scalabilità orizzontale/sec**</span><span class="sxs-lookup"><span data-stu-id="eeb43-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="eeb43-229">**Lunghezza coda di invio con scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="eeb43-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="eeb43-230">Per altre informazioni su ciò che questi contatori stanno misurando, vedere [scalabilità orizzontale di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="eeb43-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="eeb43-231">Utilizzo di altri contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="eeb43-231">Using other performance counters</span></span>

<span data-ttu-id="eeb43-232">I contatori delle prestazioni seguenti possono essere utili anche per monitorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eeb43-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="eeb43-233">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="eeb43-233">**Memory**</span></span>

- <span data-ttu-id="eeb43-234">Memoria CLR .NET byte in tutti gli heap (per w3wp)</span><span class="sxs-lookup"><span data-stu-id="eeb43-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="eeb43-235">**ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="eeb43-235">**ASP.NET**</span></span>

- <span data-ttu-id="eeb43-236">ASP. Net\richieste corrente</span><span class="sxs-lookup"><span data-stu-id="eeb43-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="eeb43-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="eeb43-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="eeb43-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="eeb43-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="eeb43-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="eeb43-239">**CPU**</span></span>

- <span data-ttu-id="eeb43-240">Tempo di Information\Processor processore</span><span class="sxs-lookup"><span data-stu-id="eeb43-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="eeb43-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="eeb43-241">**TCP/IP**</span></span>

- <span data-ttu-id="eeb43-242">TCPv6/connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="eeb43-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="eeb43-243">TCPv4/connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="eeb43-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="eeb43-244">**Servizio Web**</span><span class="sxs-lookup"><span data-stu-id="eeb43-244">**Web Service**</span></span>

- <span data-ttu-id="eeb43-245">Connessioni Web\connessioni Web</span><span class="sxs-lookup"><span data-stu-id="eeb43-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="eeb43-246">Connessioni Service\Maximum Web</span><span class="sxs-lookup"><span data-stu-id="eeb43-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="eeb43-247">**Threading**</span><span class="sxs-lookup"><span data-stu-id="eeb43-247">**Threading**</span></span>

- <span data-ttu-id="eeb43-248">LocksAndThreads CLR .NET\# dei thread logici correnti</span><span class="sxs-lookup"><span data-stu-id="eeb43-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="eeb43-249">Thread LocksAnd CLR .NET\# dei thread fisici correnti</span><span class="sxs-lookup"><span data-stu-id="eeb43-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="eeb43-250">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="eeb43-250">Other Resources</span></span>

<span data-ttu-id="eeb43-251">Per ulteriori informazioni sul monitoraggio e l'ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="eeb43-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="eeb43-252">[Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="eeb43-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="eeb43-253">Utilizzo di thread ASP.NET in IIS 7,5, IIS 7,0 e IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="eeb43-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="eeb43-254">&lt;elemento&gt; applicationPool (impostazioni Web)</span><span class="sxs-lookup"><span data-stu-id="eeb43-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
