---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guida dell'API Hub SignalR ASP.NET-client .NETC#() | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e svantaggi...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057004"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="b4642-103">Guida dell'API Hub SignalR ASP.NET-client .NETC#()</span><span class="sxs-lookup"><span data-stu-id="b4642-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b4642-104">Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="b4642-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="b4642-105">L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="b4642-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="b4642-106">Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="b4642-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="b4642-107">Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="b4642-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="b4642-108">SignalR si occupa automaticamente di tutte le tubature da client a server.</span><span class="sxs-lookup"><span data-stu-id="b4642-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="b4642-109">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="b4642-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="b4642-110">Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come creare un'applicazione SignalR completa, vedere [SignalR-Introduzione](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="b4642-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b4642-111">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="b4642-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b4642-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b4642-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="b4642-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b4642-113">.NET 4.5</span></span>
> - <span data-ttu-id="b4642-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="b4642-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b4642-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="b4642-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b4642-116">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b4642-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b4642-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="b4642-117">Questions and comments</span></span>
>
> <span data-ttu-id="b4642-118">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="b4642-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b4642-119">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b4642-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="b4642-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b4642-120">Overview</span></span>

<span data-ttu-id="b4642-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="b4642-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="b4642-122">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="b4642-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="b4642-123">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="b4642-124">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="b4642-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="b4642-125">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="b4642-126">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="b4642-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="b4642-127">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="b4642-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="b4642-128">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="b4642-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="b4642-129">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="b4642-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="b4642-130">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="b4642-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="b4642-131">Come creare il proxy dell'hub</span><span class="sxs-lookup"><span data-stu-id="b4642-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="b4642-132">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="b4642-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="b4642-133">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="b4642-134">Metodi con parametri, specifica dei tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="b4642-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="b4642-135">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="b4642-136">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="b4642-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="b4642-137">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="b4642-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="b4642-138">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="b4642-139">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="b4642-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="b4642-140">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="b4642-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="b4642-141">Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="b4642-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="b4642-142">Per i progetti client .NET di esempio, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="b4642-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="b4642-143">[Gustavo-Armenta/SignalR-](https://github.com/gustavo-armenta/SignalR-Samples) esempi in GitHub.com (WinRT, Silverlight, esempi di app console).</span><span class="sxs-lookup"><span data-stu-id="b4642-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="b4642-144">[DamianEdwards/SignalR-MoveShapeDemo/MoveShape. desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) su GitHub.com (esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="b4642-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="b4642-145">[SignalR/Microsoft. AspNet. SignalR. client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app console).</span><span class="sxs-lookup"><span data-stu-id="b4642-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="b4642-146">Per la documentazione su come programmare il server o i client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="b4642-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="b4642-147">Guida dell'API degli hub SignalR-server</span><span class="sxs-lookup"><span data-stu-id="b4642-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="b4642-148">Guida dell'API degli hub SignalR-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b4642-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="b4642-149">I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API.</span><span class="sxs-lookup"><span data-stu-id="b4642-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="b4642-150">Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4642-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="b4642-151">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="b4642-151">Client setup</span></span>

<span data-ttu-id="b4642-152">Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (non il pacchetto [Microsoft. AspNet. SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="b4642-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="b4642-153">Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e client Windows Phone, sia per .NET 4 che per .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b4642-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="b4642-154">Se la versione di SignalR presente nel client è diversa dalla versione del server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="b4642-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="b4642-155">Ad esempio, un server che esegue SignalR versione 2 supporterà i client in cui è installato 1.1. x e i client in cui è installata la versione 2.</span><span class="sxs-lookup"><span data-stu-id="b4642-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="b4642-156">Se la differenza tra la versione sul server e la versione sul client è troppo grande o se il client è più recente del server, SignalR genera un'eccezione `InvalidOperationException` quando il client tenta di stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="b4642-157">Il messaggio di errore è "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="b4642-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="b4642-158">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-158">How to establish a connection</span></span>

<span data-ttu-id="b4642-159">Prima di stabilire una connessione, è necessario creare un oggetto `HubConnection` e creare un proxy.</span><span class="sxs-lookup"><span data-stu-id="b4642-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="b4642-160">Per stabilire la connessione, chiamare il metodo `Start` sull'oggetto `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="b4642-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="b4642-161">Per i client JavaScript è necessario registrare almeno un gestore eventi prima di chiamare il metodo `Start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="b4642-162">Questa operazione non è necessaria per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="b4642-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="b4642-163">Per i client JavaScript, il codice proxy generato crea automaticamente proxy per tutti gli hub presenti nel server e la registrazione di un gestore è la modalità di indicazione degli hub che il client intende usare.</span><span class="sxs-lookup"><span data-stu-id="b4642-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="b4642-164">Tuttavia, per un client .NET è possibile creare i proxy dell'hub manualmente, quindi SignalR presuppone che venga usato qualsiasi hub per cui si crea un proxy.</span><span class="sxs-lookup"><span data-stu-id="b4642-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="b4642-165">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="b4642-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="b4642-166">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="b4642-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="b4642-167">Il metodo `Start` viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b4642-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="b4642-168">Per assicurarsi che le righe di codice successive non vengano eseguite fino a quando non viene stabilita la connessione, usare `await` in un metodo asincrono ASP.NET 4,5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="b4642-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="b4642-169">Non usare `.Wait()` in un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="b4642-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="b4642-170">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="b4642-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="b4642-171">Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendere disponibile un servizio tra i limiti di dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4642-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="b4642-172">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-172">How to configure the connection</span></span>

<span data-ttu-id="b4642-173">Prima di stabilire una connessione, è possibile specificare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b4642-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="b4642-174">Limite connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="b4642-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="b4642-175">Parametri della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="b4642-175">Query string parameters.</span></span>
- <span data-ttu-id="b4642-176">Metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="b4642-176">The transport method.</span></span>
- <span data-ttu-id="b4642-177">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4642-177">HTTP headers.</span></span>
- <span data-ttu-id="b4642-178">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="b4642-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="b4642-179">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="b4642-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="b4642-180">Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee dal valore predefinito 2.</span><span class="sxs-lookup"><span data-stu-id="b4642-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="b4642-181">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="b4642-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="b4642-182">Per ulteriori informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4642-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="b4642-183">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="b4642-183">How to specify query string parameters</span></span>

<span data-ttu-id="b4642-184">Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="b4642-185">Nell'esempio seguente viene illustrato come impostare un parametro della stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="b4642-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="b4642-186">Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="b4642-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="b4642-187">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="b4642-187">How to specify the transport method</span></span>

<span data-ttu-id="b4642-188">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client.</span><span class="sxs-lookup"><span data-stu-id="b4642-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="b4642-189">Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="b4642-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="b4642-190">Per specificare il metodo di trasporto, passare un oggetto trasporto al metodo Start.</span><span class="sxs-lookup"><span data-stu-id="b4642-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="b4642-191">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="b4642-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="b4642-192">Lo spazio dei nomi [Microsoft. AspNet. SignalR. client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) include le classi seguenti che è possibile utilizzare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="b4642-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="b4642-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b4642-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b4642-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="b4642-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="b4642-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo se il server e il client usano .NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="b4642-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="b4642-196">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il trasporto migliore supportato dal client e dal server.</span><span class="sxs-lookup"><span data-stu-id="b4642-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="b4642-197">Si tratta del trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="b4642-197">This is the default transport.</span></span> <span data-ttu-id="b4642-198">Il passaggio di questo elemento al metodo `Start` ha lo stesso effetto del mancato passaggio.</span><span class="sxs-lookup"><span data-stu-id="b4642-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="b4642-199">Il trasporto ForeverFrame non è incluso in questo elenco perché viene usato solo dai browser.</span><span class="sxs-lookup"><span data-stu-id="b4642-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="b4642-200">Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="b4642-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="b4642-201">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b4642-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="b4642-202">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="b4642-202">How to specify HTTP headers</span></span>

<span data-ttu-id="b4642-203">Per impostare le intestazioni HTTP, utilizzare la proprietà `Headers` nell'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="b4642-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="b4642-204">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4642-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="b4642-205">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="b4642-205">How to specify client certificates</span></span>

<span data-ttu-id="b4642-206">Per aggiungere certificati client, utilizzare il metodo `AddClientCertificate` sull'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="b4642-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="b4642-207">Come creare il proxy dell'hub</span><span class="sxs-lookup"><span data-stu-id="b4642-207">How to create the Hub proxy</span></span>

<span data-ttu-id="b4642-208">Per definire i metodi sul client che un hub può chiamare dal server e per richiamare i metodi in un hub sul server, creare un proxy per l'hub chiamando `CreateHubProxy` sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="b4642-209">La stringa passata a `CreateHubProxy` è il nome della classe Hub o il nome specificato dall'attributo `HubName` se ne è stato usato uno nel server.</span><span class="sxs-lookup"><span data-stu-id="b4642-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="b4642-210">La corrispondenza del nome non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="b4642-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="b4642-211">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="b4642-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="b4642-212">**Creare il proxy client per la classe Hub**</span><span class="sxs-lookup"><span data-stu-id="b4642-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="b4642-213">Se si decora la classe Hub con un attributo `HubName`, usare tale nome.</span><span class="sxs-lookup"><span data-stu-id="b4642-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="b4642-214">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="b4642-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="b4642-215">**Creare il proxy client per la classe Hub**</span><span class="sxs-lookup"><span data-stu-id="b4642-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="b4642-216">Se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso oggetto `IHubProxy` memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="b4642-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="b4642-217">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="b4642-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="b4642-218">Per definire un metodo che il server può chiamare, usare il metodo `On` del proxy per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="b4642-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="b4642-219">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="b4642-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="b4642-220">Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`o `UpdateStockPrice` nel client.</span><span class="sxs-lookup"><span data-stu-id="b4642-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="b4642-221">Diverse piattaforme client hanno requisiti diversi per la modalità di scrittura del codice del metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b4642-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="b4642-222">Gli esempi illustrati sono per i client WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="b4642-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="b4642-223">Gli esempi di applicazioni WPF, Silverlight e console sono disponibili in [una sezione separata più avanti in questo argomento](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="b4642-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="b4642-224">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-224">Methods without parameters</span></span>

<span data-ttu-id="b4642-225">Se il metodo che si sta gestendo non contiene parametri, usare l'overload non generico del metodo `On`:</span><span class="sxs-lookup"><span data-stu-id="b4642-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="b4642-226">**Codice server che chiama il metodo client senza parametri**</span><span class="sxs-lookup"><span data-stu-id="b4642-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="b4642-227">**Codice client WinRT per il metodo chiamato da server senza parametri ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4642-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b4642-228">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="b4642-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b4642-229">Se il metodo che si sta gestendo presenta parametri, specificare i tipi di parametri come tipi generici del metodo `On`.</span><span class="sxs-lookup"><span data-stu-id="b4642-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="b4642-230">Sono disponibili overload generici del metodo `On` per consentire l'impostazione di un massimo di 8 parametri (4 su Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="b4642-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="b4642-231">Nell'esempio seguente viene inviato un parametro al metodo `UpdateStockPrice`.</span><span class="sxs-lookup"><span data-stu-id="b4642-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="b4642-232">**Codice server che chiama il metodo client con un parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="b4642-233">**Classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="b4642-234">**Codice client WinRT per un metodo chiamato dal server con un parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4642-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b4642-235">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b4642-236">In alternativa alla specifica di parametri come tipi generici del metodo `On`, è possibile specificare parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="b4642-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="b4642-237">**Codice server che chiama il metodo client con un parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="b4642-238">**Classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="b4642-239">**Codice client WinRT per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="b4642-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="b4642-240">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="b4642-240">How to remove a handler</span></span>

<span data-ttu-id="b4642-241">Per rimuovere un gestore, chiamare il relativo metodo `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="b4642-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="b4642-242">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="b4642-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="b4642-243">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="b4642-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="b4642-244">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="b4642-244">How to call server methods from the client</span></span>

<span data-ttu-id="b4642-245">Per chiamare un metodo sul server, usare il metodo `Invoke` sul proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b4642-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="b4642-246">Se il metodo Server non restituisce alcun valore, utilizzare l'overload non generico del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="b4642-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="b4642-247">**Codice server per un metodo senza valore restituito**</span><span class="sxs-lookup"><span data-stu-id="b4642-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="b4642-248">**Codice client che chiama un metodo senza valore restituito**</span><span class="sxs-lookup"><span data-stu-id="b4642-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="b4642-249">Se il metodo Server ha un valore restituito, specificare il tipo restituito come tipo generico del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="b4642-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="b4642-250">**Codice server per un metodo che ha un valore restituito e accetta un parametro di tipo complesso**</span><span class="sxs-lookup"><span data-stu-id="b4642-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="b4642-251">**Classe Stock utilizzata per il parametro e il valore restituito**</span><span class="sxs-lookup"><span data-stu-id="b4642-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="b4642-252">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso in un metodo asincrono ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="b4642-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="b4642-253">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="b4642-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="b4642-254">Il metodo `Invoke` viene eseguito in modo asincrono e restituisce un oggetto `Task`.</span><span class="sxs-lookup"><span data-stu-id="b4642-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="b4642-255">Se non si specifica `await` o `.Wait()`, la riga di codice successiva verrà eseguita prima del completamento dell'esecuzione del metodo richiamato.</span><span class="sxs-lookup"><span data-stu-id="b4642-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="b4642-256">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="b4642-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="b4642-257">SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="b4642-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="b4642-258">`Received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="b4642-259">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="b4642-259">Provides the received data.</span></span>
- <span data-ttu-id="b4642-260">`ConnectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.</span><span class="sxs-lookup"><span data-stu-id="b4642-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="b4642-261">`Reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="b4642-262">`Reconnected`: generato quando il trasporto sottostante si è riconnesso.</span><span class="sxs-lookup"><span data-stu-id="b4642-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="b4642-263">`StateChanged`: generato quando viene modificato lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="b4642-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="b4642-264">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="b4642-264">Provides the old state and the new state.</span></span> <span data-ttu-id="b4642-265">Per informazioni sui valori dello stato di connessione, vedere [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4642-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="b4642-266">`Closed`: generato quando la connessione è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="b4642-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="b4642-267">Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori che non sono irreversibili, ma che provocano problemi di connessione intermittenti, come la lentezza o l'eliminazione frequente della connessione, gestire l'evento `ConnectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="b4642-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="b4642-268">Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b4642-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="b4642-269">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="b4642-269">How to handle errors</span></span>

<span data-ttu-id="b4642-270">Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="b4642-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="b4642-271">Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="b4642-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="b4642-272">Per gestire gli errori generati da SignalR, è possibile aggiungere un gestore per l'evento `Error` sull'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="b4642-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="b4642-273">Per gestire gli errori dalle chiamate al metodo, eseguire il wrapping del codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="b4642-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="b4642-274">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="b4642-274">How to enable client-side logging</span></span>

<span data-ttu-id="b4642-275">Per abilitare la registrazione lato client, impostare le proprietà `TraceLevel` e `TraceWriter` nell'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="b4642-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="b4642-276">Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="b4642-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="b4642-277">Gli esempi di codice illustrati in precedenza per la definizione di metodi client che il server può chiamare si applicano ai client WinRT.</span><span class="sxs-lookup"><span data-stu-id="b4642-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="b4642-278">Negli esempi seguenti viene illustrato il codice equivalente per i client WPF, Silverlight e dell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="b4642-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="b4642-279">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-279">Methods without parameters</span></span>

<span data-ttu-id="b4642-280">**Codice client WPF per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="b4642-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="b4642-281">**Codice client Silverlight per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="b4642-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="b4642-282">**Codice client dell'applicazione console per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="b4642-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="b4642-283">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="b4642-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="b4642-284">**Codice client WPF per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="b4642-285">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="b4642-286">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="b4642-287">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="b4642-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="b4642-288">**Codice client WPF per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="b4642-289">**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="b4642-290">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="b4642-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
