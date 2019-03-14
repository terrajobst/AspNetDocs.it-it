---
title: Componenti di Razor modelli di hosting
author: guardrex
description: Comprendere Blazor lato client e lato server ASP.NET Core Razor componenti modelli di hosting.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042438"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="ecbac-103">Componenti di Razor modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="ecbac-103">Razor Components hosting models</span></span>

<span data-ttu-id="ecbac-104">Da [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ecbac-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ecbac-105">I componenti di Razor è un framework web progettato per l'esecuzione del client nel browser in un runtime di .NET basate su WebAssembly (*Blazor*) o sul lato server in ASP.NET Core (*ASP.NET Core Razor componenti*).</span><span class="sxs-lookup"><span data-stu-id="ecbac-105">Razor Components is a web framework designed to run client-side in the browser on a WebAssembly-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="ecbac-106">Indipendentemente dal fatto i modelli di modello, l'app e il componente di hosting *rimangono invariate*.</span><span class="sxs-lookup"><span data-stu-id="ecbac-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="ecbac-107">Questo articolo illustra i modelli di hosting disponibili.</span><span class="sxs-lookup"><span data-stu-id="ecbac-107">This article discusses the available hosting models.</span></span>

## <a name="client-side-hosting-model"></a><span data-ttu-id="ecbac-108">Modello di hosting sul lato client</span><span class="sxs-lookup"><span data-stu-id="ecbac-108">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="ecbac-109">Modello di hosting dell'entità per Blazor è sul lato client in esecuzione nel browser.</span><span class="sxs-lookup"><span data-stu-id="ecbac-109">The principal hosting model for Blazor is running client-side in the browser.</span></span> <span data-ttu-id="ecbac-110">In questo modello, l'app Blazor, le relative dipendenze e il runtime di .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="ecbac-110">In this model, the Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="ecbac-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="ecbac-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="ecbac-112">Tutti gli aggiornamenti dell'interfaccia utente e la gestione degli eventi avviene entro lo stesso processo.</span><span class="sxs-lookup"><span data-stu-id="ecbac-112">All UI updates and event handling happens within the same process.</span></span> <span data-ttu-id="ecbac-113">Le risorse di app possono essere distribuite come file statici usando qualsiasi server web è preferito (vedere [Host e distribuire](xref:host-and-deploy/razor-components/index)).</span><span class="sxs-lookup"><span data-stu-id="ecbac-113">The app assets can be deployed as static files using whatever web server is preferred (see [Host and deploy](xref:host-and-deploy/razor-components/index)).</span></span>

![Blazor lato client: L'app Blazor viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

<span data-ttu-id="ecbac-115">Per creare un'app Blazor usando il modello di hosting lato client, usare il **Blazor** oppure **Blazor (ASP.NET Core ospitate)** modelli di progetto (`blazor` o `blazorhosted` modello quando si usa il [dotnet nuovo](/dotnet/core/tools/dotnet-new) seguente al prompt dei comandi).</span><span class="sxs-lookup"><span data-stu-id="ecbac-115">To create a Blazor app using the client-side hosting model, use the **Blazor** or **Blazor (ASP.NET Core Hosted)** project templates (`blazor` or `blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command at a command prompt).</span></span> <span data-ttu-id="ecbac-116">L'oggetto incluso *blazor.webassembly.js* gli handle di script:</span><span class="sxs-lookup"><span data-stu-id="ecbac-116">The included *blazor.webassembly.js* script handles:</span></span>

* <span data-ttu-id="ecbac-117">Scaricare il runtime di .NET, l'app e le relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="ecbac-117">Downloading the .NET runtime, the app, and its dependencies.</span></span>
* <span data-ttu-id="ecbac-118">Inizializzazione del runtime per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="ecbac-118">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="ecbac-119">Il modello di hosting lato client offre diversi vantaggi.</span><span class="sxs-lookup"><span data-stu-id="ecbac-119">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="ecbac-120">Blazor lato client:</span><span class="sxs-lookup"><span data-stu-id="ecbac-120">Client-side Blazor:</span></span>

* <span data-ttu-id="ecbac-121">Non dispone di alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="ecbac-121">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="ecbac-122">Ha un'interfaccia utente interattiva completa.</span><span class="sxs-lookup"><span data-stu-id="ecbac-122">Has a rich interactive UI.</span></span>
* <span data-ttu-id="ecbac-123">Utilizza le funzionalità e le risorse del client.</span><span class="sxs-lookup"><span data-stu-id="ecbac-123">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="ecbac-124">Gli Offload di lavoro dal server al client.</span><span class="sxs-lookup"><span data-stu-id="ecbac-124">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="ecbac-125">Supporta scenari non in linea.</span><span class="sxs-lookup"><span data-stu-id="ecbac-125">Supports offline scenarios.</span></span>

<span data-ttu-id="ecbac-126">Degli svantaggi all'hosting lato client.</span><span class="sxs-lookup"><span data-stu-id="ecbac-126">There are downsides to client-side hosting.</span></span> <span data-ttu-id="ecbac-127">Blazor lato client:</span><span class="sxs-lookup"><span data-stu-id="ecbac-127">Client-side Blazor:</span></span>

* <span data-ttu-id="ecbac-128">Limita l'app per le funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="ecbac-128">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="ecbac-129">Richiede un client in grado di supportare hardware e software (ad esempio, WebAssembly supporto).</span><span class="sxs-lookup"><span data-stu-id="ecbac-129">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="ecbac-130">Ha una maggiore dimensione del download e l'app più il tempo di caricamento.</span><span class="sxs-lookup"><span data-stu-id="ecbac-130">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="ecbac-131">È minore maturare .NET runtime e strumenti di supporto (ad esempio, le limitazioni di supporto di .NET Standard e debug).</span><span class="sxs-lookup"><span data-stu-id="ecbac-131">Has less mature .NET runtime and tooling support (for example, limitations in .NET Standard support and debugging).</span></span>

<span data-ttu-id="ecbac-132">Visual Studio include il **Blazor (ASP.NET Core ospitate)** modello di progetto per la creazione di un'app Blazor che viene eseguito su WebAssembly ed è ospitata in un server di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecbac-132">Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template for creating a Blazor app that runs on WebAssembly and is hosted on an ASP.NET Core server.</span></span> <span data-ttu-id="ecbac-133">L'app ASP.NET Core viene usato l'app Blazor ai client, ma in caso contrario, è un processo separato.</span><span class="sxs-lookup"><span data-stu-id="ecbac-133">The ASP.NET Core app serves the Blazor app to clients but is otherwise a separate process.</span></span> <span data-ttu-id="ecbac-134">L'app Blazor lato client può interagire con il server sulla rete mediante chiamate all'API Web o le connessioni SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecbac-134">The client-side Blazor app can interact with the server over the network using Web API calls or SignalR connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ecbac-135">Se un'app Blazor lato client è servita da un'app ASP.NET Core ospitata come app secondarie di IIS, disabilitare il gestore ereditato modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecbac-135">If a client-side Blazor app is served by an ASP.NET Core app hosted as an IIS sub-app, disable the inherited ASP.NET Core Module handler.</span></span> <span data-ttu-id="ecbac-136">Impostare il percorso base dell'app dell'App Blazor *index. HTML* file per l'alias IIS utilizzato durante la configurazione della sotto-app in IIS.</span><span class="sxs-lookup"><span data-stu-id="ecbac-136">Set the app base path in the Blazor app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>
>
> <span data-ttu-id="ecbac-137">Per altre informazioni, vedere [percorso base dell'App](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="ecbac-137">For more information, see [App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="ecbac-138">Modello di hosting sul lato server</span><span class="sxs-lookup"><span data-stu-id="ecbac-138">Server-side hosting model</span></span>

<span data-ttu-id="ecbac-139">Nel modello di hosting ASP.NET Core Razor componenti lato server, l'app viene eseguita nel server da all'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecbac-139">In the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="ecbac-140">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecbac-140">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

![ASP.NET Core Razor componenti lato server: Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="ecbac-142">Per creare un'app Razor componenti usando il modello di hosting sul lato server, usare il **Blazor (Server-side in ASP.NET Core)** modello (`blazorserver` quando si usa [dotnet nuovo](/dotnet/core/tools/dotnet-new) un prompt dei comandi).</span><span class="sxs-lookup"><span data-stu-id="ecbac-142">To create a Razor Components app using the server-side hosting model, use the **Blazor (Server-side in ASP.NET Core)** template (`blazorserver` when using [dotnet new](/dotnet/core/tools/dotnet-new) at a command prompt).</span></span> <span data-ttu-id="ecbac-143">Un'app ASP.NET Core ospita l'app sul lato server per i componenti di Razor e configura l'endpoint di SignalR in cui i client si connettono.</span><span class="sxs-lookup"><span data-stu-id="ecbac-143">An ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="ecbac-144">L'app ASP.NET Core fa riferimento l'app `Startup` classe da aggiungere:</span><span class="sxs-lookup"><span data-stu-id="ecbac-144">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="ecbac-145">Servizi di Razor componenti lato server.</span><span class="sxs-lookup"><span data-stu-id="ecbac-145">Server-side Razor Components services.</span></span>
* <span data-ttu-id="ecbac-146">L'app per la gestione della pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ecbac-146">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="ecbac-147">Il *blazor.server.js* script&dagger; stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="ecbac-147">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="ecbac-148">È responsabilità dell'app per mantenere e ripristinare lo stato di app in base alle esigenze (ad esempio, in caso di una connessione di rete persa).</span><span class="sxs-lookup"><span data-stu-id="ecbac-148">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="ecbac-149">Il modello di hosting di server-side offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="ecbac-149">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="ecbac-150">Consente di scrivere l'intera app con .NET e C# usando il modello di componente.</span><span class="sxs-lookup"><span data-stu-id="ecbac-150">Allows you to write your entire app with .NET and C# using the component model.</span></span>
* <span data-ttu-id="ecbac-151">Fornisce un'idea interattiva avanzata, evitando inutili pagina viene aggiornata.</span><span class="sxs-lookup"><span data-stu-id="ecbac-151">Provides a rich interactive feel and avoids unnecessary page refreshes.</span></span>
* <span data-ttu-id="ecbac-152">Ha una dimensione di app significativamente inferiore rispetto a un'app Blazor lato client e vengono caricati molto più rapidamente.</span><span class="sxs-lookup"><span data-stu-id="ecbac-152">Has a significantly smaller app size than a client-side Blazor app and loads much faster.</span></span>
* <span data-ttu-id="ecbac-153">Logica dei componenti può usufruire di funzionalità server, incluso l'uso di qualsiasi API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ecbac-153">Component logic can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="ecbac-154">Viene eseguito in .NET Core nel server, quindi .NET esistenti degli strumenti, ad esempio il debug, funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="ecbac-154">Runs on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="ecbac-155">Funziona con i thin client (ad esempio, i browser che non supportano WebAssembly e risorse vincolate dispositivi).</span><span class="sxs-lookup"><span data-stu-id="ecbac-155">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>

<span data-ttu-id="ecbac-156">Esistono svantaggi lato server di hosting:</span><span class="sxs-lookup"><span data-stu-id="ecbac-156">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="ecbac-157">Presenta una latenza più elevata: Ogni interazione dell'utente prevede un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="ecbac-157">Has higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="ecbac-158">Non offre alcun supporto offline: Se la connessione del client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="ecbac-158">Offers no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="ecbac-159">Ha ridotto la scalabilità: Il server deve gestire più connessioni di client e la gestione dello stato del client.</span><span class="sxs-lookup"><span data-stu-id="ecbac-159">Has reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="ecbac-160">Richiede un server di ASP.NET Core per rendere disponibile l'app.</span><span class="sxs-lookup"><span data-stu-id="ecbac-160">Requires an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="ecbac-161">Distribuzione senza un server (ad esempio, da una rete CDN) non è possibile.</span><span class="sxs-lookup"><span data-stu-id="ecbac-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="ecbac-162">&dagger;Il *blazor.server.js* lo script viene pubblicato nel seguente percorso: *bin / {Debug | Versione} / {FRAMEWORK di destinazione} /publish/ {nome applicazione}. Dist/App/Framework*.</span><span class="sxs-lookup"><span data-stu-id="ecbac-162">&dagger;The *blazor.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
