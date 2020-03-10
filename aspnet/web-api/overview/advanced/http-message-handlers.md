---
uid: web-api/overview/advanced/http-message-handlers
title: Gestori di messaggi HTTP in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Panoramica dei gestori di messaggi HTTP in API Web ASP.NET per ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622584"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a>Gestori di messaggi HTTP in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Un *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta http. I gestori di messaggi derivano dalla classe **HttpMessageHandler** astratta.

In genere, una serie di gestori di messaggi viene concatenata. Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e assegna la richiesta al gestore successivo. A un certo punto, viene creata la risposta e viene eseguito il backup della catena. Questo modello viene definito gestore *delegato* .

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Gestori di messaggi sul lato server

Sul lato server, la pipeline dell'API Web usa alcuni gestori di messaggi predefiniti:

- **HttpServer** ottiene la richiesta dall'host.
- **HttpRoutingDispatcher** Invia la richiesta in base alla route.
- **HttpControllerDispatcher** Invia la richiesta a un controller API Web.

È possibile aggiungere gestori personalizzati alla pipeline. I gestori di messaggi sono ideali per le problematiche trasversali che operano al livello dei messaggi HTTP (anziché le azioni del controller). Un gestore di messaggi, ad esempio, potrebbe:

- Leggere o modificare le intestazioni della richiesta.
- Aggiungere un'intestazione di risposta alle risposte.
- Convalidare le richieste prima che raggiungano il controller.

Questo diagramma mostra due gestori personalizzati inseriti nella pipeline:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Sul lato client, HttpClient usa anche i gestori di messaggi. Per altre informazioni, vedere [gestori di messaggi HttpClient](httpclient-message-handlers.md).

## <a name="custom-message-handlers"></a>Gestori di messaggi personalizzati

Per scrivere un gestore di messaggi personalizzato, derivare da **System .NET. http. DelegatingHandler** ed eseguire l'override del metodo **SendAsync** . Questo metodo ha la firma seguente:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Il metodo accetta un **HttpRequestMessage** come input e restituisce un **HttpResponseMessage**in modo asincrono. Un'implementazione tipica esegue le operazioni seguenti:

1. Elaborare il messaggio di richiesta.
2. Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.
3. Il gestore interno restituisce un messaggio di risposta. (Questo passaggio è asincrono).
4. Elaborare la risposta e restituirla al chiamante.

Di seguito è riportato un esempio semplice:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> La chiamata a `base.SendAsync` è asincrona. Se il gestore esegue operazioni dopo questa chiamata, usare la parola chiave **await** , come illustrato.

Un gestore delegato può anche ignorare il gestore interno e creare direttamente la risposta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Se un gestore di delega crea la risposta senza chiamare `base.SendAsync`, la richiesta ignora il resto della pipeline. Questa operazione può essere utile per un gestore che convalida la richiesta (creando una risposta di errore).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Aggiunta di un gestore alla pipeline

Per aggiungere un gestore di messaggi sul lato server, aggiungere il gestore alla raccolta **HttpConfiguration. MessageHandlers** . Se è stato usato il modello "applicazione Web MVC 4 ASP.NET" per creare il progetto, è possibile eseguire questa operazione all'interno della classe **WebApiConfig** :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

I gestori di messaggi vengono chiamati nello stesso ordine in cui vengono visualizzati nella raccolta **MessageHandlers** . Poiché sono annidati, il messaggio di risposta si sposta nell'altra direzione. Ovvero l'ultimo gestore è il primo a ottenere il messaggio di risposta.

Si noti che non è necessario impostare i gestori interni; il Framework API Web connette automaticamente i gestori di messaggi.

Se si esegue l' [hosting automatico](../older-versions/self-host-a-web-api.md), creare un'istanza della classe **HttpSelfHostConfiguration** e aggiungere i gestori alla raccolta **MessageHandlers** .

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Verranno ora esaminati alcuni esempi di gestori di messaggi personalizzati.

## <a name="example-x-http-method-override"></a>Esempio: X-HTTP-method-override

X-HTTP-method-override è un'intestazione HTTP non standard. È progettata per i client che non possono inviare determinati tipi di richiesta HTTP, ad esempio PUT o DELETE. Al contrario, il client invia una richiesta POST e imposta l'intestazione X-HTTP-method-override sul metodo desiderato. Esempio:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Di seguito è riportato un gestore di messaggi che aggiunge il supporto per X-HTTP-method-override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

Nel metodo **SendAsync** , il gestore verifica se il messaggio di richiesta è una richiesta post e se contiene l'intestazione X-http-method-override. In tal caso, convalida il valore dell'intestazione e quindi modifica il metodo di richiesta. Infine, il gestore chiama `base.SendAsync` per passare il messaggio al gestore successivo.

Quando la richiesta raggiunge la classe **HttpControllerDispatcher** , **HttpControllerDispatcher** instraderà la richiesta in base al metodo di richiesta aggiornato.

## <a name="example-adding-a-custom-response-header"></a>Esempio: aggiunta di un'intestazione di risposta personalizzata

Di seguito è riportato un gestore di messaggi che aggiunge un'intestazione personalizzata a ogni messaggio di risposta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Il gestore chiama innanzitutto `base.SendAsync` per passare la richiesta al gestore messaggi interno. Il gestore interno restituisce un messaggio di risposta, ma esegue questa operazione in modo asincrono utilizzando un' **attività&lt;t&gt;** oggetto. Il messaggio di risposta non è disponibile finché il `base.SendAsync` non viene completato in modo asincrono.

Questo esempio usa la parola chiave **await** per eseguire il lavoro in modo asincrono dopo il completamento di `SendAsync`. Se la destinazione è .NET Framework 4,0, usare l' **attività**&lt;t&gt; **. Metodo ContinueWith** :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Esempio: verifica della presenza di una chiave API

Alcuni servizi Web richiedono che i client includano una chiave API nella richiesta. Nell'esempio seguente viene illustrato come un gestore di messaggi può verificare le richieste di una chiave API valida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Questo gestore cerca la chiave API nella stringa di query URI. Per questo esempio si presuppone che la chiave sia una stringa statica. Un'implementazione reale può probabilmente usare una convalida più complessa. Se la stringa di query contiene la chiave, il gestore passa la richiesta al gestore interno.

Se la richiesta non dispone di una chiave valida, il gestore crea un messaggio di risposta con stato 403, accesso negato. In questo caso, il gestore non chiama `base.SendAsync`, quindi il gestore interno non riceve mai la richiesta né il controller. Il controller può pertanto presupporre che tutte le richieste in ingresso dispongano di una chiave API valida.

> [!NOTE]
> Se la chiave API si applica solo a determinate azioni del controller, è consigliabile utilizzare un filtro azioni anziché un gestore di messaggi. I filtri azione vengono eseguiti dopo l'esecuzione del routing dell'URI.

## <a name="per-route-message-handlers"></a>Gestori di messaggi per Route

I gestori nella raccolta **HttpConfiguration. MessageHandlers** si applicano a livello globale.

In alternativa, è possibile aggiungere un gestore di messaggi a una route specifica quando si definisce la route:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

In questo esempio, se l'URI della richiesta corrisponde a "Route2", la richiesta viene inviata al `MessageHandler2`. Il diagramma seguente illustra la pipeline per queste due route:

![](http-message-handlers/_static/image4.png)

Si noti che `MessageHandler2` sostituisce il valore predefinito **HttpControllerDispatcher**. In questo esempio `MessageHandler2` crea la risposta e le richieste che corrispondono a "Route2" non vengono mai indirizzate a un controller. Ciò consente di sostituire l'intero meccanismo del controller API Web con un endpoint personalizzato.

In alternativa, un gestore di messaggi per route può delegare a **HttpControllerDispatcher**, che quindi invia a un controller.

![](http-message-handlers/_static/image5.png)

Il codice seguente illustra come configurare questa route:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
