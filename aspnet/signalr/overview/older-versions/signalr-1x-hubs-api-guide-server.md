---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guida dell'API Hub SignalR ASP.NET-Server (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Questo documento fornisce un'introduzione alla programmazione del lato server dell'API Hub SignalR ASP.NET per SignalR versione 1,1, con esempi di codice che dimostrano...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: d9cd3fad36c0300d96c6dbdc61291ef119da2327
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579639"
---
# <a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="9b36f-103">Guida dell'API Hub SignalR ASP.NET-Server (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="9b36f-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>

<span data-ttu-id="9b36f-104">di [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9b36f-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="9b36f-105">Questo documento fornisce un'introduzione alla programmazione del lato server dell'API Hub SignalR ASP.NET per SignalR versione 1,1, con esempi di codice che illustrano le opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="9b36f-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="9b36f-106">L'API degli hub SignalR consente di eseguire chiamate a procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="9b36f-107">Nel codice del server si definiscono metodi che possono essere chiamati dai client e si chiamano metodi che vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="9b36f-108">Nel codice client si definiscono metodi che possono essere chiamati dal server e si chiamano metodi che vengono eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="9b36f-109">SignalR si occupa automaticamente di tutte le tubature da client a server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="9b36f-110">SignalR offre anche un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="9b36f-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="9b36f-111">Per un'introduzione a SignalR, Hub e connessioni permanenti oppure per un'esercitazione che illustra come creare un'applicazione SignalR completa, vedere [SignalR-Introduzione](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="9b36f-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9b36f-112">Overview</span></span>

<span data-ttu-id="9b36f-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="9b36f-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="9b36f-114">Come registrare la route SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="9b36f-115">URL di/SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="9b36f-116">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="9b36f-117">Come creare e usare classi Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="9b36f-118">Durata degli oggetti Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="9b36f-119">Combinazione di maiuscole e minuscole dei nomi di Hub nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b36f-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="9b36f-120">Hub multipli</span><span class="sxs-lookup"><span data-stu-id="9b36f-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="9b36f-121">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="9b36f-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="9b36f-122">Combinazione di maiuscole e minuscole dei nomi di metodo nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b36f-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="9b36f-123">Quando eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="9b36f-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="9b36f-124">Definizione degli overload</span><span class="sxs-lookup"><span data-stu-id="9b36f-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="9b36f-125">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="9b36f-126">Selezione dei client che riceveranno la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="9b36f-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="9b36f-127">Nessuna convalida in fase di compilazione per i nomi dei metodi</span><span class="sxs-lookup"><span data-stu-id="9b36f-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="9b36f-128">Corrispondenza nome metodo senza distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="9b36f-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="9b36f-129">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="9b36f-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="9b36f-130">Come gestire l'appartenenza a un gruppo dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="9b36f-131">Esecuzione asincrona di metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="9b36f-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="9b36f-132">Persistenza appartenenza a gruppi</span><span class="sxs-lookup"><span data-stu-id="9b36f-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="9b36f-133">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="9b36f-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="9b36f-134">Come gestire gli eventi di durata della connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="9b36f-135">Quando OnConnected, Disconnected e OnReconnected vengono chiamati</span><span class="sxs-lookup"><span data-stu-id="9b36f-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="9b36f-136">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="9b36f-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="9b36f-137">Come ottenere informazioni sul client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="9b36f-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="9b36f-138">Come passare lo stato tra i client e la classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="9b36f-139">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="9b36f-140">Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="9b36f-141">Chiamata di metodi client</span><span class="sxs-lookup"><span data-stu-id="9b36f-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="9b36f-142">Gestione dell'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="9b36f-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="9b36f-143">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="9b36f-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="9b36f-144">Come personalizzare la pipeline degli hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="9b36f-145">Per la documentazione relativa alla modalità di programmazione dei client, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b36f-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="9b36f-146">Guida dell'API degli hub SignalR-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b36f-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="9b36f-147">Guida dell'API degli hub SignalR-client .NET</span><span class="sxs-lookup"><span data-stu-id="9b36f-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="9b36f-148">I collegamenti agli argomenti di riferimento sulle API sono relativi alla versione .NET 4,5 dell'API.</span><span class="sxs-lookup"><span data-stu-id="9b36f-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="9b36f-149">Se si usa .NET 4, vedere [la versione .NET 4 degli argomenti dell'API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b36f-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="9b36f-150">Come registrare la route SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="9b36f-151">Per definire la route che i client utilizzeranno per connettersi all'hub, chiamare il metodo [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="9b36f-152">`MapHubs` è un [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per la classe `System.Web.Routing.RouteCollection`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="9b36f-153">Nell'esempio seguente viene illustrato come definire la route degli hub SignalR nel file *Global. asax* .</span><span class="sxs-lookup"><span data-stu-id="9b36f-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="9b36f-154">Se si aggiunge la funzionalità SignalR a un'applicazione ASP.NET MVC, assicurarsi che la route SignalR venga aggiunta prima delle altre route.</span><span class="sxs-lookup"><span data-stu-id="9b36f-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="9b36f-155">Per altre informazioni, vedere [esercitazione: introduzione con SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="9b36f-156">URL di/SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-156">The /signalr URL</span></span>

<span data-ttu-id="9b36f-157">Per impostazione predefinita, l'URL di route che i client utilizzeranno per connettersi all'hub è "/SignalR".</span><span class="sxs-lookup"><span data-stu-id="9b36f-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="9b36f-158">Non confondere questo URL con l'URL "/SignalR/Hubs", che è per il file JavaScript generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="9b36f-159">Per altre informazioni sul proxy generato, vedere [Guida dell'API degli hub SignalR-client JavaScript-il proxy generato e la relativa](index.md)funzione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="9b36f-160">Potrebbero esserci circostanze eccezionali che rendono questo URL di base non utilizzabile per SignalR; ad esempio, è presente una cartella nel progetto denominata *SignalR* e non si vuole modificare il nome.</span><span class="sxs-lookup"><span data-stu-id="9b36f-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="9b36f-161">In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/SignalR" nel codice di esempio con l'URL desiderato).</span><span class="sxs-lookup"><span data-stu-id="9b36f-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="9b36f-162">**Codice server che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="9b36f-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="9b36f-163">**Codice client JavaScript che specifica l'URL (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="9b36f-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="9b36f-164">**Codice client JavaScript che specifica l'URL (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="9b36f-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="9b36f-165">**Codice client .NET che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="9b36f-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="9b36f-166">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="9b36f-166">Configuring SignalR Options</span></span>

<span data-ttu-id="9b36f-167">Gli overload del metodo `MapHubs` consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzato e le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b36f-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="9b36f-168">Abilitare le chiamate tra domini dai client del browser.</span><span class="sxs-lookup"><span data-stu-id="9b36f-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="9b36f-169">In genere, se il browser carica una pagina dal `http://contoso.com`, la connessione SignalR si trova nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="9b36f-170">Se la pagina di `http://contoso.com` crea una connessione a `http://fabrikam.com/signalr`, ovvero una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="9b36f-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="9b36f-171">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9b36f-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="9b36f-172">Per altre informazioni, vedere [Guida dell'API Hub signalr ASP.NET-client JavaScript-come stabilire una connessione tra domini](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="9b36f-173">Abilitare i messaggi di errore dettagliati.</span><span class="sxs-lookup"><span data-stu-id="9b36f-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="9b36f-174">Quando si verificano errori, il comportamento predefinito di SignalR è inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="9b36f-175">L'invio di informazioni dettagliate sugli errori ai client non è consigliato in produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni in attacchi contro l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="9b36f-176">Per la risoluzione dei problemi, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione degli errori più informativa.</span><span class="sxs-lookup"><span data-stu-id="9b36f-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="9b36f-177">Disabilitare i file proxy JavaScript generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="9b36f-178">Per impostazione predefinita, viene generato un file JavaScript con proxy per le classi dell'hub in risposta all'URL "/SignalR/Hubs".</span><span class="sxs-lookup"><span data-stu-id="9b36f-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="9b36f-179">Se non si vuole usare i proxy JavaScript o se si vuole generare questo file manualmente e si fa riferimento a un file fisico nei client, è possibile usare questa opzione per disabilitare la generazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="9b36f-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="9b36f-180">Per altre informazioni, vedere [Guida dell'API degli hub SignalR-client JavaScript-come creare un file fisico per il proxy generato da SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="9b36f-181">Nell'esempio seguente viene illustrato come specificare l'URL della connessione SignalR e queste opzioni in una chiamata al metodo `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="9b36f-182">Per specificare un URL personalizzato, sostituire "/SignalR" nell'esempio con l'URL che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="9b36f-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="9b36f-183">Come creare e usare classi Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-183">How to create and use Hub classes</span></span>

<span data-ttu-id="9b36f-184">Per creare un hub, creare una classe che derivi da [Microsoft. AspNet. SignalR. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b36f-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="9b36f-185">Nell'esempio seguente viene illustrata una classe di hub semplice per un'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="9b36f-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="9b36f-186">In questo esempio, un client connesso può chiamare il metodo `NewContosoChatMessage` e, in questo caso, i dati ricevuti vengono trasmessi a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="9b36f-187">Durata degli oggetti Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-187">Hub object lifetime</span></span>

<span data-ttu-id="9b36f-188">Non è possibile creare un'istanza della classe Hub o chiamare i relativi metodi dal codice nel server. tutto ciò che viene eseguito per l'utente dalla pipeline degli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b36f-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="9b36f-189">SignalR crea una nuova istanza della classe Hub ogni volta che è necessario gestire un'operazione dell'hub, ad esempio quando un client si connette, disconnette o effettua una chiamata al server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="9b36f-190">Poiché le istanze della classe Hub sono temporanee, non è possibile usarle per mantenere lo stato da una chiamata al metodo a quella successiva.</span><span class="sxs-lookup"><span data-stu-id="9b36f-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="9b36f-191">Ogni volta che il server riceve una chiamata al metodo da un client, una nuova istanza della classe Hub elabora il messaggio.</span><span class="sxs-lookup"><span data-stu-id="9b36f-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="9b36f-192">Per mantenere lo stato attraverso più connessioni e chiamate al metodo, usare un altro metodo, ad esempio un database o una variabile statica nella classe Hub, oppure una classe diversa che non derivi da `Hub`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="9b36f-193">Se si mantengono i dati in memoria, usando un metodo come una variabile statica nella classe Hub, i dati andranno perduti quando il dominio dell'applicazione viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="9b36f-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="9b36f-194">Se si vuole inviare messaggi ai client dal codice che viene eseguito all'esterno della classe Hub, non è possibile eseguire questa operazione creando un'istanza della classe Hub, ma è possibile ottenere un riferimento all'oggetto contesto SignalR per la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="9b36f-195">Per ulteriori informazioni, vedere [come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9b36f-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="9b36f-196">Combinazione di maiuscole e minuscole dei nomi di Hub nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b36f-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="9b36f-197">Per impostazione predefinita, i client JavaScript fanno riferimento agli hub usando una versione con la combinazione di maiuscole e minuscole del nome della classe.</span><span class="sxs-lookup"><span data-stu-id="9b36f-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="9b36f-198">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b36f-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="9b36f-199">L'esempio precedente viene definito `contosoChatHub` nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b36f-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="9b36f-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="9b36f-201">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="9b36f-202">Se si vuole specificare un nome diverso per i client da usare, aggiungere l'attributo `HubName`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="9b36f-203">Quando si usa un attributo `HubName`, non viene apportata alcuna modifica al nome Camel nei client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b36f-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="9b36f-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="9b36f-205">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="9b36f-206">Hub multipli</span><span class="sxs-lookup"><span data-stu-id="9b36f-206">Multiple Hubs</span></span>

<span data-ttu-id="9b36f-207">È possibile definire più classi di hub in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="9b36f-208">Quando si esegue questa operazione, la connessione viene condivisa ma i gruppi sono separati:</span><span class="sxs-lookup"><span data-stu-id="9b36f-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="9b36f-209">Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/SignalR" o l'URL personalizzato se ne è stato specificato uno) e tale connessione viene utilizzata per tutti gli hub definiti dal servizio.</span><span class="sxs-lookup"><span data-stu-id="9b36f-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="9b36f-210">Non esiste alcuna differenza di prestazioni per più hub rispetto alla definizione di tutte le funzionalità dell'hub in un'unica classe.</span><span class="sxs-lookup"><span data-stu-id="9b36f-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="9b36f-211">Tutti gli hub ottengono le stesse informazioni sulla richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b36f-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="9b36f-212">Poiché tutti gli hub condividono la stessa connessione, le uniche informazioni della richiesta HTTP ottenute dal server sono quelle contenute nella richiesta HTTP originale che stabilisce la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b36f-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="9b36f-213">Se si usa la richiesta di connessione per passare informazioni dal client al server specificando una stringa di query, non è possibile fornire stringhe di query diverse a hub diversi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="9b36f-214">Tutti gli hub riceveranno le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="9b36f-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="9b36f-215">Il file proxy JavaScript generato conterrà proxy per tutti gli hub in un unico file.</span><span class="sxs-lookup"><span data-stu-id="9b36f-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="9b36f-216">Per informazioni sui proxy JavaScript, vedere [Guida dell'API degli hub SignalR-client JavaScript-il proxy generato e le operazioni che esegue per l'utente](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="9b36f-217">I gruppi sono definiti all'interno degli hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="9b36f-218">In SignalR è possibile definire gruppi denominati da trasmettere a subset di client connessi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="9b36f-219">I gruppi vengono mantenuti separatamente per ogni hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="9b36f-220">Un gruppo denominato "Administrators", ad esempio, include un set di client per la classe `ContosoChatHub` e lo stesso nome di gruppo fa riferimento a un set di client diverso per la classe `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="9b36f-221">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="9b36f-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="9b36f-222">Per esporre un metodo nell'hub che si vuole chiamare dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="9b36f-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="9b36f-223">È possibile specificare un tipo restituito e i parametri, inclusi i tipi e le matrici complessi, come si farebbe C# con qualsiasi metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="9b36f-224">Tutti i dati ricevuti nei parametri o restituiti al chiamante vengono comunicati tra il client e il server tramite JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.</span><span class="sxs-lookup"><span data-stu-id="9b36f-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="9b36f-225">Combinazione di maiuscole e minuscole dei nomi di metodo nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b36f-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="9b36f-226">Per impostazione predefinita, i client JavaScript fanno riferimento ai metodi dell'hub usando una versione con la combinazione di maiuscole e minuscole del nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="9b36f-227">SignalR apporta automaticamente questa modifica in modo che il codice JavaScript possa essere conforme alle convenzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b36f-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9b36f-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="9b36f-229">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="9b36f-230">Se si vuole specificare un nome diverso per i client da usare, aggiungere l'attributo `HubMethodName`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="9b36f-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="9b36f-232">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="9b36f-233">Quando eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="9b36f-233">When to execute asynchronously</span></span>

<span data-ttu-id="9b36f-234">Se il metodo avrà un'esecuzione prolungata o dovrà eseguire operazioni che comportano l'attesa, ad esempio una ricerca nel database o una chiamata al servizio Web, rendere asincrono il metodo dell'hub restituendo un' [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (al posto di `void` Return) o l'oggetto [task&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) (al posto del tipo restituito `T`).</span><span class="sxs-lookup"><span data-stu-id="9b36f-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="9b36f-235">Quando si restituisce un oggetto `Task` dal metodo, SignalR attende il completamento del `Task` e quindi invia il risultato di cui non è stato eseguito il Wrapped al client, quindi non esiste alcuna differenza nella modalità di codifica della chiamata al metodo nel client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="9b36f-236">Rendere asincrono un metodo dell'hub evita di bloccare la connessione quando usa il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9b36f-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="9b36f-237">Quando un metodo dell'hub viene eseguito in modo sincrono e il trasporto è WebSocket, le chiamate successive dei metodi nell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="9b36f-238">Nell'esempio seguente viene illustrato lo stesso metodo codificato per l'esecuzione sincrona o asincrona, seguito dal codice client JavaScript che funziona per chiamare entrambe le versioni.</span><span class="sxs-lookup"><span data-stu-id="9b36f-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="9b36f-239">**Sincrono**</span><span class="sxs-lookup"><span data-stu-id="9b36f-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="9b36f-240">**Asincrono-ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="9b36f-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="9b36f-241">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="9b36f-242">Per altre informazioni su come usare i metodi asincroni in ASP.NET 4,5, vedere [uso di metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="9b36f-243">Definizione degli overload</span><span class="sxs-lookup"><span data-stu-id="9b36f-243">Defining Overloads</span></span>

<span data-ttu-id="9b36f-244">Se si desidera definire overload per un metodo, il numero di parametri in ogni overload deve essere diverso.</span><span class="sxs-lookup"><span data-stu-id="9b36f-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="9b36f-245">Se si distingue un overload semplicemente specificando tipi di parametro diversi, la classe Hub verrà compilata, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tenteranno di chiamare uno degli overload.</span><span class="sxs-lookup"><span data-stu-id="9b36f-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="9b36f-246">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="9b36f-247">Per chiamare i metodi client dal server, usare la proprietà `Clients` in un metodo nella classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="9b36f-248">Nell'esempio seguente viene illustrato il codice server che chiama `addNewMessageToPage` su tutti i client connessi e il codice client che definisce il metodo in un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b36f-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="9b36f-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="9b36f-250">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="9b36f-251">Non è possibile ottenere un valore restituito da un metodo client; la sintassi, ad esempio `int x = Clients.All.add(1,1)`, non funziona.</span><span class="sxs-lookup"><span data-stu-id="9b36f-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="9b36f-252">È possibile specificare tipi e matrici complessi per i parametri.</span><span class="sxs-lookup"><span data-stu-id="9b36f-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="9b36f-253">Nell'esempio seguente un tipo complesso viene passato al client in un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="9b36f-254">**Codice server che chiama un metodo client usando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="9b36f-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="9b36f-255">**Codice server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="9b36f-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="9b36f-256">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="9b36f-257">Selezione dei client che riceveranno la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="9b36f-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="9b36f-258">La proprietà clients restituisce un oggetto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) che fornisce diverse opzioni per specificare quali client riceveranno la chiamata RPC:</span><span class="sxs-lookup"><span data-stu-id="9b36f-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="9b36f-259">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="9b36f-260">Solo il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="9b36f-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="9b36f-261">Tutti i client tranne il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="9b36f-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="9b36f-262">Un client specifico identificato dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="9b36f-263">Questo esempio chiama `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'uso di `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="9b36f-264">Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="9b36f-265">Tutti i client connessi in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="9b36f-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="9b36f-266">Tutti i client connessi in un gruppo specificato, ad eccezione dei client specificati, identificati dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="9b36f-267">Tutti i client connessi in un gruppo specificato eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="9b36f-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="9b36f-268">Nessuna convalida in fase di compilazione per i nomi dei metodi</span><span class="sxs-lookup"><span data-stu-id="9b36f-268">No compile-time validation for method names</span></span>

<span data-ttu-id="9b36f-269">Il nome del metodo specificato viene interpretato come oggetto dinamico, il che significa che non è presente alcuna convalida di IntelliSense o della fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="9b36f-270">L'espressione viene valutata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="9b36f-271">Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori del parametro al client e, se il client dispone di un metodo che corrisponde al nome, viene chiamato il metodo e i valori del parametro vengono passati.</span><span class="sxs-lookup"><span data-stu-id="9b36f-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="9b36f-272">Se non viene trovato alcun metodo corrispondente nel client, non viene generato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="9b36f-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="9b36f-273">Per informazioni sul formato dei dati trasmessi dal SignalR al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="9b36f-274">Corrispondenza nome metodo senza distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="9b36f-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="9b36f-275">La corrispondenza del nome del metodo non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="9b36f-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="9b36f-276">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`o `addContosoChatMessageToPage` nel client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="9b36f-277">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="9b36f-277">Asynchronous execution</span></span>

<span data-ttu-id="9b36f-278">Il metodo chiamato viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="9b36f-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="9b36f-279">Il codice che segue una chiamata al metodo a un client verrà eseguito immediatamente senza attendere che SignalR completi la trasmissione dei dati ai client a meno che non si specifichi che le righe di codice successive attendono il completamento del metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="9b36f-280">Gli esempi di codice seguenti illustrano come eseguire in sequenza due metodi client, uno usando il codice che funziona in .NET 4,5 e uno che usa codice che funziona in .NET 4.</span><span class="sxs-lookup"><span data-stu-id="9b36f-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="9b36f-281">**Esempio .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="9b36f-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="9b36f-282">**Esempio .NET 4**</span><span class="sxs-lookup"><span data-stu-id="9b36f-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="9b36f-283">Se si usa `await` o `ContinueWith` per attendere il completamento di un metodo client prima che venga eseguita la riga di codice successiva, questo non significa che i client riceveranno effettivamente il messaggio prima che venga eseguita la riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="9b36f-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="9b36f-284">Il termine "completamento" di una chiamata al metodo client significa solo che SignalR ha eseguito tutte le operazioni necessarie per l'invio del messaggio.</span><span class="sxs-lookup"><span data-stu-id="9b36f-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="9b36f-285">Se è necessario verificare che i client abbiano ricevuto il messaggio, è necessario programmare autonomamente il meccanismo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="9b36f-286">Ad esempio, è possibile codificare un metodo di `MessageReceived` nell'hub e nel metodo `addContosoChatMessageToPage` sul client è possibile chiamare `MessageReceived` dopo aver eseguito tutte le operazioni necessarie nel client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="9b36f-287">In `MessageReceived` nell'hub è possibile eseguire qualsiasi operazione a seconda della ricezione e dell'elaborazione effettive del client della chiamata al metodo originale.</span><span class="sxs-lookup"><span data-stu-id="9b36f-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="9b36f-288">Come usare una variabile stringa come nome del metodo</span><span class="sxs-lookup"><span data-stu-id="9b36f-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="9b36f-289">Se si vuole richiamare un metodo client usando una variabile stringa come nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) per `IClientProxy` e quindi chiamare [Invoke (methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b36f-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="9b36f-290">Come gestire l'appartenenza a un gruppo dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="9b36f-291">I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a subset specificati di client connessi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="9b36f-292">Un gruppo può avere un numero qualsiasi di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="9b36f-293">Per gestire l'appartenenza a un gruppo, usare i metodi di [aggiunta](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimozione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) forniti dalla proprietà `Groups` della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="9b36f-294">Nell'esempio seguente vengono illustrati i metodi `Groups.Add` e `Groups.Remove` usati nei metodi dell'hub chiamati dal codice client, seguiti dal codice client JavaScript che li chiama.</span><span class="sxs-lookup"><span data-stu-id="9b36f-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="9b36f-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="9b36f-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="9b36f-296">**Client JavaScript con proxy generato**</span><span class="sxs-lookup"><span data-stu-id="9b36f-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="9b36f-297">Non è necessario creare gruppi in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="9b36f-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="9b36f-298">In effetti, un gruppo viene creato automaticamente la prima volta che si specifica il nome in una chiamata a `Groups.Add`e viene eliminato quando si rimuove l'ultima connessione dall'appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="9b36f-299">Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="9b36f-300">SignalR invia messaggi a client e gruppi basati su un [modello di pubblicazione/sottoscrizione](http://en.wikipedia.org/wiki/Publish/subscribe)e il server non gestisce gli elenchi di gruppi o appartenenze a gruppi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="9b36f-301">Ciò consente di massimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a un Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="9b36f-302">Esecuzione asincrona di metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="9b36f-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="9b36f-303">I metodi `Groups.Add` e `Groups.Remove` vengono eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="9b36f-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="9b36f-304">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo `Groups.Add` venga completato per primo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="9b36f-305">Gli esempi di codice seguenti illustrano come eseguire questa operazione, una usando il codice che funziona in .NET 4,5 e uno usando il codice che funziona in .NET 4</span><span class="sxs-lookup"><span data-stu-id="9b36f-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="9b36f-306">**Esempio .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="9b36f-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="9b36f-307">**Esempio .NET 4**</span><span class="sxs-lookup"><span data-stu-id="9b36f-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="9b36f-308">Persistenza appartenenza a gruppi</span><span class="sxs-lookup"><span data-stu-id="9b36f-308">Group membership persistence</span></span>

<span data-ttu-id="9b36f-309">SignalR tiene traccia delle connessioni, non degli utenti, pertanto se si desidera che un utente si trovi nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="9b36f-310">Dopo una perdita temporanea della connettività, a volte SignalR è in grado di ripristinare automaticamente la connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="9b36f-311">In tal caso, SignalR sta ripristinando la stessa connessione, non stabilendo una nuova connessione, quindi l'appartenenza al gruppo del client viene ripristinata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="9b36f-312">Questo è possibile anche quando l'interruzione temporanea è il risultato di un riavvio o di un errore del server, perché lo stato di connessione per ogni client, incluse le appartenenze ai gruppi, viene sottoposto a round trip al client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="9b36f-313">Se un server diventa inattivo e viene sostituito da un nuovo server prima del timeout della connessione, un client può riconnettersi automaticamente al nuovo server ed eseguire di nuovo la registrazione nei gruppi di cui è membro.</span><span class="sxs-lookup"><span data-stu-id="9b36f-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="9b36f-314">Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività o quando si verifica il timeout della connessione oppure quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), le appartenenze ai gruppi vengono perse.</span><span class="sxs-lookup"><span data-stu-id="9b36f-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="9b36f-315">La volta successiva che l'utente si connette sarà una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="9b36f-316">Per gestire le appartenenze ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve tenere traccia delle associazioni tra utenti e gruppi e ripristinare le appartenenze ai gruppi ogni volta che un utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="9b36f-317">Per ulteriori informazioni sulle connessioni e le riconnessioni, vedere [come gestire gli eventi di durata della connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9b36f-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="9b36f-318">Gruppi di utenti singoli</span><span class="sxs-lookup"><span data-stu-id="9b36f-318">Single-user groups</span></span>

<span data-ttu-id="9b36f-319">Le applicazioni che usano SignalR devono in genere tenere traccia delle associazioni tra utenti e connessioni per poter stabilire quale utente ha inviato un messaggio e quali utenti devono ricevere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="9b36f-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="9b36f-320">I gruppi vengono utilizzati in uno dei due modelli di uso comune.</span><span class="sxs-lookup"><span data-stu-id="9b36f-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="9b36f-321">Gruppi di utenti singoli.</span><span class="sxs-lookup"><span data-stu-id="9b36f-321">Single-user groups.</span></span>

    <span data-ttu-id="9b36f-322">È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette.</span><span class="sxs-lookup"><span data-stu-id="9b36f-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="9b36f-323">Per inviare messaggi all'utente inviato al gruppo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="9b36f-324">Uno svantaggio di questo metodo è che il gruppo non fornisce un modo per determinare se l'utente è online o offline.</span><span class="sxs-lookup"><span data-stu-id="9b36f-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="9b36f-325">Tenere traccia delle associazioni tra i nomi utente e gli ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="9b36f-326">È possibile archiviare un'associazione tra ogni nome utente e uno o più ID di connessione in un dizionario o un database e aggiornare i dati archiviati ogni volta che l'utente si connette o si disconnette.</span><span class="sxs-lookup"><span data-stu-id="9b36f-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="9b36f-327">Per inviare messaggi all'utente, è necessario specificare gli ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="9b36f-328">Uno svantaggio di questo metodo è che richiede una maggiore quantità di memoria.</span><span class="sxs-lookup"><span data-stu-id="9b36f-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="9b36f-329">Come gestire gli eventi di durata della connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="9b36f-330">Le cause tipiche della gestione degli eventi di durata della connessione sono tenere traccia del fatto che un utente sia connesso o meno e per tenere traccia dell'associazione tra i nomi utente e gli ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="9b36f-331">Per eseguire il proprio codice quando i client si connettono o si disconnettono, eseguire l'override dei metodi virtuali `OnConnected`, `OnDisconnected`e `OnReconnected` della classe Hub, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="9b36f-332">Quando OnConnected, Disconnected e OnReconnected vengono chiamati</span><span class="sxs-lookup"><span data-stu-id="9b36f-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="9b36f-333">Ogni volta che un browser passa a una nuova pagina, è necessario stabilire una nuova connessione, ovvero SignalR eseguirà il metodo `OnDisconnected` seguito dal metodo `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="9b36f-334">SignalR crea sempre un nuovo ID connessione quando viene stabilita una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="9b36f-335">Il metodo `OnReconnected` viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR può recuperare automaticamente, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il metodo `OnDisconnected` viene chiamato quando il client è disconnesso e SignalR non può riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="9b36f-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="9b36f-336">Pertanto, una possibile sequenza di eventi per un determinato client è `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected``OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="9b36f-337">Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected``OnReconnected` per una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="9b36f-338">Il metodo `OnDisconnected` non viene chiamato in alcuni scenari, ad esempio quando un server diventa inattivo o viene riciclato il dominio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="9b36f-339">Quando un altro server viene attivato o viene completato il riciclo del dominio dell'applicazione, è possibile che alcuni client siano in grado di riconnettersi e generare l'evento `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="9b36f-340">Per altre informazioni, vedere informazioni [e gestione degli eventi di durata della connessione in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="9b36f-341">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="9b36f-341">Caller state not populated</span></span>

<span data-ttu-id="9b36f-342">I metodi del gestore eventi per la durata della connessione vengono chiamati dal server, il che significa che qualsiasi stato inserito nell'oggetto `state` nel client non verrà popolato nella proprietà `Caller` sul server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="9b36f-343">Per informazioni sull'oggetto `state` e sulla proprietà `Caller`, vedere [come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9b36f-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="9b36f-344">Come ottenere informazioni sul client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="9b36f-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="9b36f-345">Per ottenere informazioni sul client, usare la proprietà `Context` della classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="9b36f-346">La proprietà `Context` restituisce un oggetto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) che consente di accedere alle informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b36f-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="9b36f-347">ID connessione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="9b36f-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="9b36f-348">L'ID connessione è un GUID assegnato da SignalR (non è possibile specificare il valore nel codice personalizzato).</span><span class="sxs-lookup"><span data-stu-id="9b36f-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="9b36f-349">È presente un ID di connessione per ogni connessione e lo stesso ID di connessione viene usato da tutti gli hub se nell'applicazione sono presenti più hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="9b36f-350">Dati dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b36f-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="9b36f-351">È anche possibile ottenere intestazioni HTTP dal `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="9b36f-352">Il motivo di più riferimenti alla stessa cosa è che `Context.Headers` è stato creato per primo, la proprietà `Context.Request` è stata aggiunta successivamente e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="9b36f-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="9b36f-353">Dati della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b36f-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="9b36f-354">È anche possibile ottenere dati della stringa di query da `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="9b36f-355">La stringa di query che si ottiene in questa proprietà corrisponde a quella usata con la richiesta HTTP che ha stabilito la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b36f-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="9b36f-356">È possibile aggiungere parametri della stringa di query nel client configurando la connessione, un modo pratico per passare i dati sul client dal client al server.</span><span class="sxs-lookup"><span data-stu-id="9b36f-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="9b36f-357">Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript quando si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="9b36f-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="9b36f-358">Per ulteriori informazioni sull'impostazione dei parametri della stringa di query, vedere le guide alle API per i client [JavaScript](index.md) e [.NET](index.md) .</span><span class="sxs-lookup"><span data-stu-id="9b36f-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="9b36f-359">È possibile trovare il metodo di trasporto usato per la connessione nei dati della stringa di query, insieme ad altri valori usati internamente da SignalR:</span><span class="sxs-lookup"><span data-stu-id="9b36f-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="9b36f-360">Il valore di `transportMethod` sarà "WebSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="9b36f-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="9b36f-361">Si noti che se si seleziona questo valore nel metodo del gestore dell'evento `OnConnected`, in alcuni scenari potrebbe essere inizialmente presente un valore di trasporto che non corrisponde al metodo di trasporto negoziato finale per la connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="9b36f-362">In tal caso, il metodo genererà un'eccezione e verrà chiamato di nuovo in un secondo momento quando viene stabilito il metodo di trasporto finale.</span><span class="sxs-lookup"><span data-stu-id="9b36f-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="9b36f-363">Cookie.</span><span class="sxs-lookup"><span data-stu-id="9b36f-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="9b36f-364">È anche possibile ottenere i cookie dal `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="9b36f-365">informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="9b36f-366">Oggetto HttpContext per la richiesta:</span><span class="sxs-lookup"><span data-stu-id="9b36f-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="9b36f-367">Utilizzare questo metodo anziché ottenere `HttpContext.Current` per ottenere l'oggetto `HttpContext` per la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b36f-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="9b36f-368">Come passare lo stato tra i client e la classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="9b36f-369">Il proxy client fornisce un oggetto `state` in cui è possibile archiviare i dati che si desidera trasmettere al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="9b36f-370">Sul server è possibile accedere a questi dati nella proprietà `Clients.Caller` nei metodi dell'hub chiamati dai client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="9b36f-371">La proprietà `Clients.Caller` non viene popolata per i metodi del gestore eventi della durata della connessione `OnConnected`, `OnDisconnected`e `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="9b36f-372">La creazione o l'aggiornamento dei dati nell'oggetto `state` e nella proprietà `Clients.Caller` funziona in entrambe le direzioni.</span><span class="sxs-lookup"><span data-stu-id="9b36f-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="9b36f-373">È possibile aggiornare i valori nel server e passarli di nuovo al client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="9b36f-374">Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="9b36f-375">Nell'esempio seguente viene illustrato il codice equivalente in un client .NET.</span><span class="sxs-lookup"><span data-stu-id="9b36f-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="9b36f-376">Nella classe Hub è possibile accedere a questi dati nella proprietà `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="9b36f-377">Nell'esempio seguente viene illustrato il codice che recupera lo stato a cui si fa riferimento nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="9b36f-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="9b36f-378">Questo meccanismo per rendere permanente lo stato non è destinato a grandi quantità di dati, perché tutto ciò che si inserisce nella `state` o `Clients.Caller` proprietà viene sottoposto a round trip con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="9b36f-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="9b36f-379">È utile per elementi più piccoli, ad esempio nomi utente o contatori.</span><span class="sxs-lookup"><span data-stu-id="9b36f-379">It's useful for smaller items such as user names or counters.</span></span>

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="9b36f-380">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="9b36f-381">Per gestire gli errori che si verificano nei metodi della classe Hub, usare uno o entrambi i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b36f-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="9b36f-382">Eseguire il wrapping del codice del metodo nei blocchi try-catch e registrare l'oggetto Exception.</span><span class="sxs-lookup"><span data-stu-id="9b36f-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="9b36f-383">A scopo di debug è possibile inviare l'eccezione al client, ma per motivi di sicurezza l'invio di informazioni dettagliate ai client nell'ambiente di produzione non è consigliato.</span><span class="sxs-lookup"><span data-stu-id="9b36f-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="9b36f-384">Creare un modulo della pipeline Hub che gestisce il metodo [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="9b36f-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="9b36f-385">Nell'esempio seguente viene illustrato un modulo pipeline che registra gli errori, seguito dal codice in Global. asax che inserisce il modulo nella pipeline degli hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="9b36f-386">Per ulteriori informazioni sui moduli della pipeline dell'hub, vedere [How to customize the hub pipeline](#hubpipeline) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9b36f-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="9b36f-387">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="9b36f-387">How to enable tracing</span></span>

<span data-ttu-id="9b36f-388">Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics al file Web. config, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b36f-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="9b36f-389">Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nella finestra **output** .</span><span class="sxs-lookup"><span data-stu-id="9b36f-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="9b36f-390">Come chiamare i metodi client e gestire i gruppi dall'esterno della classe Hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="9b36f-391">Per chiamare i metodi client da una classe diversa da quella della classe Hub, ottenere un riferimento all'oggetto contesto SignalR per l'hub e usarlo per chiamare i metodi sul client o gestire i gruppi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="9b36f-392">L'esempio seguente `StockTicker` classe ottiene l'oggetto context, lo archivia in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e usa il contesto dell'istanza della classe singleton per chiamare il metodo `updateStockPrice` sui client connessi a un Hub denominato `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="9b36f-393">Se è necessario usare il contesto più volte in un oggetto di lunga durata, ottenere il riferimento una volta e salvarlo invece di ottenerlo di nuovo ogni volta.</span><span class="sxs-lookup"><span data-stu-id="9b36f-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="9b36f-394">Ottenere il contesto una volta garantisce che SignalR invii messaggi ai client nella stessa sequenza in cui i metodi dell'hub effettuano chiamate al metodo client.</span><span class="sxs-lookup"><span data-stu-id="9b36f-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="9b36f-395">Per un'esercitazione che illustra come usare il contesto di SignalR per un hub, vedere [server broadcast with ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="9b36f-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="9b36f-396">Chiamata di metodi client</span><span class="sxs-lookup"><span data-stu-id="9b36f-396">Calling client methods</span></span>

<span data-ttu-id="9b36f-397">È possibile specificare quali client riceveranno la RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="9b36f-398">Il motivo è che il contesto non è associato a una particolare chiamata da un client, pertanto tutti i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad esempio `Clients.Others`, o `Clients.Caller`o `Clients.OthersInGroup`, non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="9b36f-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="9b36f-399">Sono disponibili le seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="9b36f-399">The following options are available:</span></span>

- <span data-ttu-id="9b36f-400">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="9b36f-401">Un client specifico identificato dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="9b36f-402">Tutti i client connessi ad eccezione dei client specificati, identificati dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="9b36f-403">Tutti i client connessi in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="9b36f-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="9b36f-404">Tutti i client connessi in un gruppo specificato, ad eccezione dei client specificati, identificati dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="9b36f-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="9b36f-405">Se si esegue una chiamata nella classe non hub dai metodi della classe Hub, è possibile passare l'ID di connessione corrente e usarlo con `Clients.Client`, `Clients.AllExcept`o `Clients.Group` per simulare `Clients.Caller`, `Clients.Others`o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="9b36f-406">Nell'esempio seguente, la classe `MoveShapeHub` passa l'ID connessione alla classe `Broadcaster` in modo che la classe `Broadcaster` possa simulare `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="9b36f-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="9b36f-407">Gestione dell'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="9b36f-407">Managing group membership</span></span>

<span data-ttu-id="9b36f-408">Per la gestione dei gruppi sono disponibili le stesse opzioni di una classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="9b36f-409">Aggiungere un client a un gruppo</span><span class="sxs-lookup"><span data-stu-id="9b36f-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="9b36f-410">Rimuovere un client da un gruppo</span><span class="sxs-lookup"><span data-stu-id="9b36f-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="9b36f-411">Come personalizzare la pipeline degli hub</span><span class="sxs-lookup"><span data-stu-id="9b36f-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="9b36f-412">SignalR consente di inserire codice personalizzato nella pipeline dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9b36f-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="9b36f-413">Nell'esempio seguente viene illustrato un modulo della pipeline di hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuta dal client e la chiamata al metodo in uscita richiamata sul client:</span><span class="sxs-lookup"><span data-stu-id="9b36f-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="9b36f-414">Il codice seguente nel file *Global. asax* registra il modulo per l'esecuzione nella pipeline dell'hub:</span><span class="sxs-lookup"><span data-stu-id="9b36f-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="9b36f-415">È possibile eseguire l'override di molti metodi diversi.</span><span class="sxs-lookup"><span data-stu-id="9b36f-415">There are many different methods that you can override.</span></span> <span data-ttu-id="9b36f-416">Per un elenco completo, vedere [Metodi HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9b36f-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
