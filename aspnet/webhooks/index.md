---
uid: webhooks/index
title: Panoramica di Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduzione ai webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075086"
---
# <a name="aspnet-webhooks-overview"></a>Panoramica di Webhook ASP.NET

Webhook è un modello HTTP leggero che fornisce un semplice modello di pubblicazione/sottoscrizione per collegare le API Web e i servizi SaaS. Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di richiesta HTTP POST ai sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che consentono al ricevitore di agire di conseguenza.

Grazie alla loro semplicità, i webhook sono già esposti da un numero elevato di servizi, tra cui [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [striping](http://www.stripe.com), [Trello](http://www.trello.com/)e molti altri. Ad esempio, un webhook può indicare che un file è stato modificato in [Dropbox](http://dropbox.com/)oppure è stato eseguito il commit di una modifica del codice in GitHub oppure è stato avviato un pagamento in [PayPal](http://www.paypal.com/)oppure è stata creata una scheda in [Trello](http://www.trello.com/). Le possibilità sono infinite!

Microsoft ASP.NET webhook rende più semplice inviare e ricevere webhook come parte dell'applicazione ASP.NET:

* Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione dei webhook da un numero qualsiasi di provider di webhook. Con supporto per [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[Wordpress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) , ma è facile aggiungere il supporto per altre informazioni.

* Sul lato di invio, fornisce il supporto per la gestione e l'archiviazione delle sottoscrizioni, nonché per l'invio di notifiche degli eventi al set corretto di sottoscrittori. In questo modo è possibile definire un set di eventi personalizzato che i sottoscrittori possono sottoscrivere e inviare notifiche quando si verificano problemi.

Le due parti possono essere utilizzate insieme o separatamente a seconda dello scenario. Se è necessario ricevere webhook solo da altri servizi, è possibile usare solo la parte Receiver; Se si vuole esporre i webhook solo per l'utilizzo da altri utenti, è possibile eseguire questa operazione.

Il codice ha come destinazione API Web ASP.NET 2 e ASP.NET MVC 5 ed è disponibile come [OSS su GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Panoramica di Webhook

Webhook è un modello che significa che varia in base al modo in cui viene usato dal servizio al servizio, ma l'idea di base è la stessa. È possibile considerare i webhook come un semplice modello di pubblicazione/sottoscrizione in cui un utente può sottoscrivere gli eventi che si verificano altrove. Le notifiche degli eventi vengono propagate come richieste HTTP POST contenenti informazioni sull'evento stesso.

La richiesta HTTP POST contiene in genere un oggetto JSON o dati del modulo HTML determinati dal mittente del webhook, incluse le informazioni sull'evento che ha causato l'attivazione del webhook. Ad esempio, il corpo della richiesta POST di un webhook da [GitHub](https://www.github.com/) è simile al seguente in seguito all'apertura di un nuovo problema in un repository particolare:

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

Per assicurarsi che il webhook sia effettivamente dal mittente previsto, la richiesta POST viene protetta in qualche modo e quindi verificata dal ricevitore. Ad esempio, i [webhook GitHub](https://developer.github.com/webhooks/) includono un'intestazione HTTP *X-Hub-Signature* con un hash del corpo della richiesta controllato dall'implementazione del ricevitore, quindi non è necessario preoccuparsi.

Il flusso del webhook viene in genere eseguito come segue:

* Il mittente del webhook espone gli eventi che un client può sottoscrivere. Gli eventi descrivono le modifiche osservabili del sistema, ad esempio se è stato inserito un nuovo elemento di dati, che un processo è stato completato o altro.

* Il ricevitore del webhook sottoscrive registrando un webhook costituito da quattro elementi:

     1. URI per il quale deve essere inviata la notifica degli eventi sotto forma di richiesta HTTP POST.

     2. Un set di filtri che descrivono gli eventi specifici per i quali deve essere generato il webhook;

     3. Chiave privata usata per firmare la richiesta HTTP POST;

     4. Dati aggiuntivi che devono essere inclusi nella richiesta HTTP POST. Questo può ad esempio essere proprietà o campi di intestazione HTTP aggiuntivi inclusi nel corpo della richiesta HTTP POST.

* Quando si verifica un evento, vengono trovate le registrazioni dei webhook corrispondenti e vengono inviate richieste HTTP POST. In genere, la generazione delle richieste POST HTTP viene ripetuta più volte se per qualche motivo il destinatario non risponde o la richiesta HTTP POST restituisce una risposta di errore.

## <a name="webhooks-processing-pipeline"></a>Pipeline di elaborazione webhook

La pipeline di elaborazione dei webhook Microsoft ASP.NET per i webhook in ingresso è simile alla seguente:

![Pipeline di elaborazione webhook ASP.NET](_static/WebHookReceivers.png)

I due concetti principali sono i *ricevitori* e i *gestori*:

* I *ricevitori* sono responsabili della gestione della particolare versione del webhook da un determinato mittente e dell'applicazione dei controlli di sicurezza per garantire che la richiesta di Webhook sia effettivamente dal mittente previsto.

* I *gestori* sono in genere dove il codice utente esegue l'elaborazione del webhook specifico.

Nei nodi seguenti questi concetti sono descritti in maggiore dettaglio.
