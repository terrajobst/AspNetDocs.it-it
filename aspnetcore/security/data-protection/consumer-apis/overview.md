---
title: Panoramica delle API consumer per ASP.NET Core
author: rick-anderson
description: Ricevere una breve panoramica del consumer di varie API disponibili all'interno della libreria di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: b0d11d097ee2d448b6781f6fa84445f6400fbc76
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024888"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a><span data-ttu-id="4aa62-103">Panoramica delle API consumer per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4aa62-103">Consumer APIs overview for ASP.NET Core</span></span>

<span data-ttu-id="4aa62-104">Il `IDataProtectionProvider` e `IDataProtector` interfacce sono le interfacce di base tramite cui i consumer utilizzano il sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="4aa62-104">The `IDataProtectionProvider` and `IDataProtector` interfaces are the basic interfaces through which consumers use the data protection system.</span></span> <span data-ttu-id="4aa62-105">Che si trovano nel [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4aa62-105">They're located in the [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) package.</span></span>

## <a name="idataprotectionprovider"></a><span data-ttu-id="4aa62-106">IDataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="4aa62-106">IDataProtectionProvider</span></span>

<span data-ttu-id="4aa62-107">L'interfaccia del provider rappresenta la radice del sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="4aa62-107">The provider interface represents the root of the data protection system.</span></span> <span data-ttu-id="4aa62-108">Non è direttamente utilizzabile per proteggere o rimuovere la protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="4aa62-108">It cannot directly be used to protect or unprotect data.</span></span> <span data-ttu-id="4aa62-109">Al contrario, il consumer deve ottenere un riferimento a un `IDataProtector` chiamando `IDataProtectionProvider.CreateProtector(purpose)`, in cui scopo è una stringa che descrive il caso d'uso previsto consumer.</span><span class="sxs-lookup"><span data-stu-id="4aa62-109">Instead, the consumer must get a reference to an `IDataProtector` by calling `IDataProtectionProvider.CreateProtector(purpose)`, where purpose is a string that describes the intended consumer use case.</span></span> <span data-ttu-id="4aa62-110">Visualizzare [stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings) per molte più informazioni all'obiettivo di questo parametro e come scegliere un valore appropriato.</span><span class="sxs-lookup"><span data-stu-id="4aa62-110">See [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings) for much more information on the intent of this parameter and how to choose an appropriate value.</span></span>

## <a name="idataprotector"></a><span data-ttu-id="4aa62-111">IDataProtector</span><span class="sxs-lookup"><span data-stu-id="4aa62-111">IDataProtector</span></span>

<span data-ttu-id="4aa62-112">L'interfaccia di protezione viene restituito da una chiamata a `CreateProtector`ed è questa interfaccia che gli utenti possono usare per eseguire proteggere e rimuovere la protezione di operazioni.</span><span class="sxs-lookup"><span data-stu-id="4aa62-112">The protector interface is returned by a call to `CreateProtector`, and it's this interface which consumers can use to perform protect and unprotect operations.</span></span>

<span data-ttu-id="4aa62-113">Per proteggere una porzione di dati, passare i dati per il `Protect` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4aa62-113">To protect a piece of data, pass the data to the `Protect` method.</span></span> <span data-ttu-id="4aa62-114">L'interfaccia di base definisce un metodo che converte byte [] -> byte [], ma è presente anche un overload (fornito come metodo di estensione) che lo converte in stringa -> stringa.</span><span class="sxs-lookup"><span data-stu-id="4aa62-114">The basic interface defines a method which converts byte[] -> byte[], but there's also an overload (provided as an extension method) which converts string -> string.</span></span> <span data-ttu-id="4aa62-115">La sicurezza offerta da due metodi è identica. lo sviluppatore deve scegliere qualsiasi overload è più semplice per i casi d'uso.</span><span class="sxs-lookup"><span data-stu-id="4aa62-115">The security offered by the two methods is identical; the developer should choose whichever overload is most convenient for their use case.</span></span> <span data-ttu-id="4aa62-116">Indipendentemente dall'overload scelto, il valore restituito da Protect metodo è ora protetto (cifrati e prova di manomissione) e l'applicazione può inviare a un client non attendibile.</span><span class="sxs-lookup"><span data-stu-id="4aa62-116">Irrespective of the overload chosen, the value returned by the Protect method is now protected (enciphered and tamper-proofed), and the application can send it to an untrusted client.</span></span>

<span data-ttu-id="4aa62-117">Per disattivare la protezione una porzione di dati protetti in precedenza, passare i dati protetti per il `Unprotect` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4aa62-117">To unprotect a previously-protected piece of data, pass the protected data to the `Unprotect` method.</span></span> <span data-ttu-id="4aa62-118">(Sono presenti byte []-overload in base e basate su stringhe per comodità dello sviluppatore.) Se il payload protetto è stato generato da una precedente chiamata a `Protect` questa stessa `IDataProtector`, il `Unprotect` metodo restituirà il payload non protetto originale.</span><span class="sxs-lookup"><span data-stu-id="4aa62-118">(There are byte[]-based and string-based overloads for developer convenience.) If the protected payload was generated by an earlier call to `Protect` on this same `IDataProtector`, the `Unprotect` method will return the original unprotected payload.</span></span> <span data-ttu-id="4aa62-119">Se il payload protetto è stato alterato o è stato generato da un'altra `IDataProtector`, il `Unprotect` metodo genererà CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="4aa62-119">If the protected payload has been tampered with or was produced by a different `IDataProtector`, the `Unprotect` method will throw CryptographicException.</span></span>

<span data-ttu-id="4aa62-120">Il concetto di uguale e diversi `IDataProtector` ties indietro al concetto di utilizzo generico.</span><span class="sxs-lookup"><span data-stu-id="4aa62-120">The concept of same vs. different `IDataProtector` ties back to the concept of purpose.</span></span> <span data-ttu-id="4aa62-121">Se due `IDataProtector` istanze sono state generate dalla stessa radice `IDataProtectionProvider` ma tramite stringhe di scopi diversi nella chiamata a `IDataProtectionProvider.CreateProtector`, quindi considerate [protezioni diversi](xref:security/data-protection/consumer-apis/purpose-strings), e uno non saranno in grado di rimuovere la protezione payload generati dagli altri.</span><span class="sxs-lookup"><span data-stu-id="4aa62-121">If two `IDataProtector` instances were generated from the same root `IDataProtectionProvider` but via different purpose strings in the call to `IDataProtectionProvider.CreateProtector`, then they're considered [different protectors](xref:security/data-protection/consumer-apis/purpose-strings), and one won't be able to unprotect payloads generated by the other.</span></span>

## <a name="consuming-these-interfaces"></a><span data-ttu-id="4aa62-122">Utilizzo di queste interfacce</span><span class="sxs-lookup"><span data-stu-id="4aa62-122">Consuming these interfaces</span></span>

<span data-ttu-id="4aa62-123">Per un componente compatibile con l'inserimento delle dipendenze, l'utilizzo previsto è che il componente di richiedere un `IDataProtectionProvider` parametro nel relativo costruttore e che il sistema di inserimento delle dipendenze offre questo servizio automaticamente quando viene creata un'istanza al componente.</span><span class="sxs-lookup"><span data-stu-id="4aa62-123">For a DI-aware component, the intended usage is that the component take an `IDataProtectionProvider` parameter in its constructor and that the DI system automatically provides this service when the component is instantiated.</span></span>

> [!NOTE]
> <span data-ttu-id="4aa62-124">Alcune applicazioni (ad esempio applicazioni console o applicazioni ASP.NET 4.x) potrebbero non essere l'inserimento delle dipendenze in grado di riconoscere in modo che non è possibile utilizzare il meccanismo descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="4aa62-124">Some applications (such as console applications or ASP.NET 4.x applications) might not be DI-aware so cannot use the mechanism described here.</span></span> <span data-ttu-id="4aa62-125">Per questi scenari, consultare il [scenari Non compatibili con](xref:security/data-protection/configuration/non-di-scenarios) documento per altre informazioni su come ottenere un'istanza di un `IDataProtection` provider senza passare attraverso l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4aa62-125">For these scenarios consult the [Non DI Aware Scenarios](xref:security/data-protection/configuration/non-di-scenarios) document for more information on getting an instance of an `IDataProtection` provider without going through DI.</span></span>

<span data-ttu-id="4aa62-126">L'esempio seguente illustra tre concetti:</span><span class="sxs-lookup"><span data-stu-id="4aa62-126">The following sample demonstrates three concepts:</span></span>

1. <span data-ttu-id="4aa62-127">[Aggiungere il sistema di protezione dati](xref:security/data-protection/configuration/overview) al contenitore del servizio,</span><span class="sxs-lookup"><span data-stu-id="4aa62-127">[Add the data protection system](xref:security/data-protection/configuration/overview) to the service container,</span></span>

2. <span data-ttu-id="4aa62-128">Tramite l'inserimento delle dipendenze per la ricezione di un'istanza di un `IDataProtectionProvider`, e</span><span class="sxs-lookup"><span data-stu-id="4aa62-128">Using DI to receive an instance of an `IDataProtectionProvider`, and</span></span>

3. <span data-ttu-id="4aa62-129">Creazione di un' `IDataProtector` da un `IDataProtectionProvider` e usandolo per proteggere e rimuovere la protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="4aa62-129">Creating an `IDataProtector` from an `IDataProtectionProvider` and using it to protect and unprotect data.</span></span>

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="4aa62-130">Il pacchetto Microsoft.AspNetCore.DataProtection.Abstractions contiene un metodo di estensione `IServiceProvider.GetDataProtector` comodità per gli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="4aa62-130">The package Microsoft.AspNetCore.DataProtection.Abstractions contains an extension method `IServiceProvider.GetDataProtector` as a developer convenience.</span></span> <span data-ttu-id="4aa62-131">Incapsula un'unica operazione entrambi il recupero di un `IDataProtectionProvider` dal provider del servizio e chiamare il metodo `IDataProtectionProvider.CreateProtector`.</span><span class="sxs-lookup"><span data-stu-id="4aa62-131">It encapsulates as a single operation both retrieving an `IDataProtectionProvider` from the service provider and calling `IDataProtectionProvider.CreateProtector`.</span></span> <span data-ttu-id="4aa62-132">L'esempio seguente viene illustrato l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="4aa62-132">The following sample demonstrates its usage.</span></span>

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> <span data-ttu-id="4aa62-133">Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="4aa62-133">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="4aa62-134">È destinato che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata verso `CreateProtector`, che fanno riferimento a verranno utilizzate per più chiamate a `Protect` e `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="4aa62-134">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span> <span data-ttu-id="4aa62-135">Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto non può essere verificato o decifrato.</span><span class="sxs-lookup"><span data-stu-id="4aa62-135">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="4aa62-136">Alcuni componenti può essere utile ignorare gli errori durante rimuovere la protezione di operazioni; un componente che legge i cookie di autenticazione può gestire questo errore e trattare la richiesta come se non avesse alcun cookie affatto anziché Nega la richiesta direttamente.</span><span class="sxs-lookup"><span data-stu-id="4aa62-136">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="4aa62-137">I componenti che questo comportamento devono in particolare intercettare CryptographicException invece di assorbimento di tutte le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="4aa62-137">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>