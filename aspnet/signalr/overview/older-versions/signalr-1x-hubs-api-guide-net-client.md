---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guida dell'API Hub SignalR ASP.NET-client .NET (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e svantaggi...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623802"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="c75e7-103">Guida dell'API Hub SignalR ASP.NET-client .NET (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="c75e7-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>

<span data-ttu-id="c75e7-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c75e7-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c75e7-105">Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="c75e7-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="c75e7-106">L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="c75e7-107">Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="c75e7-108">Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="c75e7-109">SignalR si occupa automaticamente di tutte le tubature da client a server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="c75e7-110">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="c75e7-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="c75e7-111">Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come creare un'applicazione SignalR completa, vedere [SignalR-Introduzione](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="c75e7-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="c75e7-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c75e7-112">Overview</span></span>

<span data-ttu-id="c75e7-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="c75e7-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="c75e7-114">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="c75e7-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="c75e7-115">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="c75e7-116">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="c75e7-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="c75e7-117">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="c75e7-118">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="c75e7-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="c75e7-119">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="c75e7-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="c75e7-120">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="c75e7-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="c75e7-121">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="c75e7-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="c75e7-122">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="c75e7-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="c75e7-123">Come creare il proxy dell'hub</span><span class="sxs-lookup"><span data-stu-id="c75e7-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="c75e7-124">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c75e7-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="c75e7-125">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="c75e7-126">Metodi con parametri, specifica dei tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="c75e7-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="c75e7-127">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="c75e7-128">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="c75e7-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="c75e7-129">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="c75e7-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="c75e7-130">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="c75e7-131">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="c75e7-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="c75e7-132">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="c75e7-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="c75e7-133">Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c75e7-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="c75e7-134">Per i progetti client .NET di esempio, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="c75e7-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="c75e7-135">[Gustavo-Armenta/SignalR-](https://github.com/gustavo-armenta/SignalR-Samples) esempi in GitHub.com (WinRT, Silverlight, esempi di app console).</span><span class="sxs-lookup"><span data-stu-id="c75e7-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="c75e7-136">[DamianEdwards/SignalR-MoveShapeDemo/MoveShape. desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) su GitHub.com (esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="c75e7-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="c75e7-137">[SignalR/Microsoft. AspNet. SignalR. client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app console).</span><span class="sxs-lookup"><span data-stu-id="c75e7-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="c75e7-138">Per la documentazione su come programmare il server o i client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="c75e7-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="c75e7-139">Guida dell'API degli hub SignalR-server</span><span class="sxs-lookup"><span data-stu-id="c75e7-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="c75e7-140">Guida dell'API degli hub SignalR-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c75e7-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="c75e7-141">I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API.</span><span class="sxs-lookup"><span data-stu-id="c75e7-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="c75e7-142">Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="c75e7-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="c75e7-143">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="c75e7-143">Client setup</span></span>

<span data-ttu-id="c75e7-144">Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (non il pacchetto [Microsoft. AspNet. SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="c75e7-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="c75e7-145">Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e client Windows Phone, sia per .NET 4 che per .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="c75e7-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="c75e7-146">Se la versione di SignalR presente nel client è diversa dalla versione del server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="c75e7-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="c75e7-147">Ad esempio, quando SignalR versione 2,0 viene rilasciato e installato nel server, il server supporterà i client in cui è installato 1.1. x, oltre ai client con 2,0 installato.</span><span class="sxs-lookup"><span data-stu-id="c75e7-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="c75e7-148">Se la differenza tra la versione sul server e la versione sul client è troppo grande, SignalR genera un'eccezione `InvalidOperationException` quando il client tenta di stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="c75e7-149">Il messaggio di errore è "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="c75e7-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="c75e7-150">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-150">How to establish a connection</span></span>

<span data-ttu-id="c75e7-151">Prima di stabilire una connessione, è necessario creare un oggetto `HubConnection` e creare un proxy.</span><span class="sxs-lookup"><span data-stu-id="c75e7-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="c75e7-152">Per stabilire la connessione, chiamare il metodo `Start` sull'oggetto `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="c75e7-153">Per i client JavaScript è necessario registrare almeno un gestore eventi prima di chiamare il metodo `Start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="c75e7-154">Questa operazione non è necessaria per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="c75e7-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="c75e7-155">Per i client JavaScript, il codice proxy generato crea automaticamente proxy per tutti gli hub presenti nel server e la registrazione di un gestore è la modalità di indicazione degli hub che il client intende usare.</span><span class="sxs-lookup"><span data-stu-id="c75e7-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="c75e7-156">Tuttavia, per un client .NET è possibile creare i proxy dell'hub manualmente, quindi SignalR presuppone che venga usato qualsiasi hub per cui si crea un proxy.</span><span class="sxs-lookup"><span data-stu-id="c75e7-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="c75e7-157">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="c75e7-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="c75e7-158">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="c75e7-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="c75e7-159">Il metodo `Start` viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="c75e7-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="c75e7-160">Per assicurarsi che le righe di codice successive non vengano eseguite fino a quando non viene stabilita la connessione, usare `await` in un metodo asincrono ASP.NET 4,5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="c75e7-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="c75e7-161">Non usare `.Wait()` in un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="c75e7-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="c75e7-162">La classe `HubConnection` è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="c75e7-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="c75e7-163">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="c75e7-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="c75e7-164">Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendere disponibile un servizio tra i limiti di dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="c75e7-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="c75e7-165">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-165">How to configure the connection</span></span>

<span data-ttu-id="c75e7-166">Prima di stabilire una connessione, è possibile specificare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c75e7-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="c75e7-167">Limite connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="c75e7-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="c75e7-168">Parametri della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c75e7-168">Query string parameters.</span></span>
- <span data-ttu-id="c75e7-169">Metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="c75e7-169">The transport method.</span></span>
- <span data-ttu-id="c75e7-170">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="c75e7-170">HTTP headers.</span></span>
- <span data-ttu-id="c75e7-171">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="c75e7-172">Come impostare il numero massimo di connessioni simultanee nei client WPF</span><span class="sxs-lookup"><span data-stu-id="c75e7-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="c75e7-173">Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee dal valore predefinito 2.</span><span class="sxs-lookup"><span data-stu-id="c75e7-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="c75e7-174">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="c75e7-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="c75e7-175">Per ulteriori informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="c75e7-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="c75e7-176">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="c75e7-176">How to specify query string parameters</span></span>

<span data-ttu-id="c75e7-177">Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="c75e7-178">Nell'esempio seguente viene illustrato come impostare un parametro della stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="c75e7-179">Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="c75e7-180">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="c75e7-180">How to specify the transport method</span></span>

<span data-ttu-id="c75e7-181">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="c75e7-182">Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="c75e7-183">Per specificare il metodo di trasporto, passare un oggetto trasporto al metodo Start.</span><span class="sxs-lookup"><span data-stu-id="c75e7-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="c75e7-184">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="c75e7-185">Lo spazio dei nomi [Microsoft. AspNet. SignalR. client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) include le classi seguenti che è possibile utilizzare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="c75e7-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="c75e7-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="c75e7-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="c75e7-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="c75e7-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="c75e7-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo se il server e il client usano .NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="c75e7-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="c75e7-189">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il trasporto migliore supportato dal client e dal server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="c75e7-190">Si tratta del trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="c75e7-190">This is the default transport.</span></span> <span data-ttu-id="c75e7-191">Il passaggio di questo elemento al metodo `Start` ha lo stesso effetto del mancato passaggio.</span><span class="sxs-lookup"><span data-stu-id="c75e7-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="c75e7-192">Il trasporto ForeverFrame non è incluso in questo elenco perché viene usato solo dai browser.</span><span class="sxs-lookup"><span data-stu-id="c75e7-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="c75e7-193">Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="c75e7-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="c75e7-194">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="c75e7-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="c75e7-195">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="c75e7-195">How to specify HTTP headers</span></span>

<span data-ttu-id="c75e7-196">Per impostare le intestazioni HTTP, utilizzare la proprietà `Headers` nell'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="c75e7-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="c75e7-197">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="c75e7-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="c75e7-198">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="c75e7-198">How to specify client certificates</span></span>

<span data-ttu-id="c75e7-199">Per aggiungere certificati client, utilizzare il metodo `AddClientCertificate` sull'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="c75e7-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="c75e7-200">Come creare il proxy dell'hub</span><span class="sxs-lookup"><span data-stu-id="c75e7-200">How to create the Hub proxy</span></span>

<span data-ttu-id="c75e7-201">Per definire i metodi sul client che un hub può chiamare dal server e per richiamare i metodi in un hub sul server, creare un proxy per l'hub chiamando `CreateHubProxy` sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="c75e7-202">La stringa passata a `CreateHubProxy` è il nome della classe Hub o il nome specificato dall'attributo `HubName` se ne è stato usato uno nel server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="c75e7-203">La corrispondenza dei nomi non prevede alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c75e7-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="c75e7-204">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="c75e7-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="c75e7-205">**Creare il proxy client per la classe Hub**</span><span class="sxs-lookup"><span data-stu-id="c75e7-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="c75e7-206">Se si decora la classe Hub con un attributo `HubName`, usare tale nome.</span><span class="sxs-lookup"><span data-stu-id="c75e7-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="c75e7-207">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="c75e7-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="c75e7-208">**Creare il proxy client per la classe Hub**</span><span class="sxs-lookup"><span data-stu-id="c75e7-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="c75e7-209">L'oggetto proxy è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="c75e7-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="c75e7-210">Infatti, se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso oggetto `IHubProxy` memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="c75e7-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="c75e7-211">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c75e7-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="c75e7-212">Per definire un metodo che il server può chiamare, usare il metodo `On` del proxy per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="c75e7-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="c75e7-213">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c75e7-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="c75e7-214">Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`o `UpdateStockPrice` nel client.</span><span class="sxs-lookup"><span data-stu-id="c75e7-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="c75e7-215">Diverse piattaforme client hanno requisiti diversi per la modalità di scrittura del codice del metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="c75e7-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="c75e7-216">Gli esempi illustrati sono per i client WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="c75e7-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="c75e7-217">Gli esempi di applicazioni WPF, Silverlight e console sono disponibili in [una sezione separata più avanti in questo argomento](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="c75e7-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="c75e7-218">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-218">Methods without parameters</span></span>

<span data-ttu-id="c75e7-219">Se il metodo che si sta gestendo non contiene parametri, usare l'overload non generico del metodo `On`:</span><span class="sxs-lookup"><span data-stu-id="c75e7-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="c75e7-220">**Codice server che chiama il metodo client senza parametri**</span><span class="sxs-lookup"><span data-stu-id="c75e7-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="c75e7-221">**Codice client WinRT per il metodo chiamato da server senza parametri ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c75e7-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="c75e7-222">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="c75e7-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="c75e7-223">Se il metodo che si sta gestendo presenta parametri, specificare i tipi di parametri come tipi generici del metodo `On`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="c75e7-224">Sono disponibili overload generici del metodo `On` per consentire l'impostazione di un massimo di 8 parametri (4 su Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="c75e7-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="c75e7-225">Nell'esempio seguente viene inviato un parametro al metodo `UpdateStockPrice`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="c75e7-226">**Codice server che chiama il metodo client con un parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="c75e7-227">**Classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="c75e7-228">**Codice client WinRT per un metodo chiamato dal server con un parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c75e7-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="c75e7-229">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="c75e7-230">In alternativa alla specifica di parametri come tipi generici del metodo `On`, è possibile specificare parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="c75e7-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="c75e7-231">**Codice server che chiama il metodo client con un parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="c75e7-232">**Classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="c75e7-233">**Codice client WinRT per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro ([vedere gli esempi di WPF e Silverlight più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c75e7-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="c75e7-234">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="c75e7-234">How to remove a handler</span></span>

<span data-ttu-id="c75e7-235">Per rimuovere un gestore, chiamare il relativo metodo `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="c75e7-236">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="c75e7-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="c75e7-237">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="c75e7-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="c75e7-238">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="c75e7-238">How to call server methods from the client</span></span>

<span data-ttu-id="c75e7-239">Per chiamare un metodo sul server, usare il metodo `Invoke` sul proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c75e7-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="c75e7-240">Se il metodo Server non restituisce alcun valore, utilizzare l'overload non generico del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="c75e7-241">**Codice server per un metodo senza valore restituito**</span><span class="sxs-lookup"><span data-stu-id="c75e7-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="c75e7-242">**Codice client che chiama un metodo senza valore restituito**</span><span class="sxs-lookup"><span data-stu-id="c75e7-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="c75e7-243">Se il metodo Server ha un valore restituito, specificare il tipo restituito come tipo generico del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="c75e7-244">**Codice server per un metodo che ha un valore restituito e accetta un parametro di tipo complesso**</span><span class="sxs-lookup"><span data-stu-id="c75e7-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="c75e7-245">**Classe Stock utilizzata per il parametro e il valore restituito**</span><span class="sxs-lookup"><span data-stu-id="c75e7-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="c75e7-246">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso in un metodo asincrono ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="c75e7-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="c75e7-247">**Codice client che chiama un metodo che ha un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="c75e7-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="c75e7-248">Il metodo `Invoke` viene eseguito in modo asincrono e restituisce un oggetto `Task`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="c75e7-249">Se non si specifica `await` o `.Wait()`, la riga di codice successiva verrà eseguita prima del completamento dell'esecuzione del metodo richiamato.</span><span class="sxs-lookup"><span data-stu-id="c75e7-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="c75e7-250">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="c75e7-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="c75e7-251">SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="c75e7-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="c75e7-252">`Received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="c75e7-253">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="c75e7-253">Provides the received data.</span></span>
- <span data-ttu-id="c75e7-254">`ConnectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.</span><span class="sxs-lookup"><span data-stu-id="c75e7-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="c75e7-255">`Reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="c75e7-256">`Reconnected`: generato quando il trasporto sottostante si è riconnesso.</span><span class="sxs-lookup"><span data-stu-id="c75e7-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="c75e7-257">`StateChanged`: generato quando viene modificato lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="c75e7-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="c75e7-258">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="c75e7-258">Provides the old state and the new state.</span></span> <span data-ttu-id="c75e7-259">Per informazioni sui valori dello stato di connessione, vedere [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c75e7-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="c75e7-260">`Closed`: generato quando la connessione è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="c75e7-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="c75e7-261">Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori che non sono irreversibili, ma che provocano problemi di connessione intermittenti, come la lentezza o l'eliminazione frequente della connessione, gestire l'evento `ConnectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="c75e7-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="c75e7-262">Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="c75e7-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="c75e7-263">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="c75e7-263">How to handle errors</span></span>

<span data-ttu-id="c75e7-264">Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="c75e7-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="c75e7-265">Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="c75e7-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="c75e7-266">Per gestire gli errori generati da SignalR, è possibile aggiungere un gestore per l'evento `Error` sull'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="c75e7-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="c75e7-267">Per gestire gli errori dalle chiamate al metodo, eseguire il wrapping del codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="c75e7-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="c75e7-268">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="c75e7-268">How to enable client-side logging</span></span>

<span data-ttu-id="c75e7-269">Per abilitare la registrazione lato client, impostare le proprietà `TraceLevel` e `TraceWriter` nell'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="c75e7-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="c75e7-270">Esempi di codice dell'applicazione WPF, Silverlight e console per i metodi client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c75e7-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="c75e7-271">Gli esempi di codice illustrati in precedenza per la definizione di metodi client che il server può chiamare si applicano ai client WinRT.</span><span class="sxs-lookup"><span data-stu-id="c75e7-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="c75e7-272">Negli esempi seguenti viene illustrato il codice equivalente per i client WPF, Silverlight e dell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="c75e7-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="c75e7-273">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-273">Methods without parameters</span></span>

<span data-ttu-id="c75e7-274">**Codice client WPF per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="c75e7-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="c75e7-275">**Codice client Silverlight per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="c75e7-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="c75e7-276">**Codice client dell'applicazione console per il metodo chiamato da server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="c75e7-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="c75e7-277">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="c75e7-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="c75e7-278">**Codice client WPF per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="c75e7-279">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="c75e7-280">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="c75e7-281">Metodi con parametri, che specificano oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="c75e7-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="c75e7-282">**Codice client WPF per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="c75e7-283">**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="c75e7-284">**Codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="c75e7-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
