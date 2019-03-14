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
# <a name="razor-components-hosting-models"></a>Componenti di Razor modelli di hosting

Da [Daniel Roth](https://github.com/danroth27)

I componenti di Razor è un framework web progettato per l'esecuzione del client nel browser in un runtime di .NET basate su WebAssembly (*Blazor*) o sul lato server in ASP.NET Core (*ASP.NET Core Razor componenti*). Indipendentemente dal fatto i modelli di modello, l'app e il componente di hosting *rimangono invariate*. Questo articolo illustra i modelli di hosting disponibili.

## <a name="client-side-hosting-model"></a>Modello di hosting sul lato client

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Modello di hosting dell'entità per Blazor è sul lato client in esecuzione nel browser. In questo modello, l'app Blazor, le relative dipendenze e il runtime di .NET vengono scaricati nel browser. L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. Tutti gli aggiornamenti dell'interfaccia utente e la gestione degli eventi avviene entro lo stesso processo. Le risorse di app possono essere distribuite come file statici usando qualsiasi server web è preferito (vedere [Host e distribuire](xref:host-and-deploy/razor-components/index)).

![Blazor lato client: L'app Blazor viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

Per creare un'app Blazor usando il modello di hosting lato client, usare il **Blazor** oppure **Blazor (ASP.NET Core ospitate)** modelli di progetto (`blazor` o `blazorhosted` modello quando si usa il [dotnet nuovo](/dotnet/core/tools/dotnet-new) seguente al prompt dei comandi). L'oggetto incluso *blazor.webassembly.js* gli handle di script:

* Scaricare il runtime di .NET, l'app e le relative dipendenze.
* Inizializzazione del runtime per eseguire l'app.

Il modello di hosting lato client offre diversi vantaggi. Blazor lato client:

* Non dispone di alcuna dipendenza lato server .NET.
* Ha un'interfaccia utente interattiva completa.
* Utilizza le funzionalità e le risorse del client.
* Gli Offload di lavoro dal server al client.
* Supporta scenari non in linea.

Degli svantaggi all'hosting lato client. Blazor lato client:

* Limita l'app per le funzionalità del browser.
* Richiede un client in grado di supportare hardware e software (ad esempio, WebAssembly supporto).
* Ha una maggiore dimensione del download e l'app più il tempo di caricamento.
* È minore maturare .NET runtime e strumenti di supporto (ad esempio, le limitazioni di supporto di .NET Standard e debug).

Visual Studio include il **Blazor (ASP.NET Core ospitate)** modello di progetto per la creazione di un'app Blazor che viene eseguito su WebAssembly ed è ospitata in un server di ASP.NET Core. L'app ASP.NET Core viene usato l'app Blazor ai client, ma in caso contrario, è un processo separato. L'app Blazor lato client può interagire con il server sulla rete mediante chiamate all'API Web o le connessioni SignalR.

> [!IMPORTANT]
> Se un'app Blazor lato client è servita da un'app ASP.NET Core ospitata come app secondarie di IIS, disabilitare il gestore ereditato modulo ASP.NET Core. Impostare il percorso base dell'app dell'App Blazor *index. HTML* file per l'alias IIS utilizzato durante la configurazione della sotto-app in IIS.
>
> Per altre informazioni, vedere [percorso base dell'App](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Modello di hosting sul lato server

Nel modello di hosting ASP.NET Core Razor componenti lato server, l'app viene eseguita nel server da all'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione SignalR.

![ASP.NET Core Razor componenti lato server: Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

Per creare un'app Razor componenti usando il modello di hosting sul lato server, usare il **Blazor (Server-side in ASP.NET Core)** modello (`blazorserver` quando si usa [dotnet nuovo](/dotnet/core/tools/dotnet-new) un prompt dei comandi). Un'app ASP.NET Core ospita l'app sul lato server per i componenti di Razor e configura l'endpoint di SignalR in cui i client si connettono. L'app ASP.NET Core fa riferimento l'app `Startup` classe da aggiungere:

* Servizi di Razor componenti lato server.
* L'app per la gestione della pipeline delle richieste.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

Il *blazor.server.js* script&dagger; stabilisce la connessione client. È responsabilità dell'app per mantenere e ripristinare lo stato di app in base alle esigenze (ad esempio, in caso di una connessione di rete persa).

Il modello di hosting di server-side offre diversi vantaggi:

* Consente di scrivere l'intera app con .NET e C# usando il modello di componente.
* Fornisce un'idea interattiva avanzata, evitando inutili pagina viene aggiornata.
* Ha una dimensione di app significativamente inferiore rispetto a un'app Blazor lato client e vengono caricati molto più rapidamente.
* Logica dei componenti può usufruire di funzionalità server, incluso l'uso di qualsiasi API compatibili con .NET Core.
* Viene eseguito in .NET Core nel server, quindi .NET esistenti degli strumenti, ad esempio il debug, funziona come previsto.
* Funziona con i thin client (ad esempio, i browser che non supportano WebAssembly e risorse vincolate dispositivi).

Esistono svantaggi lato server di hosting:

* Presenta una latenza più elevata: Ogni interazione dell'utente prevede un hop di rete.
* Non offre alcun supporto offline: Se la connessione del client non riesce, l'app smette di funzionare.
* Ha ridotto la scalabilità: Il server deve gestire più connessioni di client e la gestione dello stato del client.
* Richiede un server di ASP.NET Core per rendere disponibile l'app. Distribuzione senza un server (ad esempio, da una rete CDN) non è possibile.

&dagger;Il *blazor.server.js* lo script viene pubblicato nel seguente percorso: *bin / {Debug | Versione} / {FRAMEWORK di destinazione} /publish/ {nome applicazione}. Dist/App/Framework*.
