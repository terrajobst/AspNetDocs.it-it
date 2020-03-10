---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Traccia in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come abilitare la traccia in API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598553"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Traccia in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

> Quando si tenta di eseguire il debug di un'applicazione basata sul Web, non è possibile sostituire un set di log di traccia valido. Questa esercitazione illustra come abilitare la traccia in API Web ASP.NET. È possibile usare questa funzionalità per tracciare le operazioni del Framework API Web prima e dopo aver richiamato il controller. È anche possibile usarlo per tracciare il proprio codice.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - [Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funziona anche con visual studio 2015)
> - API Web 2
> - [Microsoft. AspNet. WebApi. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Abilitare la traccia System. Diagnostics nell'API Web

In primo luogo, verrà creato un nuovo progetto di applicazione Web ASP.NET. In Visual Studio scegliere **nuovo** > **progetto**dal menu **file** . In **modelli**, **Web**, selezionare **applicazione Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Scegliere il modello di progetto API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**e quindi **Gestione pacchetti console**.

Nella finestra console di gestione pacchetti digitare i comandi seguenti.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Il primo comando installa il pacchetto di analisi dell'API Web più recente. Aggiorna inoltre i pacchetti dell'API Web di base. Il secondo comando Aggiorna il pacchetto WebApi. hosting alla versione più recente.

> [!NOTE]
> Se si vuole impostare come destinazione una versione specifica dell'API Web, usare il flag-Version quando si installa il pacchetto di traccia.

Aprire il file WebApiConfig.cs nella cartella di avvio dell'app\_. Aggiungere il codice seguente al metodo **Register** .

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Questo codice aggiunge la classe [oggetto SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) alla pipeline dell'API Web. La classe **oggetto SystemDiagnosticsTraceWriter** scrive le tracce in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Per visualizzare le tracce, eseguire l'applicazione nel debugger. Nel browser passare a `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Le istruzioni di traccia vengono scritte nella finestra di output in Visual Studio. Scegliere **output**dal menu **Visualizza** .

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Poiché **oggetto SystemDiagnosticsTraceWriter** scrive tracce in **System. Diagnostics. Trace**, è possibile registrare listener di traccia aggiuntivi; ad esempio, per scrivere tracce in un file di log. Per ulteriori informazioni sui writer di traccia, vedere l'argomento [listener di traccia](https://msdn.microsoft.com/library/4y5y10s7.aspx) su MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurazione di oggetto SystemDiagnosticsTraceWriter

Nel codice seguente viene illustrato come configurare il writer di traccia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Sono disponibili due impostazioni che è possibile controllare:

- Noverbose: se false, ogni traccia contiene informazioni minime. Se true, le tracce includono ulteriori informazioni.
- MinimumLevel: imposta il livello di traccia minimo. I livelli di traccia, in ordine, sono debug, info, Warn, Error e Fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Aggiunta di tracce all'applicazione API Web

L'aggiunta di un writer di traccia consente di accedere immediatamente alle tracce create dalla pipeline dell'API Web. È anche possibile usare il writer di traccia per tracciare il proprio codice:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Per ottenere il writer di traccia, chiamare **HttpConfiguration. Services. GetTraceWriter**. Da un controller, questo metodo è accessibile tramite la proprietà **ApiController. Configuration** .

Per scrivere una traccia, è possibile chiamare direttamente il metodo **ITraceWriter. Trace** , ma la classe [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definisce alcuni metodi di estensione più semplici. Ad esempio, il metodo **info** illustrato sopra crea una traccia con le **informazioni**sul livello di traccia.

## <a name="web-api-tracing-infrastructure"></a>Infrastruttura di traccia dell'API Web

In questa sezione viene descritto come scrivere un writer di traccia personalizzato per l'API Web.

Il pacchetto Microsoft. AspNet. WebApi. Tracing si basa su un'infrastruttura di traccia più generale nell'API Web. Invece di usare Microsoft. AspNet. WebApi. Tracing, è anche possibile collegare alcune altre librerie di traccia/registrazione, ad esempio [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).

Per raccogliere le tracce, implementare l'interfaccia **ITraceWriter** . Di seguito è riportato un semplice esempio:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Il metodo **ITraceWriter. Trace** crea una traccia. Il chiamante specifica una categoria e un livello di traccia. La categoria può essere qualsiasi stringa definita dall'utente. L'implementazione di **Trace** deve eseguire le operazioni seguenti:

1. Creare un nuovo **TraceRecord**. Inizializzarla con la richiesta, la categoria e il livello di traccia, come illustrato. Questi valori vengono forniti dal chiamante.
2. Richiamare il delegato *traceAction* . All'interno di questo delegato, il chiamante deve compilare il resto del **TraceRecord**.
3. Scrivere il **TraceRecord**, usando qualsiasi tecnica di registrazione. Nell'esempio riportato di seguito viene semplicemente eseguita una chiamata a **System. Diagnostics. Trace**.

## <a name="setting-the-trace-writer"></a>Impostazione del writer di traccia

Per abilitare la traccia, è necessario configurare l'API Web per l'uso dell'implementazione di **ITraceWriter** . Questa operazione viene eseguita tramite l'oggetto **HttpConfiguration** , come illustrato nel codice seguente:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Può essere attivo un solo writer di traccia. Per impostazione predefinita, l'API Web imposta un &quot;no-op&quot; Tracer che non esegue alcuna operazione. (Il &quot;non è presente alcuna operazione&quot; Tracer, in modo che il codice di traccia non debba controllare se il writer di traccia è **null** prima di scrivere una traccia).

## <a name="how-web-api-tracing-works"></a>Funzionamento della traccia dell'API Web

La traccia nell'API Web usa uno schema di *facciata* : quando la traccia è abilitata, l'API Web esegue il wrapping di varie parti della pipeline delle richieste con classi che eseguono chiamate di traccia.

Ad esempio, quando si seleziona un controller, la pipeline usa l'interfaccia **IHttpControllerSelector** . Con la traccia abilitata, la pipeline inserisce una classe che implementa **IHttpControllerSelector** ma chiama l'implementazione reale:

![La traccia dell'API Web usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

I vantaggi di questa progettazione includono:

- Se non si aggiunge un writer di traccia, non viene creata un'istanza dei componenti di traccia e non si verifica alcun effetto sulle prestazioni.
- Se si sostituiscono i servizi predefiniti, ad esempio **IHttpControllerSelector** con un'implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.

È anche possibile sostituire l'intero framework di analisi dell'API Web con il Framework personalizzato, sostituendo il servizio **ITraceManager** predefinito:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementare **ITraceManager. Initialize** per inizializzare il sistema di traccia. Tenere presente che sostituisce l' *intero* Framework di traccia, incluso tutto il codice di traccia incorporato nell'API Web.
