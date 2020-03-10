---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida dell'API Hub SignalR ASP.NET-client JavaScript | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client JavaScript, ad esempio i browser e domanda di Windows Store (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536659"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="c71db-103">Guida dell'API Hub SignalR ASP.NET-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c71db-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c71db-104">Questo documento fornisce un'introduzione all'uso dell'API hub per SignalR versione 2 nei client JavaScript, ad esempio i browser e le applicazioni di Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="c71db-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="c71db-105">L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="c71db-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="c71db-106">Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="c71db-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="c71db-107">Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="c71db-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="c71db-108">SignalR si occupa automaticamente di tutte le tubature da client a server.</span><span class="sxs-lookup"><span data-stu-id="c71db-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="c71db-109">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="c71db-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="c71db-110">Per un'introduzione a SignalR, Hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c71db-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c71db-111">Versioni del software usate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="c71db-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="c71db-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c71db-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="c71db-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c71db-113">.NET 4.5</span></span>
> - <span data-ttu-id="c71db-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="c71db-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="c71db-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="c71db-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="c71db-116">Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c71db-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c71db-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="c71db-117">Questions and comments</span></span>
>
> <span data-ttu-id="c71db-118">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="c71db-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c71db-119">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c71db-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="c71db-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c71db-120">Overview</span></span>

<span data-ttu-id="c71db-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="c71db-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="c71db-122">Il proxy generato e la relativa funzione</span><span class="sxs-lookup"><span data-stu-id="c71db-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="c71db-123">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="c71db-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="c71db-124">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="c71db-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="c71db-125">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="c71db-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="c71db-126">Come creare un file fisico per il proxy generato da SignalR</span><span class="sxs-lookup"><span data-stu-id="c71db-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="c71db-127">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="c71db-128">$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()</span><span class="sxs-lookup"><span data-stu-id="c71db-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="c71db-129">Esecuzione asincrona del metodo Start</span><span class="sxs-lookup"><span data-stu-id="c71db-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="c71db-130">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="c71db-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="c71db-131">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="c71db-132">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="c71db-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="c71db-133">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="c71db-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="c71db-134">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="c71db-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="c71db-135">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c71db-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="c71db-136">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="c71db-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="c71db-137">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="c71db-138">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="c71db-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="c71db-139">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="c71db-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="c71db-140">Per la documentazione su come programmare il server o i client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="c71db-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="c71db-141">Guida dell'API degli hub SignalR-server</span><span class="sxs-lookup"><span data-stu-id="c71db-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="c71db-142">Guida dell'API degli hub SignalR-client .NET</span><span class="sxs-lookup"><span data-stu-id="c71db-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="c71db-143">Il componente server SignalR 2 è disponibile solo in .NET 4,5 (anche se è presente un client .NET per SignalR 2 in .NET 4,0).</span><span class="sxs-lookup"><span data-stu-id="c71db-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="c71db-144">Il proxy generato e la relativa funzione</span><span class="sxs-lookup"><span data-stu-id="c71db-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="c71db-145">È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy generato automaticamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="c71db-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="c71db-146">Il proxy per consente di semplificare la sintassi del codice usato per connettersi, scrivere metodi che il server chiama e chiamare metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="c71db-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="c71db-147">Quando si scrive il codice per chiamare i metodi del server, il proxy generato consente di usare la sintassi che sembra come se fosse stata eseguita una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` anziché `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="c71db-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="c71db-148">La sintassi del proxy generata consente inoltre un errore sul lato client immediato e intelligibile se si digita un nome di metodo del server in modo non corretta.</span><span class="sxs-lookup"><span data-stu-id="c71db-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="c71db-149">Se si crea manualmente il file che definisce i proxy, è anche possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi del server.</span><span class="sxs-lookup"><span data-stu-id="c71db-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="c71db-150">Si supponga, ad esempio, che nel server sia presente la classe Hub seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="c71db-151">Negli esempi di codice seguenti viene illustrato il codice JavaScript per richiamare il metodo `NewContosoChatMessage` sul server e ricevere chiamate del metodo `addContosoChatMessageToPage` dal server.</span><span class="sxs-lookup"><span data-stu-id="c71db-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="c71db-152">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c71db-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="c71db-153">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c71db-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="c71db-154">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="c71db-154">When to use the generated proxy</span></span>

<span data-ttu-id="c71db-155">Se si desidera registrare più gestori eventi per un metodo client chiamato dal server, non è possibile utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="c71db-156">In caso contrario, è possibile scegliere di usare il proxy generato o non in base alla preferenza di codifica.</span><span class="sxs-lookup"><span data-stu-id="c71db-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="c71db-157">Se si sceglie di non usarlo, non è necessario fare riferimento all'URL "SignalR/hub" in un `script` elemento del codice client.</span><span class="sxs-lookup"><span data-stu-id="c71db-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="c71db-158">Configurazione client</span><span class="sxs-lookup"><span data-stu-id="c71db-158">Client setup</span></span>

<span data-ttu-id="c71db-159">Un client JavaScript richiede riferimenti a jQuery e al file JavaScript di base di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c71db-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="c71db-160">La versione jQuery deve essere 1.6.4 o versioni successive, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="c71db-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="c71db-161">Se si decide di usare il proxy generato, è necessario anche un riferimento al file JavaScript del proxy generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="c71db-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="c71db-162">Nell'esempio seguente viene illustrato il modo in cui i riferimenti potrebbero apparire in una pagina HTML che utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="c71db-163">Questi riferimenti devono essere inclusi in questo ordine: jQuery First, SignalR Core dopo tale operazione e i proxy SignalR per ultimi.</span><span class="sxs-lookup"><span data-stu-id="c71db-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="c71db-164">Come fare riferimento al proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="c71db-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="c71db-165">Nell'esempio precedente, il riferimento al proxy generato da SignalR è quello di generare dinamicamente codice JavaScript, non di un file fisico.</span><span class="sxs-lookup"><span data-stu-id="c71db-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="c71db-166">SignalR crea il codice JavaScript per il proxy in tempo reale e lo serve al client in risposta all'URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="c71db-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="c71db-167">Se è stato specificato un URL di base diverso per le connessioni SignalR nel server nel metodo `MapSignalR`, l'URL per il file proxy generato in modo dinamico è l'URL personalizzato con l'aggiunta di "/Hubs".</span><span class="sxs-lookup"><span data-stu-id="c71db-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="c71db-168">Per i client JavaScript di Windows 8 (Windows Store), usare il file proxy fisico anziché quello generato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="c71db-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="c71db-169">Per ulteriori informazioni, vedere [come creare un file fisico per il proxy generato da SignalR](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c71db-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="c71db-170">In una visualizzazione Razor ASP.NET MVC 4 o 5 Razor usare la tilde per fare riferimento alla radice dell'applicazione nel riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="c71db-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="c71db-171">Per altre informazioni sull'uso di SignalR in MVC 5, vedere [Introduzione con SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c71db-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="c71db-172">In una visualizzazione Razor di ASP.NET MVC 3 usare `Url.Content` per il riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="c71db-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="c71db-173">In un'applicazione Web Form ASP.NET usare `ResolveClientUrl` per il riferimento al file proxy o registrarlo tramite ScriptManager usando un percorso relativo radice dell'app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="c71db-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="c71db-174">Come regola generale, usare lo stesso metodo per specificare l'URL "/SignalR/Hubs" usato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c71db-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="c71db-175">Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si esegue il test in Visual Studio usando IIS Express ma avrà esito negativo con un errore 404 quando si esegue la distribuzione in IIS completo.</span><span class="sxs-lookup"><span data-stu-id="c71db-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="c71db-176">Per ulteriori informazioni, vedere la pagina relativa alla **risoluzione dei riferimenti alle risorse a livello di radice** in [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) sul sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="c71db-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="c71db-177">Quando si esegue un progetto Web in Visual Studio 2017 in modalità di debug e si usa Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora soluzioni** in **script**.</span><span class="sxs-lookup"><span data-stu-id="c71db-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="c71db-178">Per visualizzare il contenuto del file, fare doppio clic su **Hub**.</span><span class="sxs-lookup"><span data-stu-id="c71db-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="c71db-179">Se non si usa Visual Studio 2012 o 2013 e Internet Explorer o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="c71db-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="c71db-180">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="c71db-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="c71db-181">Come creare un file fisico per il proxy generato da SignalR</span><span class="sxs-lookup"><span data-stu-id="c71db-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="c71db-182">In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="c71db-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="c71db-183">È possibile eseguire questa operazione per controllare la memorizzazione nella cache o la creazione di bundle oppure per ottenere IntelliSense quando si codificano le chiamate ai metodi del server.</span><span class="sxs-lookup"><span data-stu-id="c71db-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="c71db-184">Per creare un file proxy, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="c71db-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="c71db-185">Installare il pacchetto NuGet [Microsoft. AspNet. SignalR. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="c71db-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="c71db-186">Aprire un prompt dei comandi e passare alla cartella *Tools* che contiene il file SignalR. exe.</span><span class="sxs-lookup"><span data-stu-id="c71db-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="c71db-187">La cartella Tools si trova nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="c71db-188">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="c71db-189">Il percorso del file con *estensione dll* è in genere la cartella *bin* nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="c71db-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="c71db-190">Questo comando crea un file denominato *Server. js* nella stessa cartella di *SignalR. exe*.</span><span class="sxs-lookup"><span data-stu-id="c71db-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="c71db-191">Inserire il file *Server. js* in una cartella appropriata nel progetto, rinominarlo nel modo appropriato per l'applicazione e aggiungervi un riferimento al posto del riferimento "SignalR/hub".</span><span class="sxs-lookup"><span data-stu-id="c71db-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="c71db-192">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-192">How to establish a connection</span></span>

<span data-ttu-id="c71db-193">Prima di stabilire una connessione, è necessario creare un oggetto connessione, creare un proxy e registrare i gestori eventi per i metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="c71db-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="c71db-194">Quando vengono configurati i gestori eventi e proxy, stabilire la connessione chiamando il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="c71db-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="c71db-195">Se si utilizza il proxy generato, non è necessario creare l'oggetto connessione nel proprio codice perché il codice proxy generato lo esegue automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c71db-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="c71db-196">**Stabilire una connessione con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c71db-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="c71db-197">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="c71db-198">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="c71db-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="c71db-199">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="c71db-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="c71db-200">Per impostazione predefinita, la posizione dell'hub è il server corrente. Se ci si connette a un server diverso, specificare l'URL prima di chiamare il metodo `start`, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="c71db-201">Normalmente si registrano i gestori eventi prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="c71db-202">Se si desidera registrare alcuni gestori eventi dopo avere stabilito la connessione, è possibile eseguire questa operazione, ma è necessario registrare almeno uno dei gestori eventi prima di chiamare il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="c71db-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="c71db-203">Un motivo è che in un'applicazione possono essere presenti molti Hub, ma non si vuole attivare l'evento `OnConnected` in ogni hub se si intende usarlo solo per uno di essi.</span><span class="sxs-lookup"><span data-stu-id="c71db-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="c71db-204">Quando viene stabilita la connessione, la presenza di un metodo client sul proxy di un hub è che indica a SignalR di attivare l'evento `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="c71db-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="c71db-205">Se non si registra alcun gestore eventi prima di chiamare il metodo `start`, sarà possibile richiamare i metodi sull'hub, ma il metodo di `OnConnected` dell'hub non verrà chiamato e non verranno richiamati i metodi client dal server.</span><span class="sxs-lookup"><span data-stu-id="c71db-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="c71db-206">$. Connection. Hub è lo stesso oggetto creato da $. hubConnection ()</span><span class="sxs-lookup"><span data-stu-id="c71db-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="c71db-207">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto Connection.</span><span class="sxs-lookup"><span data-stu-id="c71db-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="c71db-208">Si tratta dello stesso oggetto ottenuto chiamando `$.hubConnection()` quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="c71db-209">Il codice proxy generato crea automaticamente la connessione eseguendo l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="c71db-211">Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto Connection quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="c71db-212">Esecuzione asincrona del metodo Start</span><span class="sxs-lookup"><span data-stu-id="c71db-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="c71db-213">Il metodo `start` viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="c71db-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="c71db-214">Restituisce un [oggetto rinviato jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere funzioni di callback chiamando metodi quali `pipe`, `done`e `fail`.</span><span class="sxs-lookup"><span data-stu-id="c71db-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="c71db-215">Se si dispone di codice che si desidera eseguire dopo che è stata stabilita la connessione, ad esempio una chiamata a un metodo del server, inserire il codice in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="c71db-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="c71db-216">Il metodo di callback `.done` viene eseguito dopo che è stata stabilita la connessione e dopo che il codice presente nel metodo del gestore dell'evento `OnConnected` sul server termina l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c71db-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="c71db-217">Se si inserisce l'istruzione "ora connessa" dell'esempio precedente come riga di codice successiva dopo la chiamata al metodo `start` (non in un callback `.done`), la riga di `console.log` verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c71db-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato di scrivere il codice che viene eseguito dopo che è stata stabilita la connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="c71db-219">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="c71db-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="c71db-220">In genere, se il browser carica una pagina dal `http://contoso.com`, la connessione SignalR si trova nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="c71db-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="c71db-221">Se la pagina di `http://contoso.com` crea una connessione a `http://fabrikam.com/signalr`, ovvero una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="c71db-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="c71db-222">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c71db-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="c71db-223">In SignalR 1. x, le richieste tra domini erano controllate da un singolo flag EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="c71db-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="c71db-224">Questo flag controlla sia le richieste JSONP che CORS.</span><span class="sxs-lookup"><span data-stu-id="c71db-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="c71db-225">Per maggiore flessibilità, tutto il supporto per CORS è stato rimosso dal componente server di SignalR (i client JavaScript usano ancora CORS normalmente se è stato rilevato che il browser lo supporta) e il nuovo middleware OWIN è stato reso disponibile per supportare questi scenari.</span><span class="sxs-lookup"><span data-stu-id="c71db-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="c71db-226">Se JSONP è richiesto sul client (per supportare le richieste tra domini in browser meno recenti), sarà necessario abilitarlo in modo esplicito impostando `EnableJSONP` sull'oggetto `HubConfiguration` per `true`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c71db-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="c71db-227">JSONP è disabilitato per impostazione predefinita, in quanto è meno sicuro di CORS.</span><span class="sxs-lookup"><span data-stu-id="c71db-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="c71db-228">**Aggiunta di Microsoft. Owin. CORS al progetto:** Per installare questa libreria, eseguire il comando seguente nella console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="c71db-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="c71db-229">Questo comando consente di aggiungere la versione 2.1.0 del pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="c71db-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="c71db-230">Chiamata di UseCors</span><span class="sxs-lookup"><span data-stu-id="c71db-230">Calling UseCors</span></span>

 <span data-ttu-id="c71db-231">Il frammento di codice seguente illustra come implementare le connessioni tra domini in SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="c71db-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="c71db-232">**Implementazione di richieste tra domini in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="c71db-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="c71db-233">Il codice seguente illustra come abilitare CORS o JSONP in un progetto SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="c71db-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="c71db-234">Questo esempio di codice USA `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS venga eseguito solo per le richieste SignalR che richiedono il supporto di CORS (anziché tutto il traffico nel percorso specificato in `MapSignalR`). Map può essere usato anche per qualsiasi altro middleware che deve essere eseguito per un prefisso URL specifico, anziché per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="c71db-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="c71db-235">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="c71db-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Non impostare jQuery. support. CORS su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="c71db-237">SignalR gestisce l'uso di CORS.</span><span class="sxs-lookup"><span data-stu-id="c71db-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="c71db-238">Se si imposta `jQuery.support.cors` su true, il JSONP viene disabilitato perché segnala al browser di supportare CORS.</span><span class="sxs-lookup"><span data-stu-id="c71db-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="c71db-239">Quando ci si connette a un URL localhost, Internet Explorer 10 non lo considererà una connessione tra domini, quindi l'applicazione funzionerà localmente con IE 10 anche se non sono state abilitate connessioni tra domini sul server.</span><span class="sxs-lookup"><span data-stu-id="c71db-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="c71db-240">Per informazioni sull'utilizzo delle connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="c71db-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="c71db-241">Per informazioni sull'uso delle connessioni tra domini con Chrome, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="c71db-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="c71db-242">Il codice di esempio usa l'URL predefinito "/SignalR" per connettersi al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="c71db-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="c71db-243">Per informazioni su come specificare un URL di base diverso, vedere [ASP.NET SignalR Hub API guide-Server-l'URL/SignalR](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="c71db-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="c71db-244">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-244">How to configure the connection</span></span>

<span data-ttu-id="c71db-245">Prima di stabilire una connessione, è possibile specificare i parametri della stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="c71db-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="c71db-246">Come specificare i parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="c71db-246">How to specify query string parameters</span></span>

<span data-ttu-id="c71db-247">Se si desidera inviare dati al server quando si connette il client, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="c71db-248">Negli esempi seguenti viene illustrato come impostare un parametro della stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="c71db-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="c71db-249">**Impostare un valore di stringa di query prima di chiamare il metodo Start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="c71db-250">**Impostare un valore di stringa di query prima di chiamare il metodo Start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="c71db-251">Nell'esempio seguente viene illustrato come leggere un parametro della stringa di query nel codice del server.</span><span class="sxs-lookup"><span data-stu-id="c71db-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="c71db-252">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="c71db-252">How to specify the transport method</span></span>

<span data-ttu-id="c71db-253">Come parte del processo di connessione, un client SignalR in genere negozia con il server per determinare il trasporto migliore supportato da server e client.</span><span class="sxs-lookup"><span data-stu-id="c71db-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="c71db-254">Se si conosce già il trasporto che si vuole usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il metodo `start`.</span><span class="sxs-lookup"><span data-stu-id="c71db-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="c71db-255">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="c71db-256">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="c71db-257">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si vuole che SignalR li provi:</span><span class="sxs-lookup"><span data-stu-id="c71db-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="c71db-258">**Codice client che specifica uno schema di fallback del trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="c71db-259">**Codice client che specifica uno schema di fallback del trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="c71db-260">Per specificare il metodo di trasporto, è possibile usare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="c71db-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="c71db-261">WebSocket</span><span class="sxs-lookup"><span data-stu-id="c71db-261">"webSockets"</span></span>
- <span data-ttu-id="c71db-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="c71db-262">"foreverFrame"</span></span>
- <span data-ttu-id="c71db-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="c71db-263">"serverSentEvents"</span></span>
- <span data-ttu-id="c71db-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="c71db-264">"longPolling"</span></span>

<span data-ttu-id="c71db-265">Negli esempi seguenti viene illustrato come individuare il metodo di trasporto utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="c71db-266">**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="c71db-267">**Codice client che Visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="c71db-268">Per informazioni su come controllare il metodo di trasporto in codice server, vedere [Guida all'API di hub signalr ASP.NET-Server-come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="c71db-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="c71db-269">Per ulteriori informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR-trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="c71db-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="c71db-270">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="c71db-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="c71db-271">Ogni oggetto connessione creato incapsula le informazioni su una connessione a un servizio SignalR che contiene una o più classi Hub.</span><span class="sxs-lookup"><span data-stu-id="c71db-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="c71db-272">Per comunicare con una classe Hub, è possibile usare un oggetto proxy creato dall'utente (se non si usa il proxy generato) o generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c71db-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="c71db-273">Sul client il nome del proxy è una versione con la combinazione di maiuscole e minuscole del nome della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="c71db-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="c71db-274">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c71db-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="c71db-275">**Classe Hub nel server**</span><span class="sxs-lookup"><span data-stu-id="c71db-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="c71db-276">**Ottenere un riferimento al proxy client generato per l'hub**</span><span class="sxs-lookup"><span data-stu-id="c71db-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="c71db-277">**Crea proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="c71db-278">Se si decora la classe Hub con un attributo `HubName`, usare il nome esatto senza modificare il caso.</span><span class="sxs-lookup"><span data-stu-id="c71db-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="c71db-279">**Classe Hub nel server con attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="c71db-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="c71db-280">**Ottenere un riferimento al proxy client generato per l'hub**</span><span class="sxs-lookup"><span data-stu-id="c71db-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="c71db-281">**Crea proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="c71db-282">Come definire metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="c71db-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="c71db-283">Per definire un metodo che il server può chiamare da un hub, aggiungere un gestore eventi al proxy dell'hub usando la proprietà `client` del proxy generato oppure chiamare il metodo `on` se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="c71db-284">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="c71db-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="c71db-285">Aggiungere il gestore eventi prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="c71db-286">Se si desidera aggiungere gestori eventi dopo aver chiamato il metodo `start`, vedere la nota in [come stabilire una connessione](#establishconnection) in precedenza in questo documento e utilizzare la sintassi illustrata per la definizione di un metodo senza utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="c71db-287">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c71db-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="c71db-288">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` nel client.</span><span class="sxs-lookup"><span data-stu-id="c71db-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="c71db-289">**Metodo define sul client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="c71db-290">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="c71db-291">**Metodo define sul client (senza il proxy generato o quando viene aggiunto dopo la chiamata al metodo Start)**</span><span class="sxs-lookup"><span data-stu-id="c71db-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="c71db-292">**Codice server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="c71db-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="c71db-293">Gli esempi seguenti includono un oggetto complesso come parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="c71db-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="c71db-294">**Definire il metodo nel client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="c71db-295">**Definire il metodo nel client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="c71db-296">**Codice server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="c71db-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="c71db-297">**Codice server che chiama il metodo client usando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="c71db-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="c71db-298">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="c71db-298">How to call server methods from the client</span></span>

<span data-ttu-id="c71db-299">Per chiamare un metodo Server dal client, usare la proprietà `server` del proxy generato o il metodo `invoke` sul proxy hub se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c71db-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="c71db-300">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="c71db-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="c71db-301">Passare una versione con maiuscole e minuscole del nome del metodo nell'hub.</span><span class="sxs-lookup"><span data-stu-id="c71db-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="c71db-302">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c71db-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="c71db-303">Negli esempi seguenti viene illustrato come chiamare un metodo del server che non ha un valore restituito e come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="c71db-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="c71db-304">**Metodo Server senza attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="c71db-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="c71db-305">**Codice server che definisce l'oggetto complesso passato in un parametro**</span><span class="sxs-lookup"><span data-stu-id="c71db-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="c71db-306">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="c71db-307">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="c71db-308">Se il metodo Hub è stato decorato con un attributo `HubMethodName`, usare tale nome senza modificare il caso.</span><span class="sxs-lookup"><span data-stu-id="c71db-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="c71db-309">**Metodo Server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="c71db-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="c71db-310">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="c71db-311">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="c71db-312">Negli esempi precedenti viene illustrato come chiamare un metodo Server senza valore restituito.</span><span class="sxs-lookup"><span data-stu-id="c71db-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="c71db-313">Negli esempi seguenti viene illustrato come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="c71db-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="c71db-314">**Codice server per un metodo che ha un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="c71db-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="c71db-315">**Classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="c71db-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="c71db-316">**Codice client che richiama il metodo Server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="c71db-317">**Codice client che richiama il metodo Server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="c71db-318">Come gestire gli eventi di durata della connessione</span><span class="sxs-lookup"><span data-stu-id="c71db-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="c71db-319">SignalR fornisce gli eventi di durata della connessione seguenti che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="c71db-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="c71db-320">`starting`: generato prima che i dati vengano inviati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="c71db-321">`received`: generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="c71db-322">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="c71db-322">Provides the received data.</span></span>
- <span data-ttu-id="c71db-323">`connectionSlow`: generato quando il client rileva una connessione lenta o di rilascio frequente.</span><span class="sxs-lookup"><span data-stu-id="c71db-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="c71db-324">`reconnecting`: generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="c71db-325">`reconnected`: generato quando il trasporto sottostante si è riconnesso.</span><span class="sxs-lookup"><span data-stu-id="c71db-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="c71db-326">`stateChanged`: generato quando viene modificato lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="c71db-327">Fornisce lo stato precedente e il nuovo stato (connessione, connessione, riconnessione o disconnessione).</span><span class="sxs-lookup"><span data-stu-id="c71db-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="c71db-328">`disconnected`: generato quando la connessione è disconnessa.</span><span class="sxs-lookup"><span data-stu-id="c71db-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="c71db-329">Se, ad esempio, si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che possono causare ritardi evidenti, gestire l'evento `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="c71db-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="c71db-330">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="c71db-331">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="c71db-332">Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="c71db-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="c71db-333">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="c71db-333">How to handle errors</span></span>

<span data-ttu-id="c71db-334">Il client JavaScript SignalR fornisce un evento `error` per cui è possibile aggiungere un gestore.</span><span class="sxs-lookup"><span data-stu-id="c71db-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="c71db-335">È anche possibile usare il metodo Fail per aggiungere un gestore per gli errori risultanti da una chiamata al metodo Server.</span><span class="sxs-lookup"><span data-stu-id="c71db-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="c71db-336">Se non si Abilita in modo esplicito messaggi di errore dettagliati sul server, l'oggetto eccezione restituito da SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="c71db-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="c71db-337">Se, ad esempio, una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati ai client in produzione non è consigliabile per motivi di sicurezza, ma se si desidera abilitare messaggi di errore dettagliati per la risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="c71db-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="c71db-338">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="c71db-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="c71db-339">**Aggiungere un gestore errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="c71db-340">**Aggiungere un gestore errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="c71db-341">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="c71db-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="c71db-342">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="c71db-343">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="c71db-344">Se una chiamata al metodo ha esito negativo, viene generato anche l'evento `error`, quindi il codice nel gestore del metodo `error` e nel callback del metodo di `.fail` verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="c71db-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="c71db-345">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="c71db-345">How to enable client-side logging</span></span>

<span data-ttu-id="c71db-346">Per abilitare la registrazione lato client su una connessione, impostare la proprietà `logging` sull'oggetto connessione prima di chiamare il metodo `start` per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="c71db-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="c71db-347">**Abilitare la registrazione con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c71db-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="c71db-348">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c71db-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="c71db-349">Per visualizzare i log, aprire gli strumenti di sviluppo del browser e passare alla scheda della console. Per un'esercitazione in cui vengono illustrate le istruzioni dettagliate e le schermate che illustrano come eseguire questa operazione, vedere [server broadcast with ASP.NET SignalR-Enable logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="c71db-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
