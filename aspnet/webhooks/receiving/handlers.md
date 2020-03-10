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
# <a name="aspnet-webhooks-handlers"></a>Gestori webhook ASP.NET

Una volta convalidata da un ricevitore di Webhook, le richieste webhook sono pronte per essere elaborate dal codice utente. Ecco dove entrano i *gestori* . I gestori derivano dall'interfaccia [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , ma in genere usano la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) anziché derivare direttamente dall'interfaccia.

Una richiesta di Webhook può essere elaborata da uno o più gestori. I gestori vengono chiamati in ordine in base alla rispettiva proprietà *Order* passando dal più basso al più alto, dove Order è un numero intero semplice (consigliato per essere compreso tra 1 e 100):

![Diagramma proprietà ordine gestore webhook](_static/Handlers.png)

Un gestore può facoltativamente impostare la proprietà della *risposta* in WebHookHandlerContext che comporterà l'arresto dell'elaborazione e la risposta da inviare come risposta HTTP al webhook. Nel caso precedente, il gestore C non verrà chiamato perché ha un ordine superiore a B e B imposta la risposta.

L'impostazione della risposta è in genere rilevante solo per i webhook in cui la risposta può portare le informazioni all'API di origine. Questo è ad esempio il caso dei webhook Slack in cui viene eseguito il postback della risposta al canale in cui è stato creato il webhook. I gestori possono impostare la proprietà Receiver se vogliono solo ricevere webhook da tale ricevitore specifico. Se il ricevitore non viene impostato, viene chiamato per tutti.

Un altro uso comune di una risposta consiste nell'usare una risposta di *410* per indicare che il webhook non è più attivo e non devono essere inviate altre richieste.

Per impostazione predefinita, un gestore viene chiamato da tutti i ricevitori di webhook. Tuttavia, se la proprietà *Receiver* è impostata sul nome di un gestore, il gestore riceverà solo le richieste di Webhook dal ricevitore.

## <a name="processing-a-webhook"></a>Elaborazione di un webhook

Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni sulla richiesta del webhook. I dati, in genere il corpo della richiesta HTTP, sono disponibili dalla proprietà *Data* .

Il tipo di dati è in genere JSON o dati del modulo HTML, ma è possibile eseguire il cast a un tipo più specifico, se lo si desidera. Ad esempio, è possibile eseguire il cast dei webhook personalizzati generati da webhook ASP.NET al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:

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

  ## <a name="queued-processing"></a>Elaborazione in coda

La maggior parte dei mittenti del webhook invierà di nuovo un webhook se una risposta non viene generata entro pochi secondi. Ciò significa che il gestore deve completare l'elaborazione all'interno di tale intervallo di tempo, in modo da non richiamarlo.

Se l'elaborazione richiede più tempo o è più gestita separatamente, è possibile usare [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) per inviare la richiesta di Webhook a una coda, ad esempio [coda di archiviazione di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Un contorno di un'implementazione di [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) è disponibile qui:

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
