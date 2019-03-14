---
title: Gerarchia di scopi e multi-tenancy in ASP.NET Core
author: rick-anderson
description: Informazioni sulla gerarchia di stringhe di scopi e multi-tenancy in relazione a ASP.NET Core Data Protection API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047008"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a><span data-ttu-id="77e62-103">Gerarchia di scopi e multi-tenancy in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77e62-103">Purpose hierarchy and multi-tenancy in ASP.NET Core</span></span>

<span data-ttu-id="77e62-104">Poiché un' `IDataProtector` è inoltre in modo implicito un `IDataProtectionProvider`, a scopo può essere concatenato.</span><span class="sxs-lookup"><span data-stu-id="77e62-104">Since an `IDataProtector` is also implicitly an `IDataProtectionProvider`, purposes can be chained together.</span></span> <span data-ttu-id="77e62-105">In questo senso `provider.CreateProtector([ "purpose1", "purpose2" ])` equivale a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span><span class="sxs-lookup"><span data-stu-id="77e62-105">In this sense, `provider.CreateProtector([ "purpose1", "purpose2" ])` is equivalent to `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span></span>

<span data-ttu-id="77e62-106">In questo modo per alcune relazioni gerarchiche interessanti tramite il sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="77e62-106">This allows for some interesting hierarchical relationships through the data protection system.</span></span> <span data-ttu-id="77e62-107">Nell'esempio precedente di [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), può chiamare il componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` iniziale una sola volta e memorizzare nella cache il risultato in una privata `_myProvider` campo.</span><span class="sxs-lookup"><span data-stu-id="77e62-107">In the earlier example of [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), the SecureMessage component can call `provider.CreateProtector("Contoso.Messaging.SecureMessage")` once up-front and cache the result into a private `_myProvider` field.</span></span> <span data-ttu-id="77e62-108">Sarà quindi possibile creare protezioni futuri tramite chiamate a `_myProvider.CreateProtector("User: username")`, e queste protezioni vengono usate per la protezione dei singoli messaggi.</span><span class="sxs-lookup"><span data-stu-id="77e62-108">Future protectors can then be created via calls to `_myProvider.CreateProtector("User: username")`, and these protectors would be used for securing the individual messages.</span></span>

<span data-ttu-id="77e62-109">Ciò può anche essere invertita.</span><span class="sxs-lookup"><span data-stu-id="77e62-109">This can also be flipped.</span></span> <span data-ttu-id="77e62-110">Prendere in considerazione una singola applicazione logica che ospita più tenant (un sistema CMS sembra ragionevole) e ogni tenant può essere configurato con un proprio sistema di gestione dello stato e l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="77e62-110">Consider a single logical application which hosts multiple tenants (a CMS seems reasonable), and each tenant can be configured with its own authentication and state management system.</span></span> <span data-ttu-id="77e62-111">L'applicazione generico dispone di un singolo provider master e chiama `provider.CreateProtector("Tenant 1")` e `provider.CreateProtector("Tenant 2")` per fornire la propria sezione isolata del sistema di protezione dati di ogni tenant.</span><span class="sxs-lookup"><span data-stu-id="77e62-111">The umbrella application has a single master provider, and it calls `provider.CreateProtector("Tenant 1")` and `provider.CreateProtector("Tenant 2")` to give each tenant its own isolated slice of the data protection system.</span></span> <span data-ttu-id="77e62-112">I tenant è stato possibile derivare le proprie singoli programmi di protezione in base alle proprie esigenze, ma, indipendentemente dal livello provano non può creare protezioni con cui sono in conflitto con un altro tenant nel sistema.</span><span class="sxs-lookup"><span data-stu-id="77e62-112">The tenants could then derive their own individual protectors based on their own needs, but no matter how hard they try they cannot create protectors which collide with any other tenant in the system.</span></span> <span data-ttu-id="77e62-113">Graficamente, ciò è rappresentato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="77e62-113">Graphically, this is represented as below.</span></span>

![A scopo di tenancy più](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> <span data-ttu-id="77e62-115">Ciò presuppone l'ombrello controlli delle applicazioni quali API sono disponibili a singoli tenant e che i tenant non è possibile eseguire codice arbitrario nel server.</span><span class="sxs-lookup"><span data-stu-id="77e62-115">This assumes the umbrella application controls which APIs are available to individual tenants and that tenants cannot execute arbitrary code on the server.</span></span> <span data-ttu-id="77e62-116">Se un tenant può eseguire codice arbitrario, è stato possibile eseguire la reflection privata per interrompere le garanzie di isolamento oppure potrebbe semplicemente leggere direttamente il materiale della chiave master e derivano tutte le sottochiavi hanno l'esigenza di.</span><span class="sxs-lookup"><span data-stu-id="77e62-116">If a tenant can execute arbitrary code, they could perform private reflection to break the isolation guarantees, or they could just read the master keying material directly and derive whatever subkeys they desire.</span></span>

<span data-ttu-id="77e62-117">Il sistema di protezione dei dati utilizza effettivamente una sorta di multi-tenancy nella configurazione predefinita della finestra.</span><span class="sxs-lookup"><span data-stu-id="77e62-117">The data protection system actually uses a sort of multi-tenancy in its default out-of-the-box configuration.</span></span> <span data-ttu-id="77e62-118">Per impostazione predefinita materiale della chiave master viene archiviato nella cartella del profilo utente dell'account del processo di lavoro (o il Registro di sistema, per le identità pool di applicazioni IIS).</span><span class="sxs-lookup"><span data-stu-id="77e62-118">By default master keying material is stored in the worker process account's user profile folder (or the registry, for IIS application pool identities).</span></span> <span data-ttu-id="77e62-119">Ma è in realtà è abbastanza comune usare un singolo account per eseguire più applicazioni e pertanto tutte le applicazioni finirebbe materiale per le chiavi master di condivisione.</span><span class="sxs-lookup"><span data-stu-id="77e62-119">But it's actually fairly common to use a single account to run multiple applications, and thus all these applications would end up sharing the master keying material.</span></span> <span data-ttu-id="77e62-120">Per risolvere questo problema, il sistema di protezione dati inserisce automaticamente un identificatore univoco per ogni applicazione come primo elemento nella catena di scopo generale.</span><span class="sxs-lookup"><span data-stu-id="77e62-120">To solve this, the data protection system automatically inserts a unique-per-application identifier as the first element in the overall purpose chain.</span></span> <span data-ttu-id="77e62-121">Che consente di funzione implicita [isolare le singole applicazioni](xref:security/data-protection/configuration/overview#per-application-isolation) uno da altro, in modo efficace considerando ogni applicazione come un tenant all'interno del sistema e il processo di creazione di protezione univoco è identico all'immagine sopra.</span><span class="sxs-lookup"><span data-stu-id="77e62-121">This implicit purpose serves to [isolate individual applications](xref:security/data-protection/configuration/overview#per-application-isolation) from one another by effectively treating each application as a unique tenant within the system, and the protector creation process looks identical to the image above.</span></span>