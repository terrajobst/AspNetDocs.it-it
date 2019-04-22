---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guida all'API di ASP.NET SignalR Hubs - Client .NET (c#) | Microsoft Docs
author: bradygaster
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per la versione 2 nel client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e gli svantaggi di SignalR...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 473c8dd14d639fb9f4ff9e11a4c3ffa2b1a3a81e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59396031"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="5d729-103">Guida all'API di ASP.NET SignalR Hubs - Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="5d729-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>


[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5d729-104">Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client .NET, ad esempio applicazioni console, WPF, Silverlight e Windows Store (WinRT).</span><span class="sxs-lookup"><span data-stu-id="5d729-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="5d729-105">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="5d729-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="5d729-106">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="5d729-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="5d729-107">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="5d729-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="5d729-108">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="5d729-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="5d729-109">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="5d729-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="5d729-110">Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5d729-111">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="5d729-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5d729-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5d729-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="5d729-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5d729-113">.NET 4.5</span></span>
> - <span data-ttu-id="5d729-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="5d729-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5d729-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="5d729-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5d729-116">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5d729-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="5d729-117">Questions and comments</span></span>
>
> <span data-ttu-id="5d729-118">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="5d729-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5d729-119">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5d729-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="5d729-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5d729-120">Overview</span></span>

<span data-ttu-id="5d729-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="5d729-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="5d729-122">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="5d729-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="5d729-123">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="5d729-124">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="5d729-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="5d729-125">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="5d729-126">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="5d729-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="5d729-127">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="5d729-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="5d729-128">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="5d729-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="5d729-129">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="5d729-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="5d729-130">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="5d729-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="5d729-131">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="5d729-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="5d729-132">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="5d729-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="5d729-133">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="5d729-134">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="5d729-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="5d729-135">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="5d729-136">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="5d729-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="5d729-137">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="5d729-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="5d729-138">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="5d729-139">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="5d729-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="5d729-140">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="5d729-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="5d729-141">Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="5d729-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="5d729-142">Per un progetto di client .NET di esempio, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="5d729-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="5d729-143">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) in GitHub.com (esempi di app console di WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="5d729-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="5d729-144">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) in GitHub.com (ad esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="5d729-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="5d729-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app Console).</span><span class="sxs-lookup"><span data-stu-id="5d729-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="5d729-146">Per informazioni su come programmare il server o client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="5d729-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="5d729-147">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="5d729-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="5d729-148">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="5d729-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="5d729-149">I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d729-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="5d729-150">Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5d729-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="5d729-151">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="5d729-151">Client setup</span></span>

<span data-ttu-id="5d729-152">Installare il [ASPNET](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacchetto NuGet (non il [ASPNET](http://nuget.org/packages/microsoft.aspnet.signalr) pacchetto).</span><span class="sxs-lookup"><span data-stu-id="5d729-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="5d729-153">Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e i client di Windows Phone, per .NET 4 e .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d729-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="5d729-154">Se la versione di SignalR in uso sul client è diversa da quella che si dispone sul server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="5d729-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="5d729-155">Ad esempio, un server che esegue la versione 2 di SignalR supporterà i client che hanno installato 1.1.x, nonché i client con la versione 2 installato.</span><span class="sxs-lookup"><span data-stu-id="5d729-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="5d729-156">Se la differenza tra la versione sul server e la versione del client è troppo elevata o se il client è più recente rispetto al server, SignalR genera un `InvalidOperationException` eccezione quando il client prova a stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="5d729-157">Messaggio di errore "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="5d729-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="5d729-158">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-158">How to establish a connection</span></span>

<span data-ttu-id="5d729-159">Prima di stabilire una connessione, è necessario creare un `HubConnection` dell'oggetto e creare un proxy.</span><span class="sxs-lookup"><span data-stu-id="5d729-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="5d729-160">Per stabilire la connessione, chiamare il `Start` metodo di `HubConnection` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5d729-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="5d729-161">Per i client JavaScript è necessario registrare almeno un gestore di eventi prima di chiamare il `Start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="5d729-162">Non è necessario per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="5d729-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="5d729-163">Per i client JavaScript, il codice proxy generato automaticamente Crea proxy per tutti gli hub che esiste nel server e la registrazione di un gestore di è consente di indicare quali hub il client intende utilizzare.</span><span class="sxs-lookup"><span data-stu-id="5d729-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="5d729-164">Ma per un client .NET è creare i proxy di Hub manualmente, in modo da SignalR presuppone che si userà qualsiasi Hub che si crea un proxy per.</span><span class="sxs-lookup"><span data-stu-id="5d729-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="5d729-165">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d729-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="5d729-166">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="5d729-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="5d729-167">Il `Start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="5d729-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="5d729-168">Per assicurarsi che le successive righe di codice non eseguire fino a dopo aver stabilita la connessione, usare `await` in un metodo asincrono di ASP.NET 4.5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="5d729-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="5d729-169">Non usare `.Wait()` in un client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="5d729-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="5d729-170">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="5d729-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="5d729-171">Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendendo un servizio disponibile attraverso i limiti del dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="5d729-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="5d729-172">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-172">How to configure the connection</span></span>

<span data-ttu-id="5d729-173">Prima stabilire una connessione, è possibile specificare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5d729-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="5d729-174">Limite di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="5d729-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="5d729-175">I parametri della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="5d729-175">Query string parameters.</span></span>
- <span data-ttu-id="5d729-176">Il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="5d729-176">The transport method.</span></span>
- <span data-ttu-id="5d729-177">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d729-177">HTTP headers.</span></span>
- <span data-ttu-id="5d729-178">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="5d729-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="5d729-179">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="5d729-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="5d729-180">Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito di 2.</span><span class="sxs-lookup"><span data-stu-id="5d729-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="5d729-181">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="5d729-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="5d729-182">Per altre informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d729-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="5d729-183">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="5d729-183">How to specify query string parameters</span></span>

<span data-ttu-id="5d729-184">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="5d729-185">Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="5d729-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="5d729-186">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="5d729-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="5d729-187">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="5d729-187">How to specify the transport method</span></span>

<span data-ttu-id="5d729-188">Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="5d729-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="5d729-189">Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="5d729-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="5d729-190">Per specificare il metodo di trasporto, passare un oggetto di trasporto per il metodo di avvio.</span><span class="sxs-lookup"><span data-stu-id="5d729-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="5d729-191">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="5d729-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="5d729-192">Il [Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) dello spazio dei nomi include le classi seguenti che è possibile usare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="5d729-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="5d729-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="5d729-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="5d729-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="5d729-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="5d729-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando i server e client usare .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="5d729-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="5d729-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il migliore trasporto supportato dal client sia sul server.</span><span class="sxs-lookup"><span data-stu-id="5d729-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="5d729-197">Questo è il trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="5d729-197">This is the default transport.</span></span> <span data-ttu-id="5d729-198">Il passaggio di questa opzione in per il `Start` metodo ha lo stesso effetto come non conformi in un elemento.)</span><span class="sxs-lookup"><span data-stu-id="5d729-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="5d729-199">Il trasporto ForeverFrame non è incluso nell'elenco perché è utilizzato solo dai browser.</span><span class="sxs-lookup"><span data-stu-id="5d729-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="5d729-200">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="5d729-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="5d729-201">Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5d729-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="5d729-202">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="5d729-202">How to specify HTTP headers</span></span>

<span data-ttu-id="5d729-203">Per impostare le intestazioni HTTP, usare il `Headers` proprietà sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="5d729-204">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d729-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="5d729-205">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="5d729-205">How to specify client certificates</span></span>

<span data-ttu-id="5d729-206">Per aggiungere i certificati client, usare il `AddClientCertificate` metodo sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="5d729-207">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="5d729-207">How to create the Hub proxy</span></span>

<span data-ttu-id="5d729-208">Per definire i metodi sul client che un Hub è possibile chiamare dal server e per richiamare metodi su un Hub sul server, creare un proxy per l'Hub chiamando `CreateHubProxy` sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="5d729-209">La stringa è passare al `CreateHubProxy` è il nome della classe Hub o il nome specificato da di `HubName` attributo se è stata utilizzata nel server.</span><span class="sxs-lookup"><span data-stu-id="5d729-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="5d729-210">Nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="5d729-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="5d729-211">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="5d729-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="5d729-212">**Creare il proxy client per la classe dell'Hub**</span><span class="sxs-lookup"><span data-stu-id="5d729-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="5d729-213">Se si decora la classe di Hub con un `HubName` attributo, usare il nome.</span><span class="sxs-lookup"><span data-stu-id="5d729-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="5d729-214">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="5d729-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="5d729-215">**Creare il proxy client per la classe dell'Hub**</span><span class="sxs-lookup"><span data-stu-id="5d729-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="5d729-216">Se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso memorizzato nella cache `IHubProxy` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5d729-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="5d729-217">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="5d729-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="5d729-218">Per definire un metodo che può chiamare il server, usare il proxy `On` metodo per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="5d729-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="5d729-219">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="5d729-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="5d729-220">Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` sul client.</span><span class="sxs-lookup"><span data-stu-id="5d729-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="5d729-221">Piattaforme client diverse hanno requisiti diversi per la modalità di scrittura codice del metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="5d729-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="5d729-222">Gli esempi illustrati sono per i client di WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="5d729-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="5d729-223">WPF, Silverlight ed esempi di applicazione console vengono forniti [una sezione separata più avanti in questo argomento](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="5d729-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="5d729-224">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-224">Methods without parameters</span></span>

<span data-ttu-id="5d729-225">Se il metodo in fase di gestione non dispone di parametri, usare l'overload del metodo non generico di `On` metodo:</span><span class="sxs-lookup"><span data-stu-id="5d729-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="5d729-226">**Codice lato server chiamata client metodo senza parametri**</span><span class="sxs-lookup"><span data-stu-id="5d729-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="5d729-227">**Codice di WinRT Client per il metodo chiamato dal server senza parametri ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5d729-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="5d729-228">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="5d729-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="5d729-229">Se il metodo in fase di gestione dispone di parametri, specificare i tipi dei parametri come i tipi generici del `On` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5d729-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="5d729-230">Esistono overload generici del `On` metodo per consentire di specificare i parametri fino a 8 (4 in Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="5d729-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="5d729-231">Nell'esempio seguente, viene inviato un solo parametro per il `UpdateStockPrice` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5d729-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="5d729-232">**Metodo client con un parametro di chiamata del codice server**</span><span class="sxs-lookup"><span data-stu-id="5d729-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="5d729-233">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="5d729-234">**Il codice WinRT Client per un metodo chiamato dal server con un parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5d729-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="5d729-235">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="5d729-236">Come alternativa per specificare parametri come tipi generici del `On` metodo, è possibile specificare parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="5d729-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="5d729-237">**Metodo client con un parametro di chiamata del codice server**</span><span class="sxs-lookup"><span data-stu-id="5d729-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="5d729-238">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="5d729-239">**Il codice WinRT Client per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5d729-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="5d729-240">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="5d729-240">How to remove a handler</span></span>

<span data-ttu-id="5d729-241">Per rimuovere un gestore, chiamare il `Dispose` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5d729-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="5d729-242">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="5d729-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="5d729-243">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="5d729-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="5d729-244">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="5d729-244">How to call server methods from the client</span></span>

<span data-ttu-id="5d729-245">Per chiamare un metodo nel server, usare il `Invoke` metodo sul proxy dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="5d729-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="5d729-246">Se il metodo del server non restituisce alcun valore, usare l'overload del metodo non generico di `Invoke` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5d729-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="5d729-247">**Codice del server per un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="5d729-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="5d729-248">**Codice client chiama un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="5d729-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="5d729-249">Se il metodo del server ha un valore restituito, specificare il tipo restituito come tipo generico del `Invoke` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5d729-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="5d729-250">**Codice server per un metodo che presenta un valore restituito e accetta un parametro di tipo complesso**</span><span class="sxs-lookup"><span data-stu-id="5d729-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="5d729-251">**La classe Stock utilizzato per il parametro e valore restituito**</span><span class="sxs-lookup"><span data-stu-id="5d729-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="5d729-252">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo asincrono di ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="5d729-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="5d729-253">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="5d729-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="5d729-254">Il `Invoke` metodo viene eseguito in modo asincrono e restituisce un `Task` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5d729-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="5d729-255">Se non si specifica `await` o `.Wait()`, la riga successiva del codice verrà eseguito prima il metodo che si richiama ha terminato l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5d729-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="5d729-256">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="5d729-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="5d729-257">SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="5d729-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="5d729-258">`Received`: Generato quando vengono ricevuti tutti i dati nella connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="5d729-259">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="5d729-259">Provides the received data.</span></span>
- <span data-ttu-id="5d729-260">`ConnectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.</span><span class="sxs-lookup"><span data-stu-id="5d729-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="5d729-261">`Reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="5d729-262">`Reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="5d729-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="5d729-263">`StateChanged`: Generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="5d729-264">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="5d729-264">Provides the old state and the new state.</span></span> <span data-ttu-id="5d729-265">Per informazioni sulla connessione di vedere i valori dello stato [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="5d729-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="5d729-266">`Closed`: Generato quando la connessione è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="5d729-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="5d729-267">Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori non irreversibili ma causano problemi di connessione intermittente, ad esempio come lentezza o di frequenza eliminazione della connessione, gestire il `ConnectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="5d729-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="5d729-268">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="5d729-269">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="5d729-269">How to handle errors</span></span>

<span data-ttu-id="5d729-270">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="5d729-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="5d729-271">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="5d729-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="5d729-272">Per gestire gli errori che genera SignalR, è possibile aggiungere un gestore per il `Error` evento sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="5d729-273">Per gestire gli errori di chiamate al metodo, eseguire il wrapping di codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="5d729-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="5d729-274">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="5d729-274">How to enable client-side logging</span></span>

<span data-ttu-id="5d729-275">Per abilitare la registrazione lato client, impostare il `TraceLevel` e `TraceWriter` le proprietà dell'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="5d729-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="5d729-276">Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="5d729-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="5d729-277">Gli esempi di codice illustrati in precedenza per la definizione di metodi di client può chiamare il server si applicano ai client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="5d729-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="5d729-278">Gli esempi seguenti mostrano il codice equivalente per WPF, Silverlight e i client di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="5d729-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="5d729-279">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-279">Methods without parameters</span></span>

<span data-ttu-id="5d729-280">**Codice client WPF per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="5d729-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="5d729-281">**Codice client Silverlight per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="5d729-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="5d729-282">**Il codice client dell'applicazione console per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="5d729-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="5d729-283">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="5d729-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="5d729-284">**Codice client WPF per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="5d729-285">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="5d729-286">**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="5d729-287">Metodi con parametri, che specificano gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="5d729-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="5d729-288">**Codice client WPF per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="5d729-289">**Codice client Silverlight per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="5d729-290">**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="5d729-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
