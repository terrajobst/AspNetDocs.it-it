---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticazione e autorizzazione in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Fornisce una panoramica generale dell'autenticazione e dell'autorizzazione in API Web ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598644"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentication and Authorization in ASP.NET Web API (in inglese)

di [Mike Wasson](https://github.com/MikeWasson)

È stata creata un'API Web, ma ora si vuole controllare l'accesso. In questa serie di articoli verranno esaminate alcune opzioni per la protezione di un'API Web da utenti non autorizzati. Questa serie riguarderà sia l'autenticazione che l'autorizzazione.

- L' *autenticazione* sta conoscendo l'identità dell'utente. Ad esempio, Alice accede con il nome utente e la password e il server usa la password per autenticare Alice.
- L' *autorizzazione* sta decidendo se un utente è autorizzato a eseguire un'azione. Ad esempio, Alice dispone dell'autorizzazione per ottenere una risorsa, ma non creare una risorsa.

Il primo articolo della serie fornisce una panoramica generale dell'autenticazione e dell'autorizzazione in API Web ASP.NET. Altri argomenti descrivono scenari di autenticazione comuni per l'API Web.

> [!NOTE]
> Grazie alle persone che hanno esaminato questa serie e fornito un prezioso feedback: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei GE, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Authentication

L'API Web presuppone che l'autenticazione avvenga nell'host. Per l'hosting Web, l'host è IIS, che usa i moduli HTTP per l'autenticazione. È possibile configurare il progetto in modo che usi uno dei moduli di autenticazione integrati in IIS o ASP.NET oppure scrivere il proprio modulo HTTP per eseguire l'autenticazione personalizzata.

Quando l'host autentica l'utente, viene creata un' *entità*, ovvero un oggetto [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) che rappresenta il contesto di sicurezza in cui è in esecuzione il codice. L'host connette l'entità al thread corrente impostando **thread. CurrentPrincipal**. L'entità contiene un oggetto **Identity** associato che contiene informazioni sull'utente. Se l'utente viene autenticato, la proprietà **Identity. IsAuthenticated** restituisce **true**. Per le richieste anonime, l' **autenticazione** viene restituita **false**. Per ulteriori informazioni sulle entità, vedere [protezione basata sui ruoli](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Gestori di messaggi HTTP per l'autenticazione

Anziché utilizzare l'host per l'autenticazione, è possibile inserire la logica di autenticazione in un [gestore di messaggi http](../advanced/http-message-handlers.md). In tal caso, il gestore di messaggi esamina la richiesta HTTP e imposta l'oggetto Principal.

Quando è consigliabile utilizzare i gestori di messaggi per l'autenticazione? Ecco alcuni compromessi:

- Un modulo HTTP Visualizza tutte le richieste che passano attraverso la pipeline ASP.NET. Un gestore di messaggi vede solo le richieste indirizzate all'API Web.
- È possibile impostare gestori di messaggi per Route, che consentono di applicare uno schema di autenticazione a una route specifica.
- I moduli HTTP sono specifici di IIS. I gestori di messaggi sono indipendenti dall'host, quindi possono essere usati con l'hosting Web e l'hosting automatico.
- I moduli HTTP partecipano alla registrazione, al controllo e così via di IIS.
- I moduli HTTP vengono eseguiti in precedenza nella pipeline. Se si gestisce l'autenticazione in un gestore di messaggi, l'entità non viene impostata fino a quando il gestore non viene eseguito. Inoltre, l'entità torna all'entità precedente quando la risposta lascia il gestore dei messaggi.

In generale, se non è necessario supportare l'hosting automatico, un modulo HTTP è un'opzione migliore. Se è necessario supportare l'hosting automatico, prendere in considerazione un gestore di messaggi.

### <a name="setting-the-principal"></a>Impostazione dell'entità

Se l'applicazione esegue una logica di autenticazione personalizzata, è necessario impostare l'entità su due posizioni:

- **Thread. CurrentPrincipal**. Questa proprietà rappresenta il modo standard per impostare l'entità del thread in .NET.
- **HttpContext. Current. User**. Questa proprietà è specifica di ASP.NET.

Nel codice seguente viene illustrato come impostare l'entità:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Per l'hosting Web, è necessario impostare l'entità in entrambe le posizioni. in caso contrario, il contesto di sicurezza potrebbe diventare incoerente. Per il self-hosting, tuttavia, **HttpContext. Current** è null. Per assicurarsi che il codice sia indipendente dall'host, verificare quindi la presenza di valori null prima dell'assegnazione a **HttpContext. Current**, come illustrato.

## <a name="authorization"></a>Autorizzazione

L'autorizzazione si verifica in un secondo momento nella pipeline, più vicina al controller. Questo consente di effettuare scelte più granulari quando si concede l'accesso alle risorse.

- I *filtri di autorizzazione* vengono eseguiti prima dell'azione del controller. Se la richiesta non è autorizzata, il filtro restituisce una risposta di errore e l'azione non viene richiamata.
- All'interno di un'azione del controller, è possibile ottenere l'entità corrente dalla proprietà **ApiController. User** . Ad esempio, è possibile filtrare un elenco di risorse in base al nome utente, restituendo solo le risorse che appartengono a tale utente.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Utilizzo dell'attributo [autorizzate]

L'API Web fornisce un filtro di autorizzazione incorporato, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Questo filtro controlla se l'utente è autenticato. In caso contrario, restituisce il codice di stato HTTP 401 (non autorizzato), senza richiamare l'azione.

È possibile applicare il filtro a livello globale, a livello di controller o a livello di singole azioni.

A **livello globale**: per limitare l'accesso per ogni controller API Web, aggiungere il filtro **AuthorizeAttribute** all'elenco di filtri globale:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: per limitare l'accesso per un controller specifico, aggiungere il filtro come attributo al controller:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Azione**: per limitare l'accesso per azioni specifiche, aggiungere l'attributo al metodo di azione:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

In alternativa, è possibile limitare il controller e quindi consentire l'accesso anonimo a azioni specifiche usando l'attributo `[AllowAnonymous]`. Nell'esempio seguente il metodo `Post` è limitato, ma il metodo `Get` consente l'accesso anonimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Negli esempi precedenti, il filtro consente a qualsiasi utente autenticato di accedere ai metodi con restrizioni. vengono mantenuti solo gli utenti anonimi. È inoltre possibile limitare l'accesso a utenti specifici o a utenti in ruoli specifici:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Il filtro **AuthorizeAttribute** per i controller API Web si trova nello spazio dei nomi **System. Web. http** . Esiste un filtro simile per i controller MVC nello spazio dei nomi **System. Web. Mvc** , che non è compatibile con i controller API Web.

### <a name="custom-authorization-filters"></a>Filtri di autorizzazione personalizzati

Per scrivere un filtro di autorizzazione personalizzato, derivare da uno di questi tipi:

- **AuthorizeAttribute**. Estendere questa classe per eseguire la logica di autorizzazione basata sull'utente corrente e sui ruoli dell'utente.
- **AuthorizationFilterAttribute**. Estendere questa classe per eseguire la logica di autorizzazione sincrona che non è necessariamente basata sull'utente o sul ruolo corrente.
- **IAuthorizationFilter**. Implementare questa interfaccia per eseguire la logica di autorizzazione asincrona; Se, ad esempio, la logica di autorizzazione esegue chiamate di I/O asincrone o di rete. Se la logica di autorizzazione è associata alla CPU, è più semplice derivare da **AuthorizationFilterAttribute**, perché non è necessario scrivere un metodo asincrono.

Il diagramma seguente mostra la gerarchia di classi per la classe **AuthorizeAttribute** .

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorizzazione all'interno di un'azione del controller

In alcuni casi, è possibile consentire a una richiesta di procedere, ma modificare il comportamento in base al principale. Ad esempio, le informazioni restituite potrebbero variare a seconda del ruolo dell'utente. All'interno di un metodo del controller, è possibile ottenere l'entità corrente dalla proprietà **ApiController. User** .

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
