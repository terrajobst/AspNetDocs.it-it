---
title: Inserimento di dipendenze nelle visualizzazioni in ASP.NET Core
author: ardalis
description: Informazioni sulle modalità con cui ASP.NET Core supporta l'inserimento di dipendenze nelle visualizzazioni MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: dfadafe9ebb5799b45ef68653f20c5fc1a2506b5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025458"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="65b8d-103">Inserimento di dipendenze nelle visualizzazioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65b8d-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="65b8d-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="65b8d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="65b8d-105">ASP.NET Core supporta l'[inserimento di dipendenze](xref:fundamentals/dependency-injection) nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="65b8d-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="65b8d-106">Questo può essere utile per i servizi specifici delle visualizzazioni, ad esempio per la localizzazione o per dati necessari solo per il popolamento degli elementi delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="65b8d-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="65b8d-107">È consigliabile mantenere la [separazione delle competenze](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) tra i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="65b8d-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="65b8d-108">La maggior parte dei dati nelle visualizzazioni devono essere passati dal controller.</span><span class="sxs-lookup"><span data-stu-id="65b8d-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="65b8d-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="65b8d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="65b8d-110">Un semplice esempio</span><span class="sxs-lookup"><span data-stu-id="65b8d-110">A Simple Example</span></span>

<span data-ttu-id="65b8d-111">È possibile inserire un servizio in una visualizzazione usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="65b8d-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="65b8d-112">È possibile considerare `@inject` come l'aggiunta di una proprietà alla visualizzazione, con il popolamento della proprietà tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="65b8d-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="65b8d-113">Sintassi per `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="65b8d-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="65b8d-114">Esempio di `@inject` in azione:</span><span class="sxs-lookup"><span data-stu-id="65b8d-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="65b8d-115">Questa visualizzazione presenta un elenco di istanze di `ToDoItem`, nonché un riepilogo con statistiche generali.</span><span class="sxs-lookup"><span data-stu-id="65b8d-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="65b8d-116">Il riepilogo è popolato dal servizio `StatisticsService` inserito.</span><span class="sxs-lookup"><span data-stu-id="65b8d-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="65b8d-117">Questo servizio è registrato per l'inserimento di dipendenze in `ConfigureServices` in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="65b8d-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="65b8d-118">`StatisticsService` esegue alcuni calcoli per il set di istanze di `ToDoItem`, a cui accede tramite un repository:</span><span class="sxs-lookup"><span data-stu-id="65b8d-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="65b8d-119">Il repository di esempio usa una raccolta in memoria.</span><span class="sxs-lookup"><span data-stu-id="65b8d-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="65b8d-120">L'implementazione illustrata in precedenza, che opera su tutti i dati in memoria, non è consigliata per set di dati di grandi dimensioni a cui si accede in remoto.</span><span class="sxs-lookup"><span data-stu-id="65b8d-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="65b8d-121">L'esempio visualizza i dati del modello associato alla visualizzazione e il servizio inserito nella visualizzazione stessa:</span><span class="sxs-lookup"><span data-stu-id="65b8d-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Visualizzazione To Do con l'elenco degli elementi totali, degli elementi completati e della priorità media, e con un elenco di attività con i relativi livelli di priorità e i valori booleani che indicano il completamento.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="65b8d-123">Popolamento di dati di ricerca</span><span class="sxs-lookup"><span data-stu-id="65b8d-123">Populating Lookup Data</span></span>

<span data-ttu-id="65b8d-124">L'inserimento di visualizzazioni può essere utile per popolare le opzioni negli elementi dell'interfaccia utente, ad esempio negli elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="65b8d-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="65b8d-125">Si consideri un modulo di profilo utente che include opzioni che consentono di specificare il sesso, lo stato e altre preferenze.</span><span class="sxs-lookup"><span data-stu-id="65b8d-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="65b8d-126">Per il rendering di un form di questo tipo tramite un approccio MVC standard sarebbe necessario che il controller richiedesse servizi di accesso ai dati per ognuno di questi set di opzioni e quindi popolasse un modello o un `ViewBag` con ogni set di opzioni da associare.</span><span class="sxs-lookup"><span data-stu-id="65b8d-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="65b8d-127">Un approccio alternativo consiste nell'ottenere le opzioni inserendo i servizi direttamente nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="65b8d-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="65b8d-128">In questo modo la quantità di codice necessario per il controller viene ridotta al minimo, dato che la logica di costruzione di questo elemento di visualizzazione viene spostata nella visualizzazione stessa.</span><span class="sxs-lookup"><span data-stu-id="65b8d-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="65b8d-129">L'azione del controller per la visualizzazione di un modulo di modifica del profilo deve solo passare l'istanza del profilo al modulo:</span><span class="sxs-lookup"><span data-stu-id="65b8d-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="65b8d-130">Il form HTML usato per aggiornare queste preferenze include elenchi a discesa per tre delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="65b8d-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Visualizzazione Update Profile con un modulo che consente l'immissione di nome, sesso, stato e colore preferito.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="65b8d-132">Questi elenchi vengono popolati da un servizio inserito nella visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="65b8d-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="65b8d-133">`ProfileOptionsService` è un servizio a livello di interfaccia utente progettato per fornire solo i dati necessari per il modulo:</span><span class="sxs-lookup"><span data-stu-id="65b8d-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="65b8d-134">Non si deve dimenticare di registrare i tipi che si richiedono tramite l'inserimento di dipendenze nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="65b8d-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="65b8d-135">Un tipo non registrato genera un'eccezione in fase di runtime, perché viene eseguita internamente una query al provider di servizi tramite [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="65b8d-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="65b8d-136">Override di servizi</span><span class="sxs-lookup"><span data-stu-id="65b8d-136">Overriding Services</span></span>

<span data-ttu-id="65b8d-137">Oltre all'inserimento di nuovi servizi, questa tecnica può essere usata anche per eseguire l'override di servizi precedentemente inseriti in una pagina.</span><span class="sxs-lookup"><span data-stu-id="65b8d-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="65b8d-138">La figura seguente mostra tutti i campi disponibili nella pagina usata nel primo esempio:</span><span class="sxs-lookup"><span data-stu-id="65b8d-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu di scelta rapida di IntelliSense in un simbolo @ tipizzato con l'elenco dei campi Html, Component, StatsService e Url](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="65b8d-140">Come si può notare, i campi predefiniti includono `Html`, `Component`, e `Url`, nonché il servizio `StatsService` inserito.</span><span class="sxs-lookup"><span data-stu-id="65b8d-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="65b8d-141">Se ad esempio si vogliono sostituire gli helper HTML predefiniti con helper personalizzati, è possibile farlo facilmente tramite `@inject`:</span><span class="sxs-lookup"><span data-stu-id="65b8d-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="65b8d-142">Se si vuole estendere i servizi esistenti, è sufficiente usare questa tecnica mentre si eredita dall'implementazione esistente o si esegue il wrapping di quest'ultima con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="65b8d-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="65b8d-143">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="65b8d-143">See Also</span></span>

* <span data-ttu-id="65b8d-144">Blog di Simon Timms: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/) (Recupero dei dati di ricerca nella visualizzazione)</span><span class="sxs-lookup"><span data-stu-id="65b8d-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>