---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Guida dell'API per hub SignalR 1. x-client JavaScript | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 1,1 nei client JavaScript, ad esempio browser e applic di Windows Store (WinJS)...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536442"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="2df57-103">Guida all'API SignalR 1.x Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="2df57-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>

<span data-ttu-id="2df57-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2df57-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="2df57-105">Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 1,1 nei client JavaScript, ad esempio i browser e le applicazioni di Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="2df57-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="2df57-106">L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="2df57-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="2df57-107">Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="2df57-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="2df57-108">Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="2df57-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="2df57-109">SignalR si occupa automaticamente di tutte le tubature da client a server.</span><span class="sxs-lookup"><span data-stu-id="2df57-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="2df57-110">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="2df57-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="2df57-111">Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come creare un'applicazione SignalR completa, vedere [SignalR-Introduzione](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="2df57-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="2df57-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2df57-112">Overview</span></span>

<span data-ttu-id="2df57-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="2df57-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="2df57-114">Il proxy generato e la relativa funzione</span><span class="sxs-lookup"><span data-stu-id="2df57-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="2df57-115">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="2df57-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="2df57-116">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="2df57-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="2df57-117">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="2df57-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="2df57-118">Come creare un file fisico per il proxy generato da SignalR</span><span class="sxs-lookup"><span data-stu-id="2df57-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="2df57-119">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="2df57-120">$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()</span><span class="sxs-lookup"><span data-stu-id="2df57-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="2df57-121">Esecuzione asincrona del metodo Start</span><span class="sxs-lookup"><span data-stu-id="2df57-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="2df57-122">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="2df57-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="2df57-123">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="2df57-124">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="2df57-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="2df57-125">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="2df57-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="2df57-126">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="2df57-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="2df57-127">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="2df57-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="2df57-128">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="2df57-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="2df57-129">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="2df57-130">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="2df57-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="2df57-131">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="2df57-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="2df57-132">Per la documentazione su come programmare il server o i client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="2df57-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="2df57-133">Guida dell'API degli hub SignalR-server</span><span class="sxs-lookup"><span data-stu-id="2df57-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="2df57-134">Guida dell'API degli hub SignalR-client .NET</span><span class="sxs-lookup"><span data-stu-id="2df57-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="2df57-135">I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API.</span><span class="sxs-lookup"><span data-stu-id="2df57-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="2df57-136">Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="2df57-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="2df57-137">Il proxy generato e la relativa funzione</span><span class="sxs-lookup"><span data-stu-id="2df57-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="2df57-138">È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy generato automaticamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="2df57-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="2df57-139">Il proxy per consente di semplificare la sintassi del codice usato per connettersi, scrivere metodi che il server chiama e chiamare metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="2df57-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="2df57-140">Quando si scrive il codice per chiamare i metodi del server, il proxy generato consente di usare la sintassi che sembra come se fosse stata eseguita una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` anziché `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="2df57-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="2df57-141">La sintassi del proxy generata consente inoltre un errore sul lato client immediato e intelligibile se si digita un nome di metodo del server in modo non corretta.</span><span class="sxs-lookup"><span data-stu-id="2df57-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="2df57-142">Se si crea manualmente il file che definisce i proxy, è anche possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi del server.</span><span class="sxs-lookup"><span data-stu-id="2df57-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="2df57-143">Si supponga, ad esempio, che nel server sia presente la classe Hub seguente:</span><span class="sxs-lookup"><span data-stu-id="2df57-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="2df57-144">Negli esempi di codice seguenti viene illustrato il codice JavaScript per richiamare il metodo `NewContosoChatMessage` sul server e ricevere chiamate del metodo `addContosoChatMessageToPage` dal server.</span><span class="sxs-lookup"><span data-stu-id="2df57-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="2df57-145">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="2df57-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="2df57-146">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="2df57-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="2df57-147">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="2df57-147">When to use the generated proxy</span></span>

<span data-ttu-id="2df57-148">Se si desidera registrare più gestori eventi per un metodo client chiamato dal server, non è possibile utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="2df57-149">In caso contrario, è possibile scegliere di usare il proxy generato o non in base alla preferenza di codifica.</span><span class="sxs-lookup"><span data-stu-id="2df57-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="2df57-150">Se si sceglie di non usarlo, non è necessario fare riferimento all'URL "SignalR/hub" in un `script` elemento del codice client.</span><span class="sxs-lookup"><span data-stu-id="2df57-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="2df57-151">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="2df57-151">Client setup</span></span>

<span data-ttu-id="2df57-152">Un client JavaScript richiede riferimenti a jQuery e al file JavaScript di base di SignalR.</span><span class="sxs-lookup"><span data-stu-id="2df57-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="2df57-153">La versione jQuery deve essere 1.6.4 o versioni successive, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="2df57-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="2df57-154">Se si decide di usare il proxy generato, è necessario anche un riferimento al file JavaScript del proxy generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="2df57-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="2df57-155">Nell'esempio seguente viene illustrato il modo in cui i riferimenti potrebbero apparire in una pagina HTML che utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="2df57-156">Questi riferimenti devono essere inclusi in questo ordine: jQuery First, SignalR Core dopo tale operazione e i proxy SignalR per ultimi.</span><span class="sxs-lookup"><span data-stu-id="2df57-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="2df57-157">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="2df57-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="2df57-158">Nell'esempio precedente, il riferimento al proxy generato da SignalR è quello di generare dinamicamente codice JavaScript, non di un file fisico.</span><span class="sxs-lookup"><span data-stu-id="2df57-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="2df57-159">SignalR crea il codice JavaScript per il proxy in tempo reale e lo serve al client in risposta all'URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="2df57-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="2df57-160">Se è stato specificato un URL di base diverso per le connessioni SignalR nel server nel metodo `MapHubs`, l'URL per il file proxy generato in modo dinamico è l'URL personalizzato con l'aggiunta di "/Hubs".</span><span class="sxs-lookup"><span data-stu-id="2df57-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="2df57-161">Per i client JavaScript di Windows 8 (Windows Store), usare il file proxy fisico anziché quello generato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="2df57-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="2df57-162">Per ulteriori informazioni, vedere [come creare un file fisico per il proxy generato da SignalR](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="2df57-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="2df57-163">In una visualizzazione Razor ASP.NET MVC 4 usare la tilde per fare riferimento alla radice dell'applicazione nel riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="2df57-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="2df57-164">Per altre informazioni sull'uso di SignalR in MVC 4, vedere [Introduzione con SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="2df57-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="2df57-165">In una visualizzazione Razor di ASP.NET MVC 3 usare `Url.Content` per il riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="2df57-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="2df57-166">In un'applicazione Web Form ASP.NET usare `ResolveClientUrl` per il riferimento al file proxy o registrarlo tramite ScriptManager usando un percorso relativo radice dell'app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="2df57-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="2df57-167">Come regola generale, usare lo stesso metodo per specificare l'URL "/SignalR/Hubs" usato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2df57-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="2df57-168">Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si esegue il test in Visual Studio usando IIS Express ma avrà esito negativo con un errore 404 quando si esegue la distribuzione in IIS completo.</span><span class="sxs-lookup"><span data-stu-id="2df57-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="2df57-169">Per ulteriori informazioni, vedere la pagina relativa alla **risoluzione dei riferimenti alle risorse a livello di radice** in [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) sul sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="2df57-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="2df57-170">Quando si esegue un progetto Web in Visual Studio 2012 in modalità di debug e si usa Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora soluzioni** in documenti di **script**, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="2df57-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![File proxy generato da JavaScript in Esplora soluzioni](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="2df57-172">Per visualizzare il contenuto del file, fare doppio clic su **Hub**.</span><span class="sxs-lookup"><span data-stu-id="2df57-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="2df57-173">Se non si usa Visual Studio 2012 e Internet Explorer o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="2df57-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="2df57-174">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="2df57-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="2df57-175">Come creare un file fisico per il proxy generato da SignalR</span><span class="sxs-lookup"><span data-stu-id="2df57-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="2df57-176">In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="2df57-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="2df57-177">È possibile eseguire questa operazione per controllare la memorizzazione nella cache o la creazione di bundle oppure per ottenere IntelliSense quando si codificano le chiamate ai metodi del server.</span><span class="sxs-lookup"><span data-stu-id="2df57-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="2df57-178">Per creare un file proxy, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="2df57-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="2df57-179">Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="2df57-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="2df57-180">Aprire un prompt dei comandi e passare alla cartella *Tools* che contiene il file SignalR. exe.</span><span class="sxs-lookup"><span data-stu-id="2df57-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="2df57-181">La cartella Tools si trova nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="2df57-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="2df57-182">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2df57-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="2df57-183">Il percorso del file con *estensione dll* è in genere la cartella *bin* nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="2df57-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="2df57-184">Questo comando crea un file denominato *Server. js* nella stessa cartella di *SignalR. exe*.</span><span class="sxs-lookup"><span data-stu-id="2df57-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="2df57-185">Inserire il file *Server. js* in una cartella appropriata nel progetto, rinominarlo nel modo appropriato per l'applicazione e aggiungervi un riferimento al posto del riferimento "SignalR/hub".</span><span class="sxs-lookup"><span data-stu-id="2df57-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="2df57-186">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-186">How to establish a connection</span></span>

<span data-ttu-id="2df57-187">Prima di stabilire una connessione, è necessario creare un oggetto connessione, creare un proxy e registrare i gestori eventi per i metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="2df57-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="2df57-188">Quando vengono configurati i gestori eventi e proxy, stabilire la connessione chiamando il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="2df57-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="2df57-189">Se si utilizza il proxy generato, non è necessario creare l'oggetto connessione nel proprio codice perché il codice proxy generato lo esegue automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2df57-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="2df57-190">**Stabilire una connessione con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="2df57-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="2df57-191">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="2df57-192">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="2df57-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="2df57-193">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="2df57-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="2df57-194">Normalmente si registrano i gestori eventi prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="2df57-195">Se si desidera registrare alcuni gestori eventi dopo avere stabilito la connessione, è possibile eseguire questa operazione, ma è necessario registrare almeno uno dei gestori eventi prima di chiamare il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="2df57-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="2df57-196">Un motivo è che in un'applicazione possono essere presenti molti Hub, ma non si vuole attivare l'evento `OnConnected` in ogni hub se si intende usarlo solo per uno di essi.</span><span class="sxs-lookup"><span data-stu-id="2df57-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="2df57-197">Quando viene stabilita la connessione, la presenza di un metodo client sul proxy di un hub è che indica a SignalR di attivare l'evento `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="2df57-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="2df57-198">Se non si registra alcun gestore eventi prima di chiamare il metodo `start`, sarà possibile richiamare i metodi sull'hub, ma il metodo di `OnConnected` dell'hub non verrà chiamato e non verranno richiamati i metodi client dal server.</span><span class="sxs-lookup"><span data-stu-id="2df57-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="2df57-199">$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()</span><span class="sxs-lookup"><span data-stu-id="2df57-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="2df57-200">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="2df57-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="2df57-201">Si tratta dello stesso oggetto ottenuto chiamando `$.hubConnection()` quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="2df57-202">Il codice proxy generato crea automaticamente la connessione eseguendo l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="2df57-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="2df57-204">Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto Connection quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="2df57-205">Esecuzione asincrona del metodo Start</span><span class="sxs-lookup"><span data-stu-id="2df57-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="2df57-206">Il metodo `start` viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="2df57-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="2df57-207">Restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere funzioni di callback chiamando metodi quali `pipe`, `done`e `fail`.</span><span class="sxs-lookup"><span data-stu-id="2df57-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="2df57-208">Se si dispone di codice che si desidera eseguire dopo che è stata stabilita la connessione, ad esempio una chiamata a un metodo del server, inserire il codice in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="2df57-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="2df57-209">Il metodo di callback `.done` viene eseguito dopo che è stata stabilita la connessione e dopo che il codice presente nel metodo del gestore dell'evento `OnConnected` sul server termina l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2df57-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="2df57-210">Se si inserisce l'istruzione "ora connessa" dell'esempio precedente come riga di codice successiva dopo la chiamata al metodo `start` (non in un callback `.done`), la riga di `console.log` verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2df57-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato di scrivere il codice che viene eseguito dopo che è stata stabilita la connessione](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="2df57-212">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="2df57-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="2df57-213">In genere, se il browser carica una pagina dal `http://contoso.com`, la connessione SignalR si trova nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="2df57-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="2df57-214">Se la pagina di `http://contoso.com` crea una connessione a `http://fabrikam.com/signalr`, ovvero una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="2df57-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="2df57-215">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2df57-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="2df57-216">Per stabilire una connessione tra domini, verificare che le connessioni tra domini siano abilitate nel server e specificare l'URL di connessione quando si crea l'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="2df57-217">SignalR userà la tecnologia appropriata per le connessioni tra domini, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP) o [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="2df57-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="2df57-218">Nel server abilitare le connessioni tra domini selezionando l'opzione quando si chiama il metodo `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="2df57-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="2df57-219">Nel client specificare l'URL quando si crea l'oggetto connessione (senza il proxy generato) o prima di chiamare il metodo Start (con il proxy generato).</span><span class="sxs-lookup"><span data-stu-id="2df57-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="2df57-220">**Codice client che specifica una connessione tra domini (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="2df57-221">**Codice client che specifica una connessione tra domini (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="2df57-222">Quando si usa il costruttore `$.hubConnection`, non è necessario includere `signalr` nell'URL perché viene aggiunto automaticamente (a meno che non si specifichi `useDefaultUrl` come `false`).</span><span class="sxs-lookup"><span data-stu-id="2df57-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="2df57-223">È possibile creare più connessioni a endpoint diversi.</span><span class="sxs-lookup"><span data-stu-id="2df57-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="2df57-224">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="2df57-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Non impostare jQuery. support. CORS su true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="2df57-226">SignalR gestisce l'uso di JSONP o CORS.</span><span class="sxs-lookup"><span data-stu-id="2df57-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="2df57-227">Se si imposta `jQuery.support.cors` su true, il JSONP viene disabilitato perché segnala al browser di supportare CORS.</span><span class="sxs-lookup"><span data-stu-id="2df57-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="2df57-228">Quando ci si connette a un URL localhost, Internet Explorer 10 non lo considererà una connessione tra domini, quindi l'applicazione funzionerà localmente con IE 10 anche se non sono state abilitate connessioni tra domini sul server.</span><span class="sxs-lookup"><span data-stu-id="2df57-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="2df57-229">Per informazioni sull'utilizzo delle connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="2df57-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="2df57-230">Per informazioni sull'uso delle connessioni tra domini con Chrome, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="2df57-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="2df57-231">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="2df57-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="2df57-232">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="2df57-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="2df57-233">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-233">How to configure the connection</span></span>

<span data-ttu-id="2df57-234">Prima di stabilire una connessione, è possibile specificare i parametri della stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="2df57-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="2df57-235">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="2df57-235">How to specify query string parameters</span></span>

<span data-ttu-id="2df57-236">Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="2df57-237">Negli esempi seguenti viene illustrato come impostare un parametro della stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="2df57-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="2df57-238">**Impostare un valore di stringa di query prima di chiamare il metodo Start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="2df57-239">**Impostare un valore di stringa di query prima di chiamare il metodo Start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="2df57-240">Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="2df57-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="2df57-241">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="2df57-241">How to specify the transport method</span></span>

<span data-ttu-id="2df57-242">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client.</span><span class="sxs-lookup"><span data-stu-id="2df57-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="2df57-243">Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="2df57-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="2df57-244">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="2df57-245">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="2df57-246">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si vuole che SignalR li provi:</span><span class="sxs-lookup"><span data-stu-id="2df57-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="2df57-247">**Codice client che specifica uno schema di fallback del trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="2df57-248">**Codice client che specifica uno schema di fallback del trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="2df57-249">Per specificare il metodo di trasporto, è possibile usare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="2df57-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="2df57-250">WebSocket</span><span class="sxs-lookup"><span data-stu-id="2df57-250">"webSockets"</span></span>
- <span data-ttu-id="2df57-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="2df57-251">"foreverFrame"</span></span>
- <span data-ttu-id="2df57-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="2df57-252">"serverSentEvents"</span></span>
- <span data-ttu-id="2df57-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="2df57-253">"longPolling"</span></span>

<span data-ttu-id="2df57-254">Negli esempi seguenti viene illustrato come individuare il metodo di trasporto utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="2df57-255">**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="2df57-256">**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="2df57-257">Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="2df57-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="2df57-258">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="2df57-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="2df57-259">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="2df57-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="2df57-260">Ogni oggetto connessione creato incapsula le informazioni su una connessione a un servizio SignalR che contiene una o più classi Hub.</span><span class="sxs-lookup"><span data-stu-id="2df57-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="2df57-261">Per comunicare con una classe Hub, è possibile usare un oggetto proxy creato dall'utente (se non si usa il proxy generato) o generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2df57-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="2df57-262">Sul client il nome del proxy è una versione con la combinazione di maiuscole e minuscole del nome della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="2df57-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="2df57-263">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2df57-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="2df57-264">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="2df57-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="2df57-265">**Ottenere un riferimento al proxy client generato per l'hub**</span><span class="sxs-lookup"><span data-stu-id="2df57-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="2df57-266">**Crea proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="2df57-267">Se si decora la classe Hub con un attributo `HubName`, usare il nome esatto senza modificare il caso.</span><span class="sxs-lookup"><span data-stu-id="2df57-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="2df57-268">**Classe Hub nel server con attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="2df57-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="2df57-269">**Ottenere un riferimento al proxy client generato per l'hub**</span><span class="sxs-lookup"><span data-stu-id="2df57-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="2df57-270">**Crea proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="2df57-271">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="2df57-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="2df57-272">Per definire un metodo che il server può chiamare da un hub, aggiungere un gestore eventi al proxy dell'hub usando la proprietà `client` del proxy generato oppure chiamare il metodo `on` se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="2df57-273">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="2df57-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="2df57-274">Aggiungere il gestore eventi prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="2df57-275">Se si desidera aggiungere gestori eventi dopo aver chiamato il metodo `start`, vedere la nota in [come stabilire una connessione](#establishconnection) in precedenza in questo documento e utilizzare la sintassi illustrata per la definizione di un metodo senza utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="2df57-276">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="2df57-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="2df57-277">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` nel client.</span><span class="sxs-lookup"><span data-stu-id="2df57-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="2df57-278">**Metodo define sul client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="2df57-279">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="2df57-280">**Metodo define sul client (senza il proxy generato o quando viene aggiunto dopo la chiamata al metodo Start)**</span><span class="sxs-lookup"><span data-stu-id="2df57-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="2df57-281">**Codice server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="2df57-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="2df57-282">Gli esempi seguenti includono un oggetto complesso come parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="2df57-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="2df57-283">**Definire il metodo nel client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="2df57-284">**Definire il metodo nel client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="2df57-285">**Codice server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="2df57-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="2df57-286">**Codice server che chiama il metodo client usando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="2df57-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="2df57-287">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="2df57-287">How to call server methods from the client</span></span>

<span data-ttu-id="2df57-288">Per chiamare un metodo Server dal client, usare la proprietà `server` del proxy generato o il metodo `invoke` sul proxy hub se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="2df57-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="2df57-289">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="2df57-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="2df57-290">Passare una versione con maiuscole e minuscole del nome del metodo nell'hub.</span><span class="sxs-lookup"><span data-stu-id="2df57-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="2df57-291">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2df57-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="2df57-292">Negli esempi seguenti viene illustrato come chiamare un metodo del server che non ha un valore restituito e come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="2df57-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="2df57-293">**Metodo Server senza attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="2df57-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="2df57-294">**Codice server che definisce l'oggetto complesso passato in un parametro**</span><span class="sxs-lookup"><span data-stu-id="2df57-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="2df57-295">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="2df57-296">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="2df57-297">Se il metodo Hub è stato decorato con un attributo `HubMethodName`, usare tale nome senza modificare il caso.</span><span class="sxs-lookup"><span data-stu-id="2df57-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="2df57-298">**Metodo Server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="2df57-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="2df57-299">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="2df57-300">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="2df57-301">Negli esempi precedenti viene illustrato come chiamare un metodo Server senza valore restituito.</span><span class="sxs-lookup"><span data-stu-id="2df57-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="2df57-302">Negli esempi seguenti viene illustrato come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="2df57-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="2df57-303">**Codice server per un metodo che ha un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="2df57-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="2df57-304">**Classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="2df57-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="2df57-305">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="2df57-306">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="2df57-307">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="2df57-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="2df57-308">SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="2df57-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="2df57-309">`starting`: generato prima che i dati vengano inviati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="2df57-310">`received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="2df57-311">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="2df57-311">Provides the received data.</span></span>
- <span data-ttu-id="2df57-312">`connectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.</span><span class="sxs-lookup"><span data-stu-id="2df57-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="2df57-313">`reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="2df57-314">`reconnected`: generato quando il trasporto sottostante si è riconnesso.</span><span class="sxs-lookup"><span data-stu-id="2df57-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="2df57-315">`stateChanged`: generato quando viene modificato lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="2df57-316">Fornisce lo stato precedente e il nuovo stato (connessione, connessione, riconnessione o disconnessione).</span><span class="sxs-lookup"><span data-stu-id="2df57-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="2df57-317">`disconnected`: generato quando la connessione è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="2df57-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="2df57-318">Se, ad esempio, si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che possono causare ritardi evidenti, gestire l'evento `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="2df57-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="2df57-319">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="2df57-320">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="2df57-321">Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="2df57-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="2df57-322">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="2df57-322">How to handle errors</span></span>

<span data-ttu-id="2df57-323">Il client JavaScript SignalR fornisce un evento `error` per cui è possibile aggiungere un gestore.</span><span class="sxs-lookup"><span data-stu-id="2df57-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="2df57-324">È anche possibile usare il metodo Fail per aggiungere un gestore per gli errori risultanti da una chiamata al metodo Server.</span><span class="sxs-lookup"><span data-stu-id="2df57-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="2df57-325">Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="2df57-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="2df57-326">Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="2df57-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="2df57-327">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="2df57-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="2df57-328">**Aggiungere un gestore errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="2df57-329">**Aggiungere un gestore errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="2df57-330">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="2df57-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="2df57-331">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="2df57-332">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="2df57-333">Se una chiamata al metodo ha esito negativo, viene generato anche l'evento `error`, quindi il codice nel gestore del metodo `error` e nel callback del metodo di `.fail` verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="2df57-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="2df57-334">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="2df57-334">How to enable client-side logging</span></span>

<span data-ttu-id="2df57-335">Per abilitare la registrazione lato client su una connessione, impostare la proprietà `logging` sull'oggetto connessione prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="2df57-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="2df57-336">**Abilitare la registrazione con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="2df57-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="2df57-337">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="2df57-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="2df57-338">Per visualizzare i log, aprire gli strumenti di sviluppo del browser e passare alla scheda della console. Per un'esercitazione in cui vengono illustrate le istruzioni dettagliate e le schermate che illustrano come eseguire questa operazione, vedere [server broadcast with ASP.NET SignalR-Enable logging](index.md).</span><span class="sxs-lookup"><span data-stu-id="2df57-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
