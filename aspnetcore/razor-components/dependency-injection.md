---
title: Inserimento di dipendenze componenti Razor
author: guardrex
description: Scopri App Blazor e Razor componenti possono utilizzare servizi incorporati inserendoli nei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042428"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="509ef-103">Inserimento di dipendenze componenti Razor</span><span class="sxs-lookup"><span data-stu-id="509ef-103">Razor Components dependency injection</span></span>

<span data-ttu-id="509ef-104">Da [Stropek Rainer](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="509ef-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="509ef-105">[Inserimento di dipendenze ()](/aspnet/core/fundamentals/dependency-injection) è incorporato.</span><span class="sxs-lookup"><span data-stu-id="509ef-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="509ef-106">Le app possono usare i servizi incorporati inserendoli nei componenti.</span><span class="sxs-lookup"><span data-stu-id="509ef-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="509ef-107">Le app possono anche definire servizi personalizzati e renderli disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="509ef-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="509ef-108">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="509ef-108">Dependency injection</span></span>

<span data-ttu-id="509ef-109">L'inserimento delle dipendenze è una tecnica per l'accesso ai servizi configurati in una posizione centrale.</span><span class="sxs-lookup"><span data-stu-id="509ef-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="509ef-110">Ciò può risultare utile per:</span><span class="sxs-lookup"><span data-stu-id="509ef-110">This can be useful to:</span></span>

* <span data-ttu-id="509ef-111">Condividere una singola istanza di una classe di servizio tra molti componenti (noto come un *singleton* servizio).</span><span class="sxs-lookup"><span data-stu-id="509ef-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="509ef-112">Separare i componenti dalle classi di servizio concreta specifico e fare riferimento solo a astrazioni.</span><span class="sxs-lookup"><span data-stu-id="509ef-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="509ef-113">Ad esempio, un'interfaccia `IDataAccess` viene implementata da una classe concreta `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="509ef-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="509ef-114">Quando un componente Usa l'inserimento delle dipendenze per ricevere un `IDataAccess` implementazione, il componente non è associato al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="509ef-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="509ef-115">L'implementazione è possibile scambiare, forse a un'implementazione fittizia negli unit test.</span><span class="sxs-lookup"><span data-stu-id="509ef-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="509ef-116">Il sistema di inserimento delle dipendenze è responsabile della fornitura delle istanze dei servizi per i componenti.</span><span class="sxs-lookup"><span data-stu-id="509ef-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="509ef-117">L'inserimento delle dipendenze risolve inoltre le dipendenze in modo ricorsivo in modo che i servizi stessi possono dipendono dai servizi ulteriormente.</span><span class="sxs-lookup"><span data-stu-id="509ef-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="509ef-118">L'inserimento delle dipendenze viene configurato durante l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="509ef-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="509ef-119">Un esempio è illustrato più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="509ef-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="509ef-120">Aggiungere servizi per l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="509ef-120">Add services to DI</span></span>

<span data-ttu-id="509ef-121">Dopo aver creato una nuova app, esaminare il `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="509ef-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="509ef-122">Il `ConfigureServices` viene passato un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), ovvero un elenco di oggetti descrittore di servizio ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="509ef-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="509ef-123">I servizi vengono aggiunti, fornendo i descrittori di servizio per l'insieme al servizio.</span><span class="sxs-lookup"><span data-stu-id="509ef-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="509ef-124">Esempio di codice seguente viene illustrato il concetto:</span><span class="sxs-lookup"><span data-stu-id="509ef-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="509ef-125">Servizi possono essere configurati con le durate seguenti:</span><span class="sxs-lookup"><span data-stu-id="509ef-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="509ef-126">Metodo</span><span class="sxs-lookup"><span data-stu-id="509ef-126">Method</span></span>      | <span data-ttu-id="509ef-127">Descrizione</span><span class="sxs-lookup"><span data-stu-id="509ef-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="509ef-128">Singleton</span><span class="sxs-lookup"><span data-stu-id="509ef-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="509ef-129">L'inserimento delle dipendenze consente di creare un *singola istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="509ef-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="509ef-130">Tutti i componenti che richiedono questo servizio ricevono un riferimento a questa istanza.</span><span class="sxs-lookup"><span data-stu-id="509ef-130">All components requiring this service receive a reference to this instance.</span></span> |
| [<span data-ttu-id="509ef-131">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="509ef-131">Transient</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | <span data-ttu-id="509ef-132">Ogni volta che un componente richiede questo servizio, riceve un *nuova istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="509ef-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| [<span data-ttu-id="509ef-133">Con ambito</span><span class="sxs-lookup"><span data-stu-id="509ef-133">Scoped</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | <span data-ttu-id="509ef-134">Blazor lato client non ha attualmente il concetto di ambiti di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="509ef-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="509ef-135">`Scoped` si comporta come `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="509ef-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="509ef-136">Tuttavia, i componenti di ASP.NET Core Razor supporta il `Scoped` durata.</span><span class="sxs-lookup"><span data-stu-id="509ef-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="509ef-137">In un componente di Razor, la registrazione di un servizio con ambito ha come ambita la connessione.</span><span class="sxs-lookup"><span data-stu-id="509ef-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="509ef-138">Per questo motivo, è preferibile per i servizi che devono avere come ambiti l'utente corrente usando servizi con ambiti (anche se si intende corrente per l'esecuzione del client nel browser).</span><span class="sxs-lookup"><span data-stu-id="509ef-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="509ef-139">Il sistema di inserimento delle dipendenze si basa sul sistema di inserimento delle dipendenze in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="509ef-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="509ef-140">Per altre informazioni, vedere [inserimento delle dipendenze in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="509ef-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="509ef-141">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="509ef-141">Default services</span></span>

<span data-ttu-id="509ef-142">I servizi predefiniti vengono aggiunti automaticamente alla raccolta del servizio di un'app.</span><span class="sxs-lookup"><span data-stu-id="509ef-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="509ef-143">La tabella seguente illustra alcuni dei servizi predefiniti utili forniti.</span><span class="sxs-lookup"><span data-stu-id="509ef-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="509ef-144">Metodo</span><span class="sxs-lookup"><span data-stu-id="509ef-144">Method</span></span>       | <span data-ttu-id="509ef-145">Descrizione</span><span class="sxs-lookup"><span data-stu-id="509ef-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="509ef-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="509ef-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="509ef-147">Fornisce metodi per inviare richieste HTTP e ricevere risposte HTTP da una risorsa identificata da un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="509ef-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="509ef-148">Si noti che questa istanza di `HttpClient` Usa il browser per la gestione del traffico HTTP in background.</span><span class="sxs-lookup"><span data-stu-id="509ef-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="509ef-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) viene impostata automaticamente per il prefisso URI di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="509ef-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="509ef-150">`HttpClient` viene fornito solo per le app Blazor lato client.</span><span class="sxs-lookup"><span data-stu-id="509ef-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="509ef-151">Rappresenta un'istanza di un runtime JavaScript a cui possono essere inviate chiamate.</span><span class="sxs-lookup"><span data-stu-id="509ef-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="509ef-152">Per altre informazioni, vedere <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="509ef-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="509ef-153">Helper per l'uso di URI e l'esplorazione dello stato (singleton).</span><span class="sxs-lookup"><span data-stu-id="509ef-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="509ef-154">`IUriHelper` viene fornito per entrambe le app ASP.NET Core Razor componenti e Blazor di lato client.</span><span class="sxs-lookup"><span data-stu-id="509ef-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="509ef-155">Si noti che è possibile usare un provider di servizi personalizzati anziché il provider del servizio predefinito che viene aggiunto per il modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="509ef-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="509ef-156">Un provider di servizi personalizzati non fornisce automaticamente i servizi predefiniti elencati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="509ef-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="509ef-157">Tali servizi devono essere aggiunto in modo esplicito per nuovo provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="509ef-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="509ef-158">Richiesta di un servizio in un componente</span><span class="sxs-lookup"><span data-stu-id="509ef-158">Request a service in a component</span></span>

<span data-ttu-id="509ef-159">Dopo che i servizi vengono aggiunti alla raccolta di servizio, può essere inseriti all'interno di modelli Razor dei componenti utilizzando il `@inject` direttiva Razor.</span><span class="sxs-lookup"><span data-stu-id="509ef-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="509ef-160">`@inject` presenta due parametri:</span><span class="sxs-lookup"><span data-stu-id="509ef-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="509ef-161">Nome del tipo: Il tipo del servizio da inserire.</span><span class="sxs-lookup"><span data-stu-id="509ef-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="509ef-162">Nome proprietà: Il nome della proprietà la ricezione del servizio app inserito.</span><span class="sxs-lookup"><span data-stu-id="509ef-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="509ef-163">Si noti che la proprietà non richiede la creazione manuale.</span><span class="sxs-lookup"><span data-stu-id="509ef-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="509ef-164">Il compilatore crea la proprietà.</span><span class="sxs-lookup"><span data-stu-id="509ef-164">The compiler creates the property.</span></span>

<span data-ttu-id="509ef-165">Più `@inject` possono essere usate per inserire diversi servizi.</span><span class="sxs-lookup"><span data-stu-id="509ef-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="509ef-166">Nell'esempio riportato di seguito viene illustrato come usare `@inject`.</span><span class="sxs-lookup"><span data-stu-id="509ef-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="509ef-167">Il servizio che implementa `Services.IDataAccess` viene inserito nelle proprietà del componente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="509ef-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="509ef-168">Si noti come Usa solo il codice di `IDataAccess` astrazione:</span><span class="sxs-lookup"><span data-stu-id="509ef-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="509ef-169">Internamente, la proprietà generata (`DataRepository`) è dotato di `InjectAttribute` attributo.</span><span class="sxs-lookup"><span data-stu-id="509ef-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="509ef-170">In genere, questo attributo non viene utilizzato direttamente.</span><span class="sxs-lookup"><span data-stu-id="509ef-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="509ef-171">Se è necessaria per i componenti di una classe base e inserito le proprietà sono necessari anche per la classe di base, `InjectAttribute` possono essere aggiunte manualmente:</span><span class="sxs-lookup"><span data-stu-id="509ef-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="509ef-172">In componenti derivati dalla classe di base, il `@inject` direttiva non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="509ef-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="509ef-173">Il `InjectAttribute` della classe di base è sufficiente:</span><span class="sxs-lookup"><span data-stu-id="509ef-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="509ef-174">Inserimento delle dipendenze in servizi</span><span class="sxs-lookup"><span data-stu-id="509ef-174">Dependency injection in services</span></span>

<span data-ttu-id="509ef-175">Servizi complessi potrebbero richiedere servizi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="509ef-175">Complex services might require additional services.</span></span> <span data-ttu-id="509ef-176">Nell'esempio precedente `DataAccess` potrebbe richiedere il `HttpClient` del servizio predefinito.</span><span class="sxs-lookup"><span data-stu-id="509ef-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="509ef-177">`@inject` o `InjectAttribute` non possono essere usati nei servizi.</span><span class="sxs-lookup"><span data-stu-id="509ef-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="509ef-178">*Inserimento del costruttore* devono quindi essere utilizzate.</span><span class="sxs-lookup"><span data-stu-id="509ef-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="509ef-179">I servizi necessari vengono aggiunti tramite l'aggiunta di parametri al costruttore del servizio.</span><span class="sxs-lookup"><span data-stu-id="509ef-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="509ef-180">Quando l'inserimento delle dipendenze consente di creare il servizio, riconosce i servizi richiede nel costruttore e consente loro di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="509ef-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="509ef-181">Esempio di codice seguente viene illustrato il concetto:</span><span class="sxs-lookup"><span data-stu-id="509ef-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="509ef-182">Tenere presente i seguenti prerequisiti per l'inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="509ef-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="509ef-183">Deve essere presente un costruttore cui argomenti possono essere tutti specificati tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="509ef-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="509ef-184">Si noti che i parametri aggiuntivi non coperti da inserimento delle dipendenze sono consentiti se sono specificati i valori predefiniti per tali.</span><span class="sxs-lookup"><span data-stu-id="509ef-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="509ef-185">Il costruttore applicabile deve essere *pubblica*.</span><span class="sxs-lookup"><span data-stu-id="509ef-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="509ef-186">Deve essere presente solo un costruttore applicabile.</span><span class="sxs-lookup"><span data-stu-id="509ef-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="509ef-187">In caso di ambiguità, l'inserimento delle dipendenze genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="509ef-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="509ef-188">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="509ef-188">Additional resources</span></span>

* [<span data-ttu-id="509ef-189">Inserimento delle dipendenze in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="509ef-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
