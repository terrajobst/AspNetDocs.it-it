---
uid: webhooks/index
title: Panoramica di Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduzione ai webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637291"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="89c4f-103">Panoramica di Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89c4f-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="89c4f-104">Webhook è un modello HTTP leggero che fornisce un semplice modello di pubblicazione/sottoscrizione per collegare le API Web e i servizi SaaS.</span><span class="sxs-lookup"><span data-stu-id="89c4f-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="89c4f-105">Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di richiesta HTTP POST ai sottoscrittori registrati.</span><span class="sxs-lookup"><span data-stu-id="89c4f-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="89c4f-106">La richiesta POST contiene informazioni sull'evento che consentono al ricevitore di agire di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="89c4f-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="89c4f-107">Grazie alla loro semplicità, i webhook sono già esposti da un numero elevato di servizi, tra cui [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [striping](http://www.stripe.com), [Trello](http://www.trello.com/)e molti altri.</span><span class="sxs-lookup"><span data-stu-id="89c4f-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="89c4f-108">Ad esempio, un webhook può indicare che un file è stato modificato in [Dropbox](http://dropbox.com/)oppure è stato eseguito il commit di una modifica del codice in GitHub oppure è stato avviato un pagamento in [PayPal](http://www.paypal.com/)oppure è stata creata una scheda in [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="89c4f-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="89c4f-109">Le possibilità sono infinite!</span><span class="sxs-lookup"><span data-stu-id="89c4f-109">The possibilities are endless!</span></span>

<span data-ttu-id="89c4f-110">Microsoft ASP.NET webhook rende più semplice inviare e ricevere webhook come parte dell'applicazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="89c4f-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="89c4f-111">Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione dei webhook da un numero qualsiasi di provider di webhook.</span><span class="sxs-lookup"><span data-stu-id="89c4f-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="89c4f-112">Con supporto per [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[Wordpress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) , ma è facile aggiungere il supporto per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="89c4f-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="89c4f-113">Sul lato di invio, fornisce il supporto per la gestione e l'archiviazione delle sottoscrizioni, nonché per l'invio di notifiche degli eventi al set corretto di sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="89c4f-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="89c4f-114">In questo modo è possibile definire un set di eventi personalizzato che i sottoscrittori possono sottoscrivere e inviare notifiche quando si verificano problemi.</span><span class="sxs-lookup"><span data-stu-id="89c4f-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="89c4f-115">Le due parti possono essere utilizzate insieme o separatamente a seconda dello scenario.</span><span class="sxs-lookup"><span data-stu-id="89c4f-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="89c4f-116">Se è necessario ricevere webhook solo da altri servizi, è possibile usare solo la parte Receiver; Se si vuole esporre i webhook solo per l'utilizzo da altri utenti, è possibile eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="89c4f-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="89c4f-117">Il codice ha come destinazione API Web ASP.NET 2 e ASP.NET MVC 5 ed è disponibile come [OSS su GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="89c4f-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="89c4f-118">Panoramica di Webhook</span><span class="sxs-lookup"><span data-stu-id="89c4f-118">WebHooks Overview</span></span>

<span data-ttu-id="89c4f-119">Webhook è un modello che significa che varia in base al modo in cui viene usato dal servizio al servizio, ma l'idea di base è la stessa.</span><span class="sxs-lookup"><span data-stu-id="89c4f-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="89c4f-120">È possibile considerare i webhook come un semplice modello di pubblicazione/sottoscrizione in cui un utente può sottoscrivere gli eventi che si verificano altrove.</span><span class="sxs-lookup"><span data-stu-id="89c4f-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="89c4f-121">Le notifiche degli eventi vengono propagate come richieste HTTP POST contenenti informazioni sull'evento stesso.</span><span class="sxs-lookup"><span data-stu-id="89c4f-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="89c4f-122">La richiesta HTTP POST contiene in genere un oggetto JSON o dati del modulo HTML determinati dal mittente del webhook, incluse le informazioni sull'evento che ha causato l'attivazione del webhook.</span><span class="sxs-lookup"><span data-stu-id="89c4f-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="89c4f-123">Ad esempio, il corpo della richiesta POST di un webhook da [GitHub](https://www.github.com/) è simile al seguente in seguito all'apertura di un nuovo problema in un repository particolare:</span><span class="sxs-lookup"><span data-stu-id="89c4f-123">For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="89c4f-124">Per assicurarsi che il webhook sia effettivamente dal mittente previsto, la richiesta POST viene protetta in qualche modo e quindi verificata dal ricevitore.</span><span class="sxs-lookup"><span data-stu-id="89c4f-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="89c4f-125">Ad esempio, i [webhook GitHub](https://developer.github.com/webhooks/) includono un'intestazione HTTP *X-Hub-Signature* con un hash del corpo della richiesta controllato dall'implementazione del ricevitore, quindi non è necessario preoccuparsi.</span><span class="sxs-lookup"><span data-stu-id="89c4f-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="89c4f-126">Il flusso del webhook viene in genere eseguito come segue:</span><span class="sxs-lookup"><span data-stu-id="89c4f-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="89c4f-127">Il mittente del webhook espone gli eventi che un client può sottoscrivere.</span><span class="sxs-lookup"><span data-stu-id="89c4f-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="89c4f-128">Gli eventi descrivono le modifiche osservabili del sistema, ad esempio se è stato inserito un nuovo elemento di dati, che un processo è stato completato o altro.</span><span class="sxs-lookup"><span data-stu-id="89c4f-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="89c4f-129">Il ricevitore del webhook sottoscrive registrando un webhook costituito da quattro elementi:</span><span class="sxs-lookup"><span data-stu-id="89c4f-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="89c4f-130">URI per il quale deve essere inviata la notifica degli eventi sotto forma di richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="89c4f-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="89c4f-131">Un set di filtri che descrivono gli eventi specifici per i quali deve essere generato il webhook;</span><span class="sxs-lookup"><span data-stu-id="89c4f-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="89c4f-132">Chiave privata usata per firmare la richiesta HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="89c4f-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="89c4f-133">Dati aggiuntivi che devono essere inclusi nella richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="89c4f-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="89c4f-134">Questo può ad esempio essere proprietà o campi di intestazione HTTP aggiuntivi inclusi nel corpo della richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="89c4f-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="89c4f-135">Quando si verifica un evento, vengono trovate le registrazioni dei webhook corrispondenti e vengono inviate richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="89c4f-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="89c4f-136">In genere, la generazione delle richieste POST HTTP viene ripetuta più volte se per qualche motivo il destinatario non risponde o la richiesta HTTP POST restituisce una risposta di errore.</span><span class="sxs-lookup"><span data-stu-id="89c4f-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="89c4f-137">Pipeline di elaborazione webhook</span><span class="sxs-lookup"><span data-stu-id="89c4f-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="89c4f-138">La pipeline di elaborazione dei webhook Microsoft ASP.NET per i webhook in ingresso è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="89c4f-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline di elaborazione webhook ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="89c4f-140">I due concetti principali sono i *ricevitori* e i *gestori*:</span><span class="sxs-lookup"><span data-stu-id="89c4f-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="89c4f-141">I *ricevitori* sono responsabili della gestione della particolare versione del webhook da un determinato mittente e dell'applicazione dei controlli di sicurezza per garantire che la richiesta di Webhook sia effettivamente dal mittente previsto.</span><span class="sxs-lookup"><span data-stu-id="89c4f-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="89c4f-142">I *gestori* sono in genere dove il codice utente esegue l'elaborazione del webhook specifico.</span><span class="sxs-lookup"><span data-stu-id="89c4f-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="89c4f-143">Nei nodi seguenti questi concetti sono descritti in maggiore dettaglio.</span><span class="sxs-lookup"><span data-stu-id="89c4f-143">In the following nodes these concepts are described in more details.</span></span>
