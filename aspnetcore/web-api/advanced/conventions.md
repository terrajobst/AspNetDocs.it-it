---
title: Usare le convenzioni dell'API Web
author: pranavkm
description: Informazioni sulle convenzioni dell'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 5ae96b213a19464045e1d0b1a76f8eb81089dc5b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029938"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="e75b3-103">Usare le convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="e75b3-103">Use web API conventions</span></span>

<span data-ttu-id="e75b3-104">Di [Pranav Krishnamoorthy](https://github.com/pranavkm) e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e75b3-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e75b3-105">ASP.NET Core 2.2 e versioni successive include una modalità per l'estrazione di [documentazione dell'API](xref:tutorials/web-api-help-pages-using-swagger) comune e l'applicazione della documentazione a più azioni o controller, oppure a tutti i controller inclusi in un assembly.</span><span class="sxs-lookup"><span data-stu-id="e75b3-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="e75b3-106">Le convenzioni dell'API Web sostituiscono l'aggiunta di [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) alle singole azioni.</span><span class="sxs-lookup"><span data-stu-id="e75b3-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="e75b3-107">Una convenzione consente di:</span><span class="sxs-lookup"><span data-stu-id="e75b3-107">A convention allows you to:</span></span>

* <span data-ttu-id="e75b3-108">Definire i tipi restituiti più comuni e i codici di stato restituiti da un tipo di azione specifico.</span><span class="sxs-lookup"><span data-stu-id="e75b3-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="e75b3-109">Identificare le azioni che deviano dallo standard definito.</span><span class="sxs-lookup"><span data-stu-id="e75b3-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="e75b3-110">ASP.NET Core MVC 2.2 e versioni successive include un set di convenzioni predefinite in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="e75b3-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="e75b3-111">Le convenzioni sono basate sul controller (*ValuesController.cs*) disponibile nel modello di progetto **API** di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e75b3-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="e75b3-112">Se si seguono i criteri del modello, le convenzioni predefinite danno i risultati previsti.</span><span class="sxs-lookup"><span data-stu-id="e75b3-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="e75b3-113">Se tuttavia le convenzioni predefinite non soddisfano le proprie esigenze, vedere [Creare convenzioni dell'API Web](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="e75b3-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="e75b3-114">In fase di runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> riconosce le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="e75b3-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="e75b3-115">`ApiExplorer` è l'astrazione MVC per comunicare con i generatori di documenti [OpenAPI](https://www.openapis.org/) (noti anche come Swagger).</span><span class="sxs-lookup"><span data-stu-id="e75b3-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="e75b3-116">Gli attributi della convenzione applicata vengono associati a un'azione e sono inclusi nella documentazione OpenAPI dell'azione.</span><span class="sxs-lookup"><span data-stu-id="e75b3-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="e75b3-117">Anche gli [analizzatori di API](xref:web-api/advanced/analyzers) supportano le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="e75b3-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="e75b3-118">Se l'azione non è convenzionale (ad esempio, se restituisce un codice di stato non documentato dalla convenzione applicata), un avviso richiede di documentare il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="e75b3-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="e75b3-119">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e75b3-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="e75b3-120">Applicare le convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="e75b3-120">Apply web API conventions</span></span>

<span data-ttu-id="e75b3-121">Le convenzioni non sono componibili; ogni azione può essere associata a una sola convenzione.</span><span class="sxs-lookup"><span data-stu-id="e75b3-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="e75b3-122">Le convenzioni più specifiche hanno la precedenza su quelle meno specifiche.</span><span class="sxs-lookup"><span data-stu-id="e75b3-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="e75b3-123">Quando si applicano a un'azione due o più convenzioni con la stessa priorità, la selezione è non deterministica.</span><span class="sxs-lookup"><span data-stu-id="e75b3-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="e75b3-124">Per applicare una convenzione a un'azione sono disponibili le opzioni seguenti, dalla più specifica alla meno specifica:</span><span class="sxs-lookup"><span data-stu-id="e75b3-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="e75b3-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; È valida per azioni singole, specifica il tipo di convenzione e il metodo della convenzione applicato.</span><span class="sxs-lookup"><span data-stu-id="e75b3-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="e75b3-126">Nell'esempio seguente, il metodo della convenzione `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` del tipo di convenzione predefinito viene applicato all'azione `Update`:</span><span class="sxs-lookup"><span data-stu-id="e75b3-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="e75b3-127">Il metodo della convenzione `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` applica i seguenti attributi all'azione:</span><span class="sxs-lookup"><span data-stu-id="e75b3-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

<span data-ttu-id="e75b3-128">Per altre informazioni su `[ProducesDefaultResponseType]`, vedere [Default Response](https://swagger.io/docs/specification/describing-responses/#default) (Risposta predefinita).</span><span class="sxs-lookup"><span data-stu-id="e75b3-128">For more information on `[ProducesDefaultResponseType]`, see [Default Response](https://swagger.io/docs/specification/describing-responses/#default).</span></span>

1. <span data-ttu-id="e75b3-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un controller &mdash; Applica il tipo di convenzione specificato a tutte le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="e75b3-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="e75b3-130">Un metodo della convenzione è completato da hint che determinano le azioni alle quali si applica il metodo della convenzione.</span><span class="sxs-lookup"><span data-stu-id="e75b3-130">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="e75b3-131">Per altre informazioni sugli hint, vedere [Creare convenzioni dell'API Web](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="e75b3-131">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="e75b3-132">Nell'esempio seguente il set di convenzioni predefinito viene applicato a tutte le azioni in *ContactsConventionController*:</span><span class="sxs-lookup"><span data-stu-id="e75b3-132">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="e75b3-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un assembly &mdash; Applica il tipo di convenzione specificato a tutti i controller dell'assembly corrente.</span><span class="sxs-lookup"><span data-stu-id="e75b3-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="e75b3-134">È consigliabile applicare attributi a livello di assembly nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e75b3-134">As a recommendation, apply assembly-level attributes in the *Startup.cs* file.</span></span>

    <span data-ttu-id="e75b3-135">Nell'esempio seguente il set di convenzioni predefinito viene applicato a tutti i controller dell'assembly:</span><span class="sxs-lookup"><span data-stu-id="e75b3-135">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="e75b3-136">Creare convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="e75b3-136">Create web API conventions</span></span>

<span data-ttu-id="e75b3-137">Se le convenzioni dell'API predefinite non soddisfano le proprie esigenze, creare convenzioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="e75b3-137">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="e75b3-138">Una convenzione è:</span><span class="sxs-lookup"><span data-stu-id="e75b3-138">A convention is:</span></span>

* <span data-ttu-id="e75b3-139">un tipo statico con metodi.</span><span class="sxs-lookup"><span data-stu-id="e75b3-139">A static type with methods.</span></span>
* <span data-ttu-id="e75b3-140">in grado di definire [tipi di risposta](#response-types) e [requisiti di denominazione](#naming-requirements) per le azioni.</span><span class="sxs-lookup"><span data-stu-id="e75b3-140">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="e75b3-141">Tipi di risposta</span><span class="sxs-lookup"><span data-stu-id="e75b3-141">Response types</span></span>

<span data-ttu-id="e75b3-142">Questi metodi vengono annotati con attributi `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="e75b3-142">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="e75b3-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e75b3-143">For example:</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="e75b3-144">Se non sono presenti attributi dei metadati più specifici, l'applicazione di questa convenzione a un assembly impone quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e75b3-144">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="e75b3-145">Il metodo della convenzione viene applicato a qualsiasi azione con nome `Find`.</span><span class="sxs-lookup"><span data-stu-id="e75b3-145">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="e75b3-146">Un parametro con nome `id` è presente nell'azione `Find`.</span><span class="sxs-lookup"><span data-stu-id="e75b3-146">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="e75b3-147">Requisiti di denominazione</span><span class="sxs-lookup"><span data-stu-id="e75b3-147">Naming requirements</span></span>

<span data-ttu-id="e75b3-148">Gli attributi `[ApiConventionNameMatch]` e `[ApiConventionTypeMatch]` possono essere applicati al metodo convenzione che determina le azioni nelle quali vengono implementati.</span><span class="sxs-lookup"><span data-stu-id="e75b3-148">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="e75b3-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e75b3-149">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="e75b3-150">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="e75b3-150">In the preceding example:</span></span>

* <span data-ttu-id="e75b3-151">L'opzione `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` applicata al metodo indica che la convenzione corrisponde a qualsiasi azione con il prefisso "Find".</span><span class="sxs-lookup"><span data-stu-id="e75b3-151">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="e75b3-152">`Find`, `FindPet` e `FindById` sono esempi di azioni corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="e75b3-152">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="e75b3-153">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applicato al parametro indica che la convenzione corrisponde a metodi che hanno esattamente un parametro che termina con l'identificatore del suffisso.</span><span class="sxs-lookup"><span data-stu-id="e75b3-153">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="e75b3-154">Sono esempi parametri come `id` o `petId`.</span><span class="sxs-lookup"><span data-stu-id="e75b3-154">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="e75b3-155">`ApiConventionTypeMatch` può essere applicato in modo analogo ai tipi, per vincolare il tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="e75b3-155">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="e75b3-156">Un argomento `params[]` indica i parametri rimanenti, per i quali non è necessario che sia rilevata una corrispondenza esplicita.</span><span class="sxs-lookup"><span data-stu-id="e75b3-156">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e75b3-157">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e75b3-157">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>