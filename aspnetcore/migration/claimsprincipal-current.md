---
title: Eseguire la migrazione da claimsprincipal. Current
author: mjrousos
description: Informazioni su come eseguire la migrazione da claimsprincipal. Current per recuperare l'identità dell'utente autenticato corrente e le attestazioni in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 35c3389798041e141c45bf0a76fa9d7285212768
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039278"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="ec108-103">Eseguire la migrazione da claimsprincipal. Current</span><span class="sxs-lookup"><span data-stu-id="ec108-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="ec108-104">In progetti ASP.NET 4.x, era pratica comune usare [claimsprincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) per recuperare l'oggetto corrente autenticato identità e le attestazioni dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ec108-104">In ASP.NET 4.x projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="ec108-105">In ASP.NET Core, questa proprietà non è impostata non è più.</span><span class="sxs-lookup"><span data-stu-id="ec108-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="ec108-106">Il codice che è stata che dipendono da essa deve essere aggiornato per ottenere l'identità dell'utente autenticato corrente tramite modi diversi.</span><span class="sxs-lookup"><span data-stu-id="ec108-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="ec108-107">Dati specifici del contesto anziché i dati statici</span><span class="sxs-lookup"><span data-stu-id="ec108-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="ec108-108">Quando si usa ASP.NET Core, i valori di entrambi `ClaimsPrincipal.Current` e `Thread.CurrentPrincipal` non sono impostati.</span><span class="sxs-lookup"><span data-stu-id="ec108-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="ec108-109">Entrambe le proprietà rappresentano lo stato statico, evitando così di ASP.NET Core a livello generale.</span><span class="sxs-lookup"><span data-stu-id="ec108-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="ec108-110">È invece architettura ASP.NET Core per recuperare le dipendenze (ad esempio, l'identità dell'utente corrente) da raccolte specifici per il contesto del servizio (con relativo [inserimento delle dipendenze](xref:fundamentals/dependency-injection) modello (inserimento delle dipendenze)).</span><span class="sxs-lookup"><span data-stu-id="ec108-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="ec108-111">What ' s, ulteriori `Thread.CurrentPrincipal` è statico, thread e pertanto potrebbe non conservare le modifiche in alcuni scenari asincroni (e `ClaimsPrincipal.Current` chiama semplicemente `Thread.CurrentPrincipal` per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="ec108-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="ec108-112">Per comprendere gli ordinamenti dei thread dei problemi possono causare i membri statici in scenari asincroni, si consideri il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ec108-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

<span data-ttu-id="ec108-113">L'esempio di codice precedente imposta `Thread.CurrentPrincipal` e viene controllato il valore prima e dopo l'attesa di una chiamata asincrona.</span><span class="sxs-lookup"><span data-stu-id="ec108-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="ec108-114">`Thread.CurrentPrincipal` è specifico per il *thread* in cui è impostata e il metodo è probabile che riprendere l'esecuzione in un thread diverso dopo l'attesa.</span><span class="sxs-lookup"><span data-stu-id="ec108-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="ec108-115">Di conseguenza `Thread.CurrentPrincipal` è presente quando viene dapprima controllata ma è null dopo la chiamata a `await Task.Yield()`.</span><span class="sxs-lookup"><span data-stu-id="ec108-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="ec108-116">Introduzione all'identità dell'utente corrente dalla raccolta di servizi di inserimento delle dipendenze dell'app è più testabili, troppo, poiché le identità di test possono essere inserite facilmente.</span><span class="sxs-lookup"><span data-stu-id="ec108-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="ec108-117">Recuperare l'utente corrente in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec108-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="ec108-118">Sono disponibili diverse opzioni per il recupero dell'utente autenticato corrente `ClaimsPrincipal` in ASP.NET Core al posto di `ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="ec108-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="ec108-119">**ControllerBase.User**.</span><span class="sxs-lookup"><span data-stu-id="ec108-119">**ControllerBase.User**.</span></span> <span data-ttu-id="ec108-120">Controller MVC possono accedere l'utente autenticato corrente con loro [utente](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) proprietà.</span><span class="sxs-lookup"><span data-stu-id="ec108-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="ec108-121">**HttpContext.User**.</span><span class="sxs-lookup"><span data-stu-id="ec108-121">**HttpContext.User**.</span></span> <span data-ttu-id="ec108-122">I componenti con accesso all'oggetto corrente `HttpContext` (middleware, ad esempio) è possibile ottenere l'utente corrente `ClaimsPrincipal` dalla [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span><span class="sxs-lookup"><span data-stu-id="ec108-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="ec108-123">**Passato dal chiamante**.</span><span class="sxs-lookup"><span data-stu-id="ec108-123">**Passed in from caller**.</span></span> <span data-ttu-id="ec108-124">Librerie senza accesso all'attuale `HttpContext` sono spesso chiamati dal controller o i componenti middleware e può avere identità dell'utente corrente passato come argomento.</span><span class="sxs-lookup"><span data-stu-id="ec108-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="ec108-125">**IHttpContextAccessor**.</span><span class="sxs-lookup"><span data-stu-id="ec108-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="ec108-126">Il progetto in fase di migrazione ad ASP.NET Core potrebbe essere troppo grande per essere facilmente passare identità dell'utente corrente per tutte le posizioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="ec108-126">The project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="ec108-127">In questi casi [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) può essere utilizzato come soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="ec108-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="ec108-128">`IHttpContextAccessor` è in grado di accedere a corrente `HttpContext` (se presente).</span><span class="sxs-lookup"><span data-stu-id="ec108-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="ec108-129">Una soluzione a breve termine per il recupero di identità dell'utente corrente nel codice che non è ancora stato aggiornato per funzionare con architettura basata sull'inserimento delle dipendenze di ASP.NET Core sarebbe:</span><span class="sxs-lookup"><span data-stu-id="ec108-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="ec108-130">Rendere `IHttpContextAccessor` disponibile nel contenitore di inserimento delle dipendenze chiamando [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ec108-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="ec108-131">Ottenere un'istanza di `IHttpContextAccessor` durante l'avvio e archiviarlo in una variabile statica.</span><span class="sxs-lookup"><span data-stu-id="ec108-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="ec108-132">L'istanza viene resa disponibile al codice che in precedenza il recupero dell'utente corrente da una proprietà statica.</span><span class="sxs-lookup"><span data-stu-id="ec108-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="ec108-133">Recuperare l'utente corrente `ClaimsPrincipal` usando `HttpContextAccessor.HttpContext?.User`.</span><span class="sxs-lookup"><span data-stu-id="ec108-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="ec108-134">Se questo codice viene usato all'esterno del contesto di una richiesta HTTP, il `HttpContext` è null.</span><span class="sxs-lookup"><span data-stu-id="ec108-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="ec108-135">Ultima opzione, tramite `IHttpContextAccessor`, è opposto a quanto definito dai principi di ASP.NET Core (preferendo inserito le dipendenze delle dipendenze statico).</span><span class="sxs-lookup"><span data-stu-id="ec108-135">The final option, using `IHttpContextAccessor`, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="ec108-136">Prevede di rimuovere, eventualmente, la dipendenza da statico `IHttpContextAccessor` helper.</span><span class="sxs-lookup"><span data-stu-id="ec108-136">Plan to eventually remove the dependency on the static `IHttpContextAccessor` helper.</span></span> <span data-ttu-id="ec108-137">Può essere un bridge utile, tuttavia, durante la migrazione di grandi dimensioni ASP.NET le app esistenti che in precedenza usava `ClaimsPrincipal.Current`.</span><span class="sxs-lookup"><span data-stu-id="ec108-137">It can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>