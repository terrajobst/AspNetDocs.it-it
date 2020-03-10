---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN nella pipeline integrata di IIS | Microsoft Docs
author: Praburaj
description: Questo articolo illustra come eseguire i componenti del middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare l'evento della pipeline in cui viene eseguito un OMC. È necessario...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617138"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN nella pipeline integrata di IIS

di [bibanunicu Lorenzo](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questo articolo illustra come eseguire i componenti del middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare l'evento della pipeline in cui viene eseguito un OMC. Prima di leggere questa esercitazione, è consigliabile esaminare [una panoramica del progetto Katana e del rilevamento della classe di](an-overview-of-project-katana.md) [avvio OWIN](owin-startup-class-detection.md) . Questa esercitazione è stata scritta da Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, bibanunicu Lorenzo e Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).

Sebbene i componenti del middleware [OWIN](an-overview-of-project-katana.md) (OMCs) siano principalmente progettati per essere eseguiti in una pipeline indipendente dal server, è possibile eseguire un OMC anche nella pipeline integrata di IIS (**la modalità classica *non* è supportata**). È possibile fare un OMC per lavorare nella pipeline integrata di IIS installando il pacchetto seguente dalla console di gestione pacchetti (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Ciò significa che tutti i framework applicazioni, anche quelli che non sono ancora in grado di essere eseguiti all'esterno di IIS e System. Web, possono trarre vantaggio dai componenti middleware OWIN esistenti. 

> [!NOTE]
> Tutti i pacchetti `Microsoft.Owin.Security.*` forniti con il nuovo sistema di identità in Visual Studio 2013 (ad esempio, cookie, account Microsoft, Google, Facebook, Twitter, [token di porta](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, server di autorizzazione, JWT, Azure Active Directory e Active Directory Federation Services) vengono creati come OMCs e possono essere utilizzati in scenari sia indipendenti che ospitati da IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Come viene eseguito il middleware OWIN nella pipeline integrata di IIS

Per le applicazioni console OWIN, la pipeline dell'applicazione compilata usando la [configurazione di avvio](owin-startup-class-detection.md) viene impostata in base all'ordine con cui i componenti vengono aggiunti usando il metodo `IAppBuilder.Use`. Ovvero la pipeline OWIN nel runtime [Katana](an-overview-of-project-katana.md) elaborerà OMCs nell'ordine in cui sono state registrate usando `IAppBuilder.Use`. Nella pipeline integrata di IIS la pipeline delle richieste è costituita da [httpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) sottoscritto a un set predefinito di eventi della pipeline, ad esempio [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)e così via.

Se si confronta un OMC con quello di un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) nel mondo ASP.NET, è necessario registrare un OMC nell'evento della pipeline predefinito corretto. Ad esempio, il `MyModule` HttpModule verrà richiamato quando una richiesta arriva alla fase [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) nella pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Affinché un OMC partecipi a questo stesso ordine di esecuzione basato su eventi, il codice di runtime [Katana](an-overview-of-project-katana.md) analizza la [configurazione di avvio](owin-startup-class-detection.md) e sottoscrive ogni componente del middleware a un evento della pipeline integrato. Ad esempio, il codice di registrazione e OMC seguente consente di visualizzare la registrazione degli eventi predefinita dei componenti del middleware. Per istruzioni più dettagliate sulla creazione di una classe di avvio OWIN, vedere [OWIN Startup Class Detection](owin-startup-class-detection.md).

1. Creare un progetto di applicazione Web vuoto e denominarlo **owin2**.
2. Dalla console di gestione pacchetti (PMC) eseguire il comando seguente: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Aggiungere un `OWIN Startup Class` e denominarlo `Startup`. Sostituire il codice generato con il codice seguente (le modifiche sono evidenziate):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Premere F5 per eseguire l'app.

La configurazione di avvio configura una pipeline con tre componenti middleware, i primi due che visualizzano le informazioni di diagnostica e l'ultimo che risponde agli eventi e visualizzano anche le informazioni di diagnostica. Il metodo `PrintCurrentIntegratedPipelineStage` Visualizza l'evento della pipeline integrata in cui viene richiamato il middleware e un messaggio. Nelle finestre di output vengono visualizzati gli elementi seguenti:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Per impostazione predefinita, il runtime di Katana ha eseguito il mapping di ognuno dei componenti del middleware OWIN a [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) , che corrisponde all'evento [PREREQUESTHANDLEREXECUTE](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)della pipeline IIS.

## <a name="stage-markers"></a>Marcatori di fasi

È possibile contrassegnare OMCs per l'esecuzione in fasi specifiche della pipeline usando il metodo di estensione `IAppBuilder UseStageMarker()`. Per eseguire un set di componenti middleware durante una fase specifica, inserire un marcatore di fase subito dopo l'ultimo componente impostato durante la registrazione. Sono disponibili regole per la fase della pipeline in cui è possibile eseguire il middleware ed è necessario che i componenti dell'ordine vengano eseguiti (le regole sono descritte più avanti nell'esercitazione). Aggiungere il metodo `UseStageMarker` al codice `Configuration`, come illustrato di seguito:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

La chiamata `app.UseStageMarker(PipelineStage.Authenticate)` configura tutti i componenti middleware registrati in precedenza, in questo caso i due componenti di diagnostica, da eseguire nella fase di autenticazione della pipeline. L'ultimo componente middleware, che visualizza la diagnostica e risponde alle richieste, verrà eseguito nella fase `ResolveCache` (evento [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) ).

Premere F5 per eseguire l'app. La finestra di output Mostra quanto segue:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Regole indicatore di fase

Owin middleware Components (OMC) può essere configurato per l'esecuzione nei seguenti eventi della fase della pipeline OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Per impostazione predefinita, OMCs viene eseguito in corrispondenza dell'ultimo evento (`PreHandlerExecute`). Ecco perché il primo codice di esempio ha visualizzato "PreExecuteRequestHandler".
2. È possibile usare il metodo `app.UseStageMarker` per registrare un OMC per l'esecuzione precedente, in qualsiasi fase della pipeline OWIN elencata nel `PipelineStage` enum.
3. La pipeline OWIN e la pipeline IIS sono ordinate, quindi le chiamate a `app.UseStageMarker` devono essere in ordine. Non è possibile impostare il gestore eventi su un evento che precede l'ultimo evento registrato con `app.UseStageMarker`. Ad esempio, *dopo* la chiamata a:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   le chiamate a `app.UseStageMarker` che passano `Authenticate` o `PostAuthenticate` non verranno rispettate e non verrà generata alcuna eccezione. OMCs viene eseguito nella fase più recente, che per impostazione predefinita è `PreHandlerExecute`. I marcatori di fase vengono usati per eseguirli in precedenza. Se si specificano i marcatori di fase non ordinati, viene arrotondato al marcatore precedente. In altre parole, l'aggiunta di un marcatore di fase indica che l'esecuzione non è successiva alla fase X. L'esecuzione di OMC al primo indicatore di fase è stata aggiunta successivamente nella pipeline OWIN.
4. La prima fase delle chiamate a `app.UseStageMarker` WINS. Ad esempio, se si cambia l'ordine delle chiamate `app.UseStageMarker` dall'esempio precedente:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Viene visualizzata la finestra di output: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Tutti i OMCs vengono eseguiti nella fase `AuthenticateRequest`, perché l'ultimo OMC registrato con l'evento `Authenticate` e l'evento `Authenticate` precede tutti gli altri eventi.
