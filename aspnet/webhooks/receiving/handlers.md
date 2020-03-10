---
uid: webhooks/receiving/handlers
title: Gestori webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Come gestire le richieste nei webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637872"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="dcf0a-103">Gestori webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dcf0a-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="dcf0a-104">Una volta convalidata da un ricevitore di Webhook, le richieste webhook sono pronte per essere elaborate dal codice utente.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="dcf0a-105">Ecco dove entrano i *gestori* .</span><span class="sxs-lookup"><span data-stu-id="dcf0a-105">This is where *handlers* come in.</span></span> <span data-ttu-id="dcf0a-106">I gestori derivano dall'interfaccia [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , ma in genere usano la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) anziché derivare direttamente dall'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="dcf0a-107">Una richiesta di Webhook può essere elaborata da uno o più gestori.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="dcf0a-108">I gestori vengono chiamati in ordine in base alla rispettiva proprietà *Order* passando dal più basso al più alto, dove Order è un numero intero semplice (consigliato per essere compreso tra 1 e 100):</span><span class="sxs-lookup"><span data-stu-id="dcf0a-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramma proprietà ordine gestore webhook](_static/Handlers.png)

<span data-ttu-id="dcf0a-110">Un gestore può facoltativamente impostare la proprietà della *risposta* in WebHookHandlerContext che comporterà l'arresto dell'elaborazione e la risposta da inviare come risposta HTTP al webhook.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="dcf0a-111">Nel caso precedente, il gestore C non verrà chiamato perché ha un ordine superiore a B e B imposta la risposta.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="dcf0a-112">L'impostazione della risposta è in genere rilevante solo per i webhook in cui la risposta può portare le informazioni all'API di origine.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="dcf0a-113">Questo è ad esempio il caso dei webhook Slack in cui viene eseguito il postback della risposta al canale in cui è stato creato il webhook.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="dcf0a-114">I gestori possono impostare la proprietà Receiver se vogliono solo ricevere webhook da tale ricevitore specifico.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="dcf0a-115">Se il ricevitore non viene impostato, viene chiamato per tutti.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="dcf0a-116">Un altro uso comune di una risposta consiste nell'usare una risposta di *410* per indicare che il webhook non è più attivo e non devono essere inviate altre richieste.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="dcf0a-117">Per impostazione predefinita, un gestore viene chiamato da tutti i ricevitori di webhook.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="dcf0a-118">Tuttavia, se la proprietà *Receiver* è impostata sul nome di un gestore, il gestore riceverà solo le richieste di Webhook dal ricevitore.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="dcf0a-119">Elaborazione di un webhook</span><span class="sxs-lookup"><span data-stu-id="dcf0a-119">Processing a WebHook</span></span>

<span data-ttu-id="dcf0a-120">Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni sulla richiesta del webhook.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="dcf0a-121">I dati, in genere il corpo della richiesta HTTP, sono disponibili dalla proprietà *Data* .</span><span class="sxs-lookup"><span data-stu-id="dcf0a-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="dcf0a-122">Il tipo di dati è in genere JSON o dati del modulo HTML, ma è possibile eseguire il cast a un tipo più specifico, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="dcf0a-123">Ad esempio, è possibile eseguire il cast dei webhook personalizzati generati da webhook ASP.NET al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dcf0a-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="dcf0a-124">Elaborazione in coda</span><span class="sxs-lookup"><span data-stu-id="dcf0a-124">Queued Processing</span></span>

<span data-ttu-id="dcf0a-125">La maggior parte dei mittenti del webhook invierà di nuovo un webhook se una risposta non viene generata entro pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="dcf0a-126">Ciò significa che il gestore deve completare l'elaborazione all'interno di tale intervallo di tempo, in modo da non richiamarlo.</span><span class="sxs-lookup"><span data-stu-id="dcf0a-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="dcf0a-127">Se l'elaborazione richiede più tempo o è più gestita separatamente, è possibile usare [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) per inviare la richiesta di Webhook a una coda, ad esempio [coda di archiviazione di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="dcf0a-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="dcf0a-128">Un contorno di un'implementazione di [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) è disponibile qui:</span><span class="sxs-lookup"><span data-stu-id="dcf0a-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
