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
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="44bdf-103">Traccia in API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="44bdf-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="44bdf-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="44bdf-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="44bdf-105">Quando si tenta di eseguire il debug di un'applicazione basata sul Web, non è possibile sostituire un set di log di traccia valido.</span><span class="sxs-lookup"><span data-stu-id="44bdf-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="44bdf-106">Questa esercitazione illustra come abilitare la traccia in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="44bdf-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="44bdf-107">È possibile usare questa funzionalità per tracciare le operazioni del Framework API Web prima e dopo aver richiamato il controller.</span><span class="sxs-lookup"><span data-stu-id="44bdf-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="44bdf-108">È anche possibile usarlo per tracciare il proprio codice.</span><span class="sxs-lookup"><span data-stu-id="44bdf-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="44bdf-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="44bdf-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="44bdf-110">[Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funziona anche con visual studio 2015)</span><span class="sxs-lookup"><span data-stu-id="44bdf-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="44bdf-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="44bdf-111">Web API 2</span></span>
> - [<span data-ttu-id="44bdf-112">Microsoft. AspNet. WebApi. Tracing</span><span class="sxs-lookup"><span data-stu-id="44bdf-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="44bdf-113">Abilitare la traccia System. Diagnostics nell'API Web</span><span class="sxs-lookup"><span data-stu-id="44bdf-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="44bdf-114">In primo luogo, verrà creato un nuovo progetto di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="44bdf-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="44bdf-115">In Visual Studio scegliere **nuovo** > **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="44bdf-116">In **modelli**, **Web**, selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="44bdf-117">Scegliere il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="44bdf-118">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**e quindi **Gestione pacchetti console**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="44bdf-119">Nella finestra console di gestione pacchetti digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="44bdf-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="44bdf-120">Il primo comando installa il pacchetto di analisi dell'API Web più recente.</span><span class="sxs-lookup"><span data-stu-id="44bdf-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="44bdf-121">Aggiorna inoltre i pacchetti dell'API Web di base.</span><span class="sxs-lookup"><span data-stu-id="44bdf-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="44bdf-122">Il secondo comando Aggiorna il pacchetto WebApi. hosting alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="44bdf-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="44bdf-123">Se si vuole impostare come destinazione una versione specifica dell'API Web, usare il flag-Version quando si installa il pacchetto di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="44bdf-124">Aprire il file WebApiConfig.cs nella cartella di avvio dell'app\_.</span><span class="sxs-lookup"><span data-stu-id="44bdf-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="44bdf-125">Aggiungere il codice seguente al metodo **Register** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="44bdf-126">Questo codice aggiunge la classe [oggetto SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) alla pipeline dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="44bdf-127">La classe **oggetto SystemDiagnosticsTraceWriter** scrive le tracce in [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="44bdf-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="44bdf-128">Per visualizzare le tracce, eseguire l'applicazione nel debugger.</span><span class="sxs-lookup"><span data-stu-id="44bdf-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="44bdf-129">Nel browser passare a `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="44bdf-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="44bdf-130">Le istruzioni di traccia vengono scritte nella finestra di output in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44bdf-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="44bdf-131">Scegliere **output**dal menu **Visualizza** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="44bdf-132">Poiché **oggetto SystemDiagnosticsTraceWriter** scrive tracce in **System. Diagnostics. Trace**, è possibile registrare listener di traccia aggiuntivi; ad esempio, per scrivere tracce in un file di log.</span><span class="sxs-lookup"><span data-stu-id="44bdf-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="44bdf-133">Per ulteriori informazioni sui writer di traccia, vedere l'argomento [listener di traccia](https://msdn.microsoft.com/library/4y5y10s7.aspx) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="44bdf-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="44bdf-134">Configurazione di oggetto SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="44bdf-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="44bdf-135">Nel codice seguente viene illustrato come configurare il writer di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="44bdf-136">Sono disponibili due impostazioni che è possibile controllare:</span><span class="sxs-lookup"><span data-stu-id="44bdf-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="44bdf-137">Noverbose: se false, ogni traccia contiene informazioni minime.</span><span class="sxs-lookup"><span data-stu-id="44bdf-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="44bdf-138">Se true, le tracce includono ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="44bdf-138">If true, traces include more information.</span></span>
- <span data-ttu-id="44bdf-139">MinimumLevel: imposta il livello di traccia minimo.</span><span class="sxs-lookup"><span data-stu-id="44bdf-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="44bdf-140">I livelli di traccia, in ordine, sono debug, info, Warn, Error e Fatal.</span><span class="sxs-lookup"><span data-stu-id="44bdf-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="44bdf-141">Aggiunta di tracce all'applicazione API Web</span><span class="sxs-lookup"><span data-stu-id="44bdf-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="44bdf-142">L'aggiunta di un writer di traccia consente di accedere immediatamente alle tracce create dalla pipeline dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="44bdf-143">È anche possibile usare il writer di traccia per tracciare il proprio codice:</span><span class="sxs-lookup"><span data-stu-id="44bdf-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="44bdf-144">Per ottenere il writer di traccia, chiamare **HttpConfiguration. Services. GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="44bdf-145">Da un controller, questo metodo è accessibile tramite la proprietà **ApiController. Configuration** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="44bdf-146">Per scrivere una traccia, è possibile chiamare direttamente il metodo **ITraceWriter. Trace** , ma la classe [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definisce alcuni metodi di estensione più semplici.</span><span class="sxs-lookup"><span data-stu-id="44bdf-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="44bdf-147">Ad esempio, il metodo **info** illustrato sopra crea una traccia con le **informazioni**sul livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="44bdf-148">Infrastruttura di traccia dell'API Web</span><span class="sxs-lookup"><span data-stu-id="44bdf-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="44bdf-149">In questa sezione viene descritto come scrivere un writer di traccia personalizzato per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="44bdf-150">Il pacchetto Microsoft. AspNet. WebApi. Tracing si basa su un'infrastruttura di traccia più generale nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="44bdf-151">Invece di usare Microsoft. AspNet. WebApi. Tracing, è anche possibile collegare alcune altre librerie di traccia/registrazione, ad esempio [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="44bdf-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="44bdf-152">Per raccogliere le tracce, implementare l'interfaccia **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="44bdf-153">Di seguito è riportato un semplice esempio:</span><span class="sxs-lookup"><span data-stu-id="44bdf-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="44bdf-154">Il metodo **ITraceWriter. Trace** crea una traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="44bdf-155">Il chiamante specifica una categoria e un livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="44bdf-156">La categoria può essere qualsiasi stringa definita dall'utente.</span><span class="sxs-lookup"><span data-stu-id="44bdf-156">The category can be any user-defined string.</span></span> <span data-ttu-id="44bdf-157">L'implementazione di **Trace** deve eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="44bdf-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="44bdf-158">Creare un nuovo **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="44bdf-159">Inizializzarla con la richiesta, la categoria e il livello di traccia, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="44bdf-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="44bdf-160">Questi valori vengono forniti dal chiamante.</span><span class="sxs-lookup"><span data-stu-id="44bdf-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="44bdf-161">Richiamare il delegato *traceAction* .</span><span class="sxs-lookup"><span data-stu-id="44bdf-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="44bdf-162">All'interno di questo delegato, il chiamante deve compilare il resto del **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="44bdf-163">Scrivere il **TraceRecord**, usando qualsiasi tecnica di registrazione.</span><span class="sxs-lookup"><span data-stu-id="44bdf-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="44bdf-164">Nell'esempio riportato di seguito viene semplicemente eseguita una chiamata a **System. Diagnostics. Trace**.</span><span class="sxs-lookup"><span data-stu-id="44bdf-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="44bdf-165">Impostazione del writer di traccia</span><span class="sxs-lookup"><span data-stu-id="44bdf-165">Setting the Trace Writer</span></span>

<span data-ttu-id="44bdf-166">Per abilitare la traccia, è necessario configurare l'API Web per l'uso dell'implementazione di **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="44bdf-167">Questa operazione viene eseguita tramite l'oggetto **HttpConfiguration** , come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="44bdf-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="44bdf-168">Può essere attivo un solo writer di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-168">Only one trace writer can be active.</span></span> <span data-ttu-id="44bdf-169">Per impostazione predefinita, l'API Web imposta un &quot;no-op&quot; Tracer che non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="44bdf-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="44bdf-170">(Il &quot;non è presente alcuna operazione&quot; Tracer, in modo che il codice di traccia non debba controllare se il writer di traccia è **null** prima di scrivere una traccia).</span><span class="sxs-lookup"><span data-stu-id="44bdf-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="44bdf-171">Funzionamento della traccia dell'API Web</span><span class="sxs-lookup"><span data-stu-id="44bdf-171">How Web API Tracing Works</span></span>

<span data-ttu-id="44bdf-172">La traccia nell'API Web usa uno schema di *facciata* : quando la traccia è abilitata, l'API Web esegue il wrapping di varie parti della pipeline delle richieste con classi che eseguono chiamate di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="44bdf-173">Ad esempio, quando si seleziona un controller, la pipeline usa l'interfaccia **IHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="44bdf-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="44bdf-174">Con la traccia abilitata, la pipeline inserisce una classe che implementa **IHttpControllerSelector** ma chiama l'implementazione reale:</span><span class="sxs-lookup"><span data-stu-id="44bdf-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![La traccia dell'API Web usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="44bdf-176">I vantaggi di questa progettazione includono:</span><span class="sxs-lookup"><span data-stu-id="44bdf-176">The benefits of this design include:</span></span>

- <span data-ttu-id="44bdf-177">Se non si aggiunge un writer di traccia, non viene creata un'istanza dei componenti di traccia e non si verifica alcun effetto sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="44bdf-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="44bdf-178">Se si sostituiscono i servizi predefiniti, ad esempio **IHttpControllerSelector** con un'implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.</span><span class="sxs-lookup"><span data-stu-id="44bdf-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="44bdf-179">È anche possibile sostituire l'intero framework di analisi dell'API Web con il Framework personalizzato, sostituendo il servizio **ITraceManager** predefinito:</span><span class="sxs-lookup"><span data-stu-id="44bdf-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="44bdf-180">Implementare **ITraceManager. Initialize** per inizializzare il sistema di traccia.</span><span class="sxs-lookup"><span data-stu-id="44bdf-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="44bdf-181">Tenere presente che sostituisce l' *intero* Framework di traccia, incluso tutto il codice di traccia incorporato nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="44bdf-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
