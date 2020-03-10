---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Convalida del modello in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Panoramica della convalida del modello in API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557239"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="859d2-103">Convalida di modelli in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="859d2-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="859d2-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="859d2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="859d2-105">Questo articolo illustra come annotare i modelli, usare le annotazioni per la convalida dei dati e gestire gli errori di convalida nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="859d2-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="859d2-106">Quando un client invia dati all'API Web, spesso si vuole convalidare i dati prima di eseguire qualsiasi elaborazione.</span><span class="sxs-lookup"><span data-stu-id="859d2-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="859d2-107">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="859d2-107">Data Annotations</span></span>

<span data-ttu-id="859d2-108">In API Web ASP.NET, è possibile utilizzare gli attributi dello spazio dei nomi [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) per impostare le regole di convalida per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="859d2-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="859d2-109">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="859d2-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="859d2-110">Se è stata usata la convalida del modello in ASP.NET MVC, l'aspetto dovrebbe essere familiare.</span><span class="sxs-lookup"><span data-stu-id="859d2-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="859d2-111">L'attributo **required** indica che la proprietà `Name` non deve essere null.</span><span class="sxs-lookup"><span data-stu-id="859d2-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="859d2-112">L'attributo **Range** indica che `Weight` deve essere compreso tra 0 e 999.</span><span class="sxs-lookup"><span data-stu-id="859d2-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="859d2-113">Si supponga che un client invii una richiesta POST con la rappresentazione JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="859d2-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="859d2-114">È possibile notare che il client non include la proprietà `Name`, che è contrassegnata come obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="859d2-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="859d2-115">Quando l'API Web converte il codice JSON in un'istanza di `Product`, convalida l'`Product` rispetto agli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="859d2-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="859d2-116">Nell'azione del controller è possibile verificare se il modello è valido:</span><span class="sxs-lookup"><span data-stu-id="859d2-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="859d2-117">La convalida del modello non garantisce che i dati client siano sicuri.</span><span class="sxs-lookup"><span data-stu-id="859d2-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="859d2-118">Potrebbe essere necessaria una convalida aggiuntiva in altri livelli dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="859d2-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="859d2-119">Ad esempio, il livello dati potrebbe applicare vincoli di chiave esterna. L'esercitazione sull' [uso dell'API Web con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) esamina alcuni di questi problemi.</span><span class="sxs-lookup"><span data-stu-id="859d2-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="859d2-120">**"In**-posting": il sottoposting si verifica quando il client lascia alcune proprietà.</span><span class="sxs-lookup"><span data-stu-id="859d2-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="859d2-121">Si supponga, ad esempio, che il client invii quanto segue:</span><span class="sxs-lookup"><span data-stu-id="859d2-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="859d2-122">In questo caso, il client non ha specificato valori per `Price` o `Weight`.</span><span class="sxs-lookup"><span data-stu-id="859d2-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="859d2-123">Il formattatore JSON assegna un valore predefinito pari a zero alle proprietà mancanti.</span><span class="sxs-lookup"><span data-stu-id="859d2-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="859d2-124">Lo stato del modello è valido perché zero è un valore valido per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="859d2-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="859d2-125">Il fatto che si tratti di un problema dipende dallo scenario.</span><span class="sxs-lookup"><span data-stu-id="859d2-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="859d2-126">In un'operazione di aggiornamento, ad esempio, potrebbe essere necessario distinguere tra "zero" e "not set".</span><span class="sxs-lookup"><span data-stu-id="859d2-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="859d2-127">Per forzare i client a impostare un valore, rendere Nullable la proprietà e impostare l'attributo **required** :</span><span class="sxs-lookup"><span data-stu-id="859d2-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="859d2-128">**"Overposting"** : un client può anche inviare *più* dati del previsto.</span><span class="sxs-lookup"><span data-stu-id="859d2-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="859d2-129">Esempio:</span><span class="sxs-lookup"><span data-stu-id="859d2-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="859d2-130">In questo caso, il codice JSON include una proprietà ("color") che non esiste nel modello di `Product`.</span><span class="sxs-lookup"><span data-stu-id="859d2-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="859d2-131">In questo caso, il formattatore JSON ignora semplicemente questo valore.</span><span class="sxs-lookup"><span data-stu-id="859d2-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="859d2-132">Il formattatore XML esegue la stessa operazione. La sovrascrittura causa problemi se il modello dispone di proprietà che si desidera essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="859d2-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="859d2-133">Esempio:</span><span class="sxs-lookup"><span data-stu-id="859d2-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="859d2-134">Non si vuole che gli utenti aggiornino la proprietà `IsAdmin` e si elevano agli amministratori.</span><span class="sxs-lookup"><span data-stu-id="859d2-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="859d2-135">La strategia più sicura consiste nell'usare una classe modello che corrisponda esattamente a ciò che il client è autorizzato a inviare:</span><span class="sxs-lookup"><span data-stu-id="859d2-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="859d2-136">Il post di Blog di Brad Wilson "convalida dell'[input e convalida dei modelli in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" presenta una descrizione approfondita di sottoposta e overposting.</span><span class="sxs-lookup"><span data-stu-id="859d2-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="859d2-137">Sebbene il post sia relativo a ASP.NET MVC 2, i problemi sono ancora rilevanti per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="859d2-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="859d2-138">Gestione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="859d2-138">Handling Validation Errors</span></span>

<span data-ttu-id="859d2-139">L'API Web non restituisce automaticamente un errore al client quando la convalida ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="859d2-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="859d2-140">Spetta all'azione del controller controllare lo stato del modello e rispondere in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="859d2-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="859d2-141">È inoltre possibile creare un filtro azione per verificare lo stato del modello prima che venga richiamata l'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="859d2-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="859d2-142">Il codice seguente mostra un esempio:</span><span class="sxs-lookup"><span data-stu-id="859d2-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="859d2-143">Se la convalida del modello ha esito negativo, questo filtro restituisce una risposta HTTP che contiene gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="859d2-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="859d2-144">In tal caso, l'azione del controller non viene richiamata.</span><span class="sxs-lookup"><span data-stu-id="859d2-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="859d2-145">Per applicare questo filtro a tutti i controller API Web, aggiungere un'istanza del filtro alla raccolta **HttpConfiguration. filters** durante la configurazione:</span><span class="sxs-lookup"><span data-stu-id="859d2-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="859d2-146">Un'altra opzione consiste nell'impostare il filtro come attributo per singoli controller o azioni del controller:</span><span class="sxs-lookup"><span data-stu-id="859d2-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
