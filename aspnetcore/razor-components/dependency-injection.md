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
# <a name="razor-components-dependency-injection"></a>Inserimento di dipendenze componenti Razor

Da [Stropek Rainer](https://www.timecockpit.com)

[Inserimento di dipendenze ()](/aspnet/core/fundamentals/dependency-injection) è incorporato. Le app possono usare i servizi incorporati inserendoli nei componenti. Le app possono anche definire servizi personalizzati e renderli disponibili tramite l'inserimento delle dipendenze.

## <a name="dependency-injection"></a>Inserimento di dipendenze

L'inserimento delle dipendenze è una tecnica per l'accesso ai servizi configurati in una posizione centrale. Ciò può risultare utile per:

* Condividere una singola istanza di una classe di servizio tra molti componenti (noto come un *singleton* servizio).
* Separare i componenti dalle classi di servizio concreta specifico e fare riferimento solo a astrazioni. Ad esempio, un'interfaccia `IDataAccess` viene implementata da una classe concreta `DataAccess`. Quando un componente Usa l'inserimento delle dipendenze per ricevere un `IDataAccess` implementazione, il componente non è associato al tipo concreto. L'implementazione è possibile scambiare, forse a un'implementazione fittizia negli unit test.

Il sistema di inserimento delle dipendenze è responsabile della fornitura delle istanze dei servizi per i componenti. L'inserimento delle dipendenze risolve inoltre le dipendenze in modo ricorsivo in modo che i servizi stessi possono dipendono dai servizi ulteriormente. L'inserimento delle dipendenze viene configurato durante l'avvio dell'app. Un esempio è illustrato più avanti in questo argomento.

## <a name="add-services-to-di"></a>Aggiungere servizi per l'inserimento delle dipendenze

Dopo aver creato una nuova app, esaminare il `Startup.ConfigureServices` metodo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Il `ConfigureServices` viene passato un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), ovvero un elenco di oggetti descrittore di servizio ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). I servizi vengono aggiunti, fornendo i descrittori di servizio per l'insieme al servizio. Esempio di codice seguente viene illustrato il concetto:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Servizi possono essere configurati con le durate seguenti:

| Metodo      | Descrizione |
| ----------- | ----------- |
| [Singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | L'inserimento delle dipendenze consente di creare un *singola istanza* del servizio. Tutti i componenti che richiedono questo servizio ricevono un riferimento a questa istanza. |
| [Temporaneo](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Ogni volta che un componente richiede questo servizio, riceve un *nuova istanza* del servizio. |
| [Con ambito](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | Blazor lato client non ha attualmente il concetto di ambiti di inserimento delle dipendenze. `Scoped` si comporta come `Singleton`. Tuttavia, i componenti di ASP.NET Core Razor supporta il `Scoped` durata. In un componente di Razor, la registrazione di un servizio con ambito ha come ambita la connessione. Per questo motivo, è preferibile per i servizi che devono avere come ambiti l'utente corrente usando servizi con ambiti (anche se si intende corrente per l'esecuzione del client nel browser). |

Il sistema di inserimento delle dipendenze si basa sul sistema di inserimento delle dipendenze in ASP.NET Core. Per altre informazioni, vedere [inserimento delle dipendenze in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Servizi predefiniti

I servizi predefiniti vengono aggiunti automaticamente alla raccolta del servizio di un'app. La tabella seguente illustra alcuni dei servizi predefiniti utili forniti.

| Metodo       | Descrizione |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Fornisce metodi per inviare richieste HTTP e ricevere risposte HTTP da una risorsa identificata da un URI (singleton). Si noti che questa istanza di `HttpClient` Usa il browser per la gestione del traffico HTTP in background. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) viene impostata automaticamente per il prefisso URI di base dell'app. `HttpClient` viene fornito solo per le app Blazor lato client. |
| `IJSRuntime` | Rappresenta un'istanza di un runtime JavaScript a cui possono essere inviate chiamate. Per altre informazioni, vedere <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Helper per l'uso di URI e l'esplorazione dello stato (singleton). `IUriHelper` viene fornito per entrambe le app ASP.NET Core Razor componenti e Blazor di lato client. |

Si noti che è possibile usare un provider di servizi personalizzati anziché il provider del servizio predefinito che viene aggiunto per il modello predefinito. Un provider di servizi personalizzati non fornisce automaticamente i servizi predefiniti elencati nella tabella. Tali servizi devono essere aggiunto in modo esplicito per nuovo provider del servizio.

## <a name="request-a-service-in-a-component"></a>Richiesta di un servizio in un componente

Dopo che i servizi vengono aggiunti alla raccolta di servizio, può essere inseriti all'interno di modelli Razor dei componenti utilizzando il `@inject` direttiva Razor. `@inject` presenta due parametri:

* Nome del tipo: Il tipo del servizio da inserire.
* Nome proprietà: Il nome della proprietà la ricezione del servizio app inserito. Si noti che la proprietà non richiede la creazione manuale. Il compilatore crea la proprietà.

Più `@inject` possono essere usate per inserire diversi servizi.

Nell'esempio riportato di seguito viene illustrato come usare `@inject`. Il servizio che implementa `Services.IDataAccess` viene inserito nelle proprietà del componente `DataRepository`. Si noti come Usa solo il codice di `IDataAccess` astrazione:

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

Internamente, la proprietà generata (`DataRepository`) è dotato di `InjectAttribute` attributo. In genere, questo attributo non viene utilizzato direttamente. Se è necessaria per i componenti di una classe base e inserito le proprietà sono necessari anche per la classe di base, `InjectAttribute` possono essere aggiunte manualmente:

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

In componenti derivati dalla classe di base, il `@inject` direttiva non è obbligatorio. Il `InjectAttribute` della classe di base è sufficiente:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Inserimento delle dipendenze in servizi

Servizi complessi potrebbero richiedere servizi aggiuntivi. Nell'esempio precedente `DataAccess` potrebbe richiedere il `HttpClient` del servizio predefinito. `@inject` o `InjectAttribute` non possono essere usati nei servizi. *Inserimento del costruttore* devono quindi essere utilizzate. I servizi necessari vengono aggiunti tramite l'aggiunta di parametri al costruttore del servizio. Quando l'inserimento delle dipendenze consente di creare il servizio, riconosce i servizi richiede nel costruttore e consente loro di conseguenza.

Esempio di codice seguente viene illustrato il concetto:

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

Tenere presente i seguenti prerequisiti per l'inserimento del costruttore:

* Deve essere presente un costruttore cui argomenti possono essere tutti specificati tramite inserimento delle dipendenze. Si noti che i parametri aggiuntivi non coperti da inserimento delle dipendenze sono consentiti se sono specificati i valori predefiniti per tali.
* Il costruttore applicabile deve essere *pubblica*.
* Deve essere presente solo un costruttore applicabile. In caso di ambiguità, l'inserimento delle dipendenze genera un'eccezione.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Inserimento delle dipendenze in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
