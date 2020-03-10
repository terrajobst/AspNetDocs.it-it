---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestori di messaggi HttpClient in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Creazione di gestori di messaggi personalizzati per API Web ASP.NET in ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557645"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="f7351-103">Gestori di messaggi HttpClient in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7351-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="f7351-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f7351-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f7351-105">Un *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta http.</span><span class="sxs-lookup"><span data-stu-id="f7351-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="f7351-106">In genere, una serie di gestori di messaggi viene concatenata.</span><span class="sxs-lookup"><span data-stu-id="f7351-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="f7351-107">Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e assegna la richiesta al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="f7351-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="f7351-108">A un certo punto, viene creata la risposta e viene eseguito il backup della catena.</span><span class="sxs-lookup"><span data-stu-id="f7351-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="f7351-109">Questo modello viene definito gestore *delegato* .</span><span class="sxs-lookup"><span data-stu-id="f7351-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="f7351-110">Sul lato client, la classe **HttpClient** usa un gestore di messaggi per elaborare le richieste.</span><span class="sxs-lookup"><span data-stu-id="f7351-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="f7351-111">Il gestore predefinito è **HttpClientHandler**, che invia la richiesta in rete e ottiene la risposta dal server.</span><span class="sxs-lookup"><span data-stu-id="f7351-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="f7351-112">È possibile inserire gestori di messaggi personalizzati nella pipeline client:</span><span class="sxs-lookup"><span data-stu-id="f7351-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="f7351-113">API Web ASP.NET inoltre utilizza i gestori di messaggi sul lato server.</span><span class="sxs-lookup"><span data-stu-id="f7351-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="f7351-114">Per altre informazioni, vedere [gestori di messaggi http](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="f7351-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="f7351-115">Gestori di messaggi personalizzati</span><span class="sxs-lookup"><span data-stu-id="f7351-115">Custom Message Handlers</span></span>

<span data-ttu-id="f7351-116">Per scrivere un gestore di messaggi personalizzato, derivare da **System .NET. http. DelegatingHandler** ed eseguire l'override del metodo **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="f7351-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="f7351-117">Ecco la firma del metodo:</span><span class="sxs-lookup"><span data-stu-id="f7351-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="f7351-118">Il metodo accetta un **HttpRequestMessage** come input e restituisce un **HttpResponseMessage**in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="f7351-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="f7351-119">Un'implementazione tipica esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f7351-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="f7351-120">Elaborare il messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7351-120">Process the request message.</span></span>
2. <span data-ttu-id="f7351-121">Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.</span><span class="sxs-lookup"><span data-stu-id="f7351-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="f7351-122">Il gestore interno restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f7351-122">The inner handler returns a response message.</span></span> <span data-ttu-id="f7351-123">(Questo passaggio è asincrono).</span><span class="sxs-lookup"><span data-stu-id="f7351-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="f7351-124">Elaborare la risposta e restituirla al chiamante.</span><span class="sxs-lookup"><span data-stu-id="f7351-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="f7351-125">Nell'esempio seguente viene illustrato un gestore di messaggi che aggiunge un'intestazione personalizzata alla richiesta in uscita:</span><span class="sxs-lookup"><span data-stu-id="f7351-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="f7351-126">La chiamata a `base.SendAsync` è asincrona.</span><span class="sxs-lookup"><span data-stu-id="f7351-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="f7351-127">Se il gestore esegue operazioni dopo questa chiamata, usare la parola chiave **await** per riprendere l'esecuzione dopo il completamento del metodo.</span><span class="sxs-lookup"><span data-stu-id="f7351-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="f7351-128">Nell'esempio seguente viene illustrato un gestore che registra i codici di errore.</span><span class="sxs-lookup"><span data-stu-id="f7351-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="f7351-129">La registrazione non è molto interessante, ma nell'esempio viene illustrato come ottenere la risposta all'interno del gestore.</span><span class="sxs-lookup"><span data-stu-id="f7351-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="f7351-130">Aggiunta di gestori di messaggi alla pipeline client</span><span class="sxs-lookup"><span data-stu-id="f7351-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="f7351-131">Per aggiungere gestori personalizzati a **HttpClient**, usare il metodo **HttpClientFactory. Create** :</span><span class="sxs-lookup"><span data-stu-id="f7351-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="f7351-132">I gestori di messaggi vengono chiamati nell'ordine in cui vengono passati al metodo **create** .</span><span class="sxs-lookup"><span data-stu-id="f7351-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="f7351-133">Poiché i gestori sono annidati, il messaggio di risposta viaggia nell'altra direzione.</span><span class="sxs-lookup"><span data-stu-id="f7351-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="f7351-134">Ovvero l'ultimo gestore è il primo a ottenere il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f7351-134">That is, the last handler is the first to get the response message.</span></span>
