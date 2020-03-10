---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Convalida con un livello di servizioC#() | Microsoft Docs
author: StephenWalther
description: Informazioni su come spostare la logica di convalida dalle azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542861"
---
# <a name="validating-with-a-service-layer-c"></a><span data-ttu-id="f42e1-104">Convalida con un livello di servizio (C#)</span><span class="sxs-lookup"><span data-stu-id="f42e1-104">Validating with a Service Layer (C#)</span></span>

<span data-ttu-id="f42e1-105">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f42e1-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f42e1-106">Informazioni su come spostare la logica di convalida dalle azioni del controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="f42e1-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="f42e1-107">In questa esercitazione, Stephen Walther spiega come è possibile mantenere una netta separazione delle problematiche isolando il livello di servizio dal livello del controller.</span><span class="sxs-lookup"><span data-stu-id="f42e1-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>

<span data-ttu-id="f42e1-108">L'obiettivo di questa esercitazione è descrivere un metodo per eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f42e1-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f42e1-109">In questa esercitazione si apprenderà come spostare la logica di convalida dai controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="f42e1-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="f42e1-110">Separazione delle problematiche</span><span class="sxs-lookup"><span data-stu-id="f42e1-110">Separating Concerns</span></span>

<span data-ttu-id="f42e1-111">Quando si compila un'applicazione MVC ASP.NET, è consigliabile non inserire la logica del database all'interno delle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="f42e1-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="f42e1-112">La combinazione della logica del database e del controller rende più difficile la gestione dell'applicazione nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="f42e1-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="f42e1-113">Si consiglia di inserire tutta la logica del database in un livello di repository separato.</span><span class="sxs-lookup"><span data-stu-id="f42e1-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="f42e1-114">Il listato 1, ad esempio, contiene un repository semplice denominato ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="f42e1-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="f42e1-115">Il repository del prodotto contiene tutto il codice di accesso ai dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f42e1-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="f42e1-116">L'elenco include anche l'interfaccia IProductRepository implementata dal repository del prodotto.</span><span class="sxs-lookup"><span data-stu-id="f42e1-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="f42e1-117">**Listato 1--Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="f42e1-118">Il controller nel listato 2 usa il livello di repository nelle azioni index () e create ().</span><span class="sxs-lookup"><span data-stu-id="f42e1-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="f42e1-119">Si noti che questo controller non contiene alcuna logica di database.</span><span class="sxs-lookup"><span data-stu-id="f42e1-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="f42e1-120">La creazione di un livello di repository consente di mantenere una netta separazione delle problematiche.</span><span class="sxs-lookup"><span data-stu-id="f42e1-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="f42e1-121">I controller sono responsabili della logica di controllo del flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="f42e1-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="f42e1-122">**Listato 2-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="f42e1-123">Creazione di un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="f42e1-123">Creating a Service Layer</span></span>

<span data-ttu-id="f42e1-124">Quindi, la logica di controllo del flusso dell'applicazione appartiene a un controller e la logica di accesso ai dati appartiene a un repository.</span><span class="sxs-lookup"><span data-stu-id="f42e1-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="f42e1-125">In tal caso, dove si inserisce la logica di convalida?</span><span class="sxs-lookup"><span data-stu-id="f42e1-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="f42e1-126">Un'opzione consiste nell'inserire la logica di convalida in un *livello di servizio*.</span><span class="sxs-lookup"><span data-stu-id="f42e1-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="f42e1-127">Un livello di servizio è un livello aggiuntivo in un'applicazione MVC ASP.NET che media la comunicazione tra un controller e un livello di repository.</span><span class="sxs-lookup"><span data-stu-id="f42e1-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="f42e1-128">Il livello di servizio contiene la logica di business.</span><span class="sxs-lookup"><span data-stu-id="f42e1-128">The service layer contains business logic.</span></span> <span data-ttu-id="f42e1-129">In particolare, contiene la logica di convalida.</span><span class="sxs-lookup"><span data-stu-id="f42e1-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="f42e1-130">Ad esempio, il livello del servizio prodotto nel listato 3 ha un metodo CreateProduct ().</span><span class="sxs-lookup"><span data-stu-id="f42e1-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="f42e1-131">Il metodo CreateProduct () chiama il metodo ValidateProduct () per convalidare un nuovo prodotto prima di passare il prodotto al repository del prodotto.</span><span class="sxs-lookup"><span data-stu-id="f42e1-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="f42e1-132">**Listato 3-Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="f42e1-133">Il controller di prodotto è stato aggiornato nel listato 4 per usare il livello di servizio anziché il livello del repository.</span><span class="sxs-lookup"><span data-stu-id="f42e1-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="f42e1-134">Il livello controller comunica con il livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="f42e1-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="f42e1-135">Il livello di servizio comunica con il livello di repository.</span><span class="sxs-lookup"><span data-stu-id="f42e1-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="f42e1-136">Ogni livello ha una responsabilità separata.</span><span class="sxs-lookup"><span data-stu-id="f42e1-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="f42e1-137">**Listato 4-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="f42e1-138">Si noti che il servizio prodotto viene creato nel costruttore del controller del prodotto.</span><span class="sxs-lookup"><span data-stu-id="f42e1-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="f42e1-139">Quando viene creato il servizio prodotto, il dizionario di stato del modello viene passato al servizio.</span><span class="sxs-lookup"><span data-stu-id="f42e1-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="f42e1-140">Il servizio prodotto usa lo stato del modello per passare nuovamente i messaggi di errore di convalida al controller.</span><span class="sxs-lookup"><span data-stu-id="f42e1-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="f42e1-141">Separazione del livello di servizio</span><span class="sxs-lookup"><span data-stu-id="f42e1-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="f42e1-142">Non è stato possibile isolare i livelli del controller e del servizio con un solo aspetto.</span><span class="sxs-lookup"><span data-stu-id="f42e1-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="f42e1-143">I livelli del controller e del servizio comunicano tramite lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="f42e1-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="f42e1-144">In altre parole, il livello di servizio presenta una dipendenza da una particolare funzionalità del framework MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f42e1-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="f42e1-145">Si vuole isolare il più possibile il livello di servizio dal livello di controller.</span><span class="sxs-lookup"><span data-stu-id="f42e1-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="f42e1-146">In teoria, dovrebbe essere possibile usare il livello di servizio con qualsiasi tipo di applicazione e non solo un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f42e1-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="f42e1-147">In futuro, ad esempio, potrebbe essere necessario creare un front-end WPF per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f42e1-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="f42e1-148">Si dovrebbe trovare un modo per rimuovere la dipendenza dallo stato del modello MVC ASP.NET dal livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="f42e1-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="f42e1-149">Nel listato 5, il livello di servizio è stato aggiornato in modo da non utilizzare più lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="f42e1-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="f42e1-150">USA invece qualsiasi classe che implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="f42e1-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="f42e1-151">**Listato 5-Models\ProductService.cs (disaccoppiato)**</span><span class="sxs-lookup"><span data-stu-id="f42e1-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="f42e1-152">L'interfaccia IValidationDictionary è definita nel listato 6.</span><span class="sxs-lookup"><span data-stu-id="f42e1-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="f42e1-153">Questa semplice interfaccia ha un solo metodo e una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="f42e1-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="f42e1-154">**Elenco 6-Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="f42e1-155">La classe nel listato 7, denominata la classe ModelStateWrapper, implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="f42e1-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="f42e1-156">È possibile creare un'istanza della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.</span><span class="sxs-lookup"><span data-stu-id="f42e1-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="f42e1-157">**Listato 7-Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="f42e1-158">Infine, il controller aggiornato nel listato 8 USA ModelStateWrapper durante la creazione del livello di servizio nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="f42e1-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="f42e1-159">**Listato 8-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f42e1-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="f42e1-160">L'uso dell'interfaccia IValidationDictionary e della classe ModelStateWrapper ci consente di isolare completamente il livello di servizio dal livello del controller.</span><span class="sxs-lookup"><span data-stu-id="f42e1-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="f42e1-161">Il livello di servizio non dipende più dallo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="f42e1-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="f42e1-162">È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary al livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="f42e1-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="f42e1-163">Ad esempio, un'applicazione WPF può implementare l'interfaccia IValidationDictionary con una classe Collection semplice.</span><span class="sxs-lookup"><span data-stu-id="f42e1-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="f42e1-164">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f42e1-164">Summary</span></span>

<span data-ttu-id="f42e1-165">L'obiettivo di questa esercitazione è illustrare un approccio per eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f42e1-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f42e1-166">In questa esercitazione si è appreso come spostare tutta la logica di convalida dai controller e a un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="f42e1-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="f42e1-167">Si è inoltre appreso come isolare il livello di servizio dal livello controller creando una classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="f42e1-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f42e1-168">[Precedente](validating-with-the-idataerrorinfo-interface-cs.md)
> [Successivo](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f42e1-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
