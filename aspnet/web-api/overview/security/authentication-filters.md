---
uid: web-api/overview/security/authentication-filters
title: Filtri di autenticazione in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Un filtro di autenticazione è un componente che autentica una richiesta HTTP. Le API Web 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma si differenziano leggermente...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: b6815baf05303d5f47a14ee5fe0fdfc2836c1868
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519375"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Filtri di autenticazione in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

> Un filtro di autenticazione è un componente che autentica una richiesta HTTP. Le API Web 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma si differenziano leggermente, soprattutto nelle convenzioni di denominazione per l'interfaccia di filtro. Questo argomento descrive i filtri di autenticazione dell'API Web.

I filtri di autenticazione consentono di impostare uno schema di autenticazione per singoli controller o azioni. In questo modo, l'app può supportare diversi meccanismi di autenticazione per risorse HTTP diverse.

In questo articolo verrà illustrato il codice dell'esempio di [autenticazione di base](http://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) su [https://github.com/aspnet/samples](https://github.com/aspnet/samples). Nell'esempio viene illustrato un filtro di autenticazione che implementa lo schema di autenticazione di base HTTP (RFC 2617). Il filtro viene implementato in una classe denominata `IdentityBasicAuthenticationAttribute`. Non visualizzo tutto il codice dell'esempio, bensì solo le parti che illustrano come scrivere un filtro di autenticazione.

## <a name="setting-an-authentication-filter"></a>Impostazione di un filtro di autenticazione

Analogamente ad altri filtri, è possibile applicare i filtri di autenticazione per controller, per azione o a livello globale a tutti i controller API Web.

Per applicare un filtro di autenticazione a un controller, decorare la classe controller con l'attributo Filter. Il codice seguente imposta il filtro `[IdentityBasicAuthentication]` su una classe controller, che Abilita l'autenticazione di base per tutte le azioni del controller.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Per applicare il filtro a un'azione, decorare l'azione con il filtro. Il codice seguente imposta il filtro `[IdentityBasicAuthentication]` sul metodo `Post` del controller.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Per applicare il filtro a tutti i controller API Web, aggiungerlo a **GlobalConfiguration. filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementazione di un filtro di autenticazione API Web

Nell'API Web i filtri di autenticazione implementano l'interfaccia [System. Web. http. filters. IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) . Devono anche ereditare da **System. Attribute**, per essere applicati come attributi.

L'interfaccia **IAuthenticationFilter** dispone di due metodi:

- **AuthenticateAsync** autentica la richiesta convalidando le credenziali nella richiesta, se presente.
- **ChallengeAsync** aggiunge una richiesta di autenticazione alla risposta http, se necessario.

Questi metodi corrispondono al flusso di autenticazione definito in [rfc 2612](http://tools.ietf.org/html/rfc2616) e [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Il client invia le credenziali nell'intestazione dell'autorizzazione. Questa situazione si verifica in genere dopo che il client riceve una risposta 401 (non autorizzata) dal server. Tuttavia, un client può inviare le credenziali con qualsiasi richiesta, non solo dopo aver ricevuto un 401.
2. Se il server non accetta le credenziali, restituisce una risposta 401 (non autorizzato). La risposta include un'intestazione WWW-Authenticate che contiene uno o più problemi. Ogni richiesta specifica uno schema di autenticazione riconosciuto dal server.

Il server può anche restituire 401 da una richiesta anonima. In realtà, questo è in genere il modo in cui viene avviato il processo di autenticazione:

1. Il client invia una richiesta anonima.
2. Il server restituisce 401.
3. I client inviano di nuovo la richiesta con credenziali.

Questo flusso include i passaggi di *autenticazione* e *autorizzazione* .

- L'autenticazione dimostra l'identità del client.
- L'autorizzazione determina se il client può accedere a una determinata risorsa.

Nell'API Web i filtri di autenticazione gestiscono l'autenticazione, ma non l'autorizzazione. L'autorizzazione deve essere eseguita da un filtro di autorizzazione o all'interno dell'azione del controller.

Ecco il flusso nella pipeline Web API 2:

1. Prima di richiamare un'azione, l'API Web crea un elenco dei filtri di autenticazione per l'azione. Sono inclusi i filtri con ambito dell'azione, ambito del controller e ambito globale.
2. L'API Web chiama **authenticateAsync** a ogni filtro dell'elenco. Ogni filtro può convalidare le credenziali nella richiesta. Se un filtro convalida correttamente le credenziali, il filtro crea una **IPrincipal** e la collega alla richiesta. Un filtro può anche generare un errore a questo punto. In tal caso, il resto della pipeline non viene eseguito.
3. Supponendo che non vi sia alcun errore, la richiesta scorre il resto della pipeline.
4. Infine, l'API Web chiama il metodo **ChallengeAsync** di ogni filtro di autenticazione. I filtri usano questo metodo per aggiungere una richiesta di verifica alla risposta, se necessario. In genere (ma non sempre) che verrebbe eseguita in risposta a un errore 401.

I diagrammi seguenti mostrano due casi possibili. Nel primo, il filtro di autenticazione esegue correttamente l'autenticazione della richiesta, un filtro di autorizzazione autorizza la richiesta e l'azione del controller restituisce 200 (OK).

![](authentication-filters/_static/image1.png)

Nel secondo esempio, il filtro di autenticazione autentica la richiesta, ma il filtro di autorizzazione restituisce 401 (non autorizzato). In questo caso, l'azione del controller non viene richiamata. Il filtro di autenticazione aggiunge un'intestazione WWW-Authenticate alla risposta.

![](authentication-filters/_static/image2.png)

Sono possibili altre combinazioni&mdash;ad esempio, se l'azione del controller consente richieste anonime, potrebbe essere presente un filtro di autenticazione, ma nessuna autorizzazione.

## <a name="implementing-the-authenticateasync-method"></a>Implementazione del metodo AuthenticateAsync

Il metodo **authenticateAsync** tenta di autenticare la richiesta. Ecco la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Il metodo **authenticateAsync** deve eseguire una delle operazioni seguenti:

1. Niente (no-op).
2. Creare una **IPrincipal** e impostarla sulla richiesta.
3. Impostare un risultato di errore.

L'opzione (1) indica che la richiesta non contiene credenziali che il filtro riconosce. L'opzione (2) indica che il filtro ha autenticato correttamente la richiesta. L'opzione (3) indica che la richiesta dispone di credenziali non valide (ad esempio la password errata), che attiva una risposta di errore.

Ecco una struttura generale per l'implementazione di **authenticateAsync**.

1. Cercare le credenziali nella richiesta.
2. Se non sono presenti credenziali, non eseguire alcuna operazione e restituire (no-op).
3. Se sono presenti credenziali, ma il filtro non riconosce lo schema di autenticazione, non eseguire alcuna operazione e restituire (no-op). Un altro filtro della pipeline potrebbe comprendere lo schema.
4. Se sono presenti credenziali che il filtro riconosce, provare a autenticarle.
5. Se le credenziali sono errate, restituire 401 impostando `context.ErrorResult`.
6. Se le credenziali sono valide, creare un **IPrincipal** e impostare `context.Principal`.

Il codice seguente mostra il metodo **authenticateAsync** dell'esempio di [autenticazione di base](http://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . I commenti indicano ogni passaggio. Il codice mostra diversi tipi di errore: un'intestazione dell'autorizzazione senza credenziali, credenziali in formato non valido e nome utente/password non valida.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Impostazione di un risultato di errore

Se le credenziali non sono valide, il filtro deve impostare `context.ErrorResult` su un **IHttpActionResult** che crea una risposta di errore. Per altre informazioni su **IHttpActionResult**, vedere [risultati delle azioni nell'API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

L'esempio di autenticazione di base include una classe `AuthenticationFailureResult` adatta a questo scopo.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementazione di ChallengeAsync

Lo scopo del metodo **ChallengeAsync** è l'aggiunta di problemi di autenticazione alla risposta, se necessario. Ecco la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Il metodo viene chiamato su ogni filtro di autenticazione nella pipeline della richiesta.

È importante comprendere che **ChallengeAsync** viene chiamato *prima* della creazione della risposta http e possibilmente anche prima dell'esecuzione dell'azione del controller. Quando **ChallengeAsync** viene chiamato, `context.Result` contiene un **IHttpActionResult**, che viene usato in un secondo momento per creare la risposta http. Quindi, quando viene chiamato **ChallengeAsync** , non è ancora possibile sapere nulla sulla risposta http. Il metodo **ChallengeAsync** deve sostituire il valore originale di `context.Result` con un nuovo **IHttpActionResult**. Questo **IHttpActionResult** deve eseguire il wrapping del `context.Result`originale.

![](authentication-filters/_static/image3.png)

Chiamerò il *risultato* **IHttpActionResult** originale e il nuovo **IHttpActionResult** il *risultato esterno*. Il risultato esterno deve eseguire le operazioni seguenti:

1. Richiamare il risultato interno per creare la risposta HTTP.
2. Esaminare la risposta.
3. Se necessario, aggiungere una richiesta di autenticazione alla risposta.

L'esempio seguente è tratto dall'esempio di autenticazione di base. Definisce un **IHttpActionResult** per il risultato esterno.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

La proprietà `InnerResult` include la **IHttpActionResult**interna. La proprietà `Challenge` rappresenta un'intestazione www-Authentication. Si noti che **ExecuteAsync** chiama prima `InnerResult.ExecuteAsync` per creare la risposta http e quindi aggiunge la richiesta di verifica, se necessario.

Prima di aggiungere la richiesta, controllare il codice di risposta. La maggior parte degli schemi di autenticazione aggiunge una richiesta di verifica solo se la risposta è 401, come illustrato di seguito. Tuttavia, alcuni schemi di autenticazione aggiungono una sfida a una risposta di esito positivo. Vedere ad esempio [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dato il `AddChallengeOnUnauthorizedResult` classe, il codice effettivo in **ChallengeAsync** è semplice. È sufficiente creare il risultato e collegarlo a `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: l'esempio di autenticazione di base astrae questa logica un po', inserendola in un metodo di estensione.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinazione di filtri di autenticazione con l'autenticazione a livello di host

"Autenticazione a livello di host" è l'autenticazione eseguita dall'host, ad esempio IIS, prima che la richiesta raggiunga il Framework dell'API Web.

Spesso può essere necessario abilitare l'autenticazione a livello di host per il resto dell'applicazione, ma disabilitarla per i controller API Web. Ad esempio, uno scenario tipico consiste nell'abilitare l'autenticazione basata su form a livello di host, ma usare l'autenticazione basata su token per l'API Web.

Per disabilitare l'autenticazione a livello di host all'interno della pipeline dell'API Web, chiamare `config.SuppressHostPrincipal()` nella configurazione. Questo fa sì che l'API Web elimini l' **IPrincipal** da qualsiasi richiesta che accede alla pipeline dell'API Web. In effetti, &quot;Annulla l'autenticazione&quot; richiesta.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Filtri di sicurezza API Web ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
