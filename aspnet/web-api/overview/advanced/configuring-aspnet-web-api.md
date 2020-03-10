---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurazione di API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: 'Configurare API Web ASP.NET 2 per ASP.NET 4. x: Configure Settings, ASP.NET 4. x hosting, OWIN self-hosting, Global Services e pre-controller Configuration.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557722"
---
# <a name="configuring-aspnet-web-api-2"></a>Configurazione di API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come configurare API Web ASP.NET.

- [Impostazioni di configurazione](#settings)
- [Configurazione dell'API Web con ASP.NET hosting](#webhost)
- [Configurazione dell'API Web con self-hosting OWIN](#selfhost)
- [Servizi API Web globali](#services)
- [Configurazione per controller](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Impostazioni di configurazione

Le impostazioni di configurazione dell'API Web sono definite nella classe [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) .

| Membro | Description |
| --- | --- |
| **DependencyResolver** | Consente l'inserimento delle dipendenze per i controller. Vedere [uso del sistema di risoluzione delle dipendenze dell'API Web](dependency-injection.md). |
| **Filtri** | Filtri azioni. |
| **Formatters** | [Formattatori del tipo di supporto](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Specifica se nel server devono essere inclusi i dettagli dell'errore, ad esempio i messaggi di eccezione e le analisi dello stack, nei messaggi di risposta HTTP. Vedere [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inizializzatore** | Funzione che esegue l'inizializzazione finale di **HttpConfiguration**. |
| **MessageHandlers** | [Gestori di messaggi http](http-message-handlers.md). |
| **ParameterBindingRules** | Raccolta di regole per l'associazione di parametri sulle azioni del controller. |
| **Proprietà** | Contenitore di proprietà generico. |
| **Route** | Raccolta di route. Vedere [routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Servizi** | Raccolta di servizi. Vedere [Servizi](#services). |

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurazione dell'API Web con ASP.NET hosting

In un'applicazione ASP.NET configurare l'API Web chiamando [GlobalConfiguration. Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) nel metodo di **avvio dell'applicazione\_** . Il metodo **Configure** accetta un delegato con un solo parametro di tipo **HttpConfiguration**. Eseguire tutte le operazioni di configurazione all'interno del delegato.

Di seguito è riportato un esempio di utilizzo di un delegato anonimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017 il modello di progetto "applicazione Web ASP.NET" Configura automaticamente il codice di configurazione, se si seleziona "API Web" nella finestra di dialogo **nuovo progetto ASP.NET** .

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Il modello di progetto crea un file denominato WebApiConfig.cs all'interno dell'app\_cartella di avvio. Questo file di codice definisce il delegato in cui è necessario inserire il codice di configurazione dell'API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Il modello di progetto aggiunge anche il codice che chiama il delegato dall' **applicazione\_avvio**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurazione dell'API Web con self-hosting OWIN

Se si esegue l'hosting self-service con OWIN, creare una nuova istanza di **HttpConfiguration** . Eseguire qualsiasi configurazione in questa istanza, quindi passare l'istanza al metodo di estensione **Owin. UseWebApi** .

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

L'esercitazione [USA OWIN per ospitare autonomamente API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) Mostra i passaggi completi.

<a id="services"></a>
## <a name="global-web-api-services"></a>Servizi API Web globali

La raccolta **HttpConfiguration. Services** contiene un set di servizi globali usato dall'API Web per eseguire varie attività, ad esempio la selezione del controller e la negoziazione del contenuto.

> [!NOTE]
> La raccolta **Services** non è un meccanismo generico per l'individuazione del servizio o l'inserimento delle dipendenze. Archivia solo i tipi di servizio noti al Framework API Web.

La raccolta **Services** viene inizializzata con un set predefinito di servizi ed è possibile fornire implementazioni personalizzate. Alcuni servizi supportano più istanze, mentre altre possono avere una sola istanza. Tuttavia, è anche possibile fornire servizi a livello di controller. vedere [configurazione per controller](#percontrollerconfig).

Servizi a istanza singola

| Service | Description |
| --- | --- |
| **IActionValueBinder** | Ottiene un'associazione per un parametro. |
| **IApiExplorer** | Ottiene le descrizioni delle API esposte dall'applicazione. Vedere [creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Ottiene un elenco degli assembly per l'applicazione. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Convalida un modello che viene letto dal corpo della richiesta da un formattatore del tipo di supporto. |
| **IContentNegotiator** | Esegue la negoziazione del contenuto. |
| **IDocumentationProvider** | Fornisce la documentazione per le API. Il valore predefinito è **null**. Vedere [creazione di una pagina della Guida per un'API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se l'host deve memorizzare nel buffer i corpi delle entità dei messaggi HTTP. |
| **IHttpActionInvoker** | Richiama un'azione del controller. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleziona un'azione del controller. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Attiva un controller. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleziona un controller. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornisce un elenco dei tipi di controller API Web nell'applicazione. Vedere [routing e selezione delle azioni](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inizializza il Framework di traccia. Vedere [traccia in API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornisce un writer di traccia. Il valore predefinito è un writer di traccia "no-op". Vedere [traccia in API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornisce una cache di validator del modello. |

Servizi a più istanze

|                 Service                 |                                                                                                              Description                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Restituisce un elenco di filtri per un'azione del controller.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Restituisce uno strumento di associazione di modelli per un tipo specificato.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fornisce i metadati per un modello.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fornisce un validator per un modello.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Crea un provider di valori. Per altre informazioni, vedere il post di Blog su Mike Stall [come creare un provider di valori personalizzati in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Per aggiungere un'implementazione personalizzata a un servizio a istanze diverse, chiamare **Add** o **Insert** nella raccolta **Services** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Per sostituire un servizio a istanza singola con un'implementazione personalizzata, chiamare **Replace** nella raccolta **Services** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configurazione per controller

È possibile eseguire l'override delle impostazioni seguenti in base al controller:

- Formattatori di tipo multimediale
- Regole di associazione di parametri
- Servizi

A tale scopo, definire un attributo personalizzato che implementi l'interfaccia **IControllerConfiguration** . Applicare quindi l'attributo al controller.

Nell'esempio seguente vengono sostituiti i formattatori del tipo di supporto predefiniti con un formattatore personalizzato.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Il metodo **IControllerConfiguration. Initialize** accetta due parametri:

- Oggetto **HttpControllerSettings**
- Oggetto **HttpControllerDescriptor**

Il **HttpControllerDescriptor** contiene una descrizione del controller, che è possibile esaminare a scopo informativo (ad indicare, per distinguere tra due controller).

Usare l'oggetto **HttpControllerSettings** per configurare il controller. Questo oggetto contiene il subset di parametri di configurazione di cui è possibile eseguire l'override in base ai singoli controller. Tutte le impostazioni che non vengono modificate per impostazione predefinita nell'oggetto **HttpConfiguration** globale.
