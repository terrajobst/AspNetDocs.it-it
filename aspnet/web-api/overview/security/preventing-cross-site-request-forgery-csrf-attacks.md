---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevenzione degli attacchi di richiesta intersito falsa (CSRF) in ASP.NET MVC
author: MikeWasson
description: Descrive gli attacchi di richiesta intersito falsificazione (CSRF) e come implementare misure anti-CSRF in ASP.NET Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555118"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Prevenzione degli attacchi di richiesta intersito falsa (CSRF) nell'applicazione MVC ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

La richiesta tra siti falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è attualmente connesso

Di seguito è riportato un esempio di un attacco CSRF:

1. Un utente accede `www.example.com` usando l'autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza disconnettersi, l'utente visita un sito Web dannoso. Questo sito dannoso contiene il formato HTML seguente: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Si tratta della parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante Submit (Invia). Il browser include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita sul server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione consentita a un utente autenticato.

Sebbene questo esempio richieda all'utente di fare clic sul pulsante modulo, è possibile che la pagina dannosa esegua facilmente uno script che invia il modulo automaticamente. Inoltre, l'uso di SSL non impedisce un attacco CSRF, perché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili per i siti Web che utilizzano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito Web di destinazione. Gli attacchi CSRF, tuttavia, non sono limitati allo sfruttamento dei cookie. Ad esempio, anche l'autenticazione di base e del digest è vulnerabile. Dopo che un utente ha eseguito l'accesso con l'autenticazione di base o del digest. il browser invia automaticamente le credenziali fino alla scadenza della sessione.

## <a name="anti-forgery-tokens"></a>Token anti-falsificazione

Per evitare gli attacchi CSRF, ASP.NET MVC usa i token antifalsificazione, detti anche *token di verifica della richiesta*.

1. Il client richiede una pagina HTML contenente un modulo.
2. Il server include due token nella risposta. Un token viene inviato come cookie. L'altra viene inserita in un campo del form nascosto. I token vengono generati in modo casuale, in modo che un antagonista non possa indovinare i valori.
3. Quando il client invia il modulo, deve restituire entrambi i token al server. Il client invia il token del cookie come cookie e invia il token del form all'interno dei dati del form. Un client browser esegue automaticamente questa operazione quando l'utente invia il modulo.
4. Se una richiesta non include entrambi i token, il server non consente la richiesta.

Di seguito è riportato un esempio di un form HTML con un token di form nascosto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

I token antifalsificazione funzionano perché la pagina dannosa non è in grado di leggere i token dell'utente, a causa dei criteri di stessa origine. I[criteri della stessa origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) impediscono ai documenti ospitati in due siti diversi di accedere al contenuto. Nell'esempio precedente, la pagina dannosa può inviare richieste a example.com, ma non può leggere la risposta.

Per evitare gli attacchi CSRF, usare i token antifalsificazione con qualsiasi protocollo di autenticazione in cui il browser invia automaticamente le credenziali dopo l'accesso dell'utente. Sono inclusi i protocolli di autenticazione basati su cookie, ad esempio l'autenticazione basata su form, oltre a protocolli quali l'autenticazione di base e del digest.

È necessario richiedere token anti-falsificazione per qualsiasi metodo non sicuro (POST, PUT, DELETE). Assicurarsi inoltre che i metodi sicuri (GET, HEAD) non abbiano effetti collaterali. Inoltre, se si Abilita il supporto tra domini, ad esempio CORS o JSONP, anche metodi sicuri come GET sono potenzialmente vulnerabili agli attacchi CSRF, consentendo all'utente malintenzionato di leggere dati potenzialmente sensibili.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Token anti-falsificazione in ASP.NET MVC

Per aggiungere i token anti-falsificazione a una pagina Razor, usare il metodo helper **HtmlHelper. AntiForgeryToken** :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Questo metodo aggiunge il campo del form nascosto e imposta anche il token del cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF e AJAX

Il token del modulo può costituire un problema per le richieste AJAX, perché una richiesta AJAX può inviare dati JSON, non dati del form HTML. Una soluzione consiste nell'inviare i token in un'intestazione HTTP personalizzata. Il codice seguente usa la sintassi Razor per generare i token e quindi li aggiunge a una richiesta AJAX. I token vengono generati nel server chiamando **antifalsificazione. GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Quando si elabora la richiesta, estrarre i token dall'intestazione della richiesta. Chiamare quindi il metodo **Antifalsification. Validate** per convalidare i token. Il metodo **Validate** genera un'eccezione se i token non sono validi.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
