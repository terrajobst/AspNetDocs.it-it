---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteggere un'API Web con account singoli e account di accesso locale in API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Questo argomento illustra come proteggere un'API Web usando OAuth2 per eseguire l'autenticazione in un database di appartenenza. Versioni del software usate nell'esercitazione Visual Studio 201...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555195"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteggere un'API Web con account singoli e account di accesso locale in API Web ASP.NET 2,2

di [Mike Wasson](https://github.com/MikeWasson)

[Scaricare l'app di esempio](https://github.com/MikeWasson/LocalAccountsApp)

> Questo argomento illustra come proteggere un'API Web usando OAuth2 per eseguire l'autenticazione in un database di appartenenza.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2,1](../../../identity/index.md)

In Visual Studio 2013 il modello di progetto API Web offre tre opzioni per l'autenticazione:

- **Singoli account.** L'app usa un database di appartenenza.
- **Account aziendali.** Gli utenti possono accedere con le proprie Azure Active Directory, Office 365 o le credenziali Active Directory locali.
- **Autenticazione di Windows.** Questa opzione è destinata alle applicazioni Intranet e usa il modulo IIS per l'autenticazione di Windows.

Per ulteriori informazioni su queste opzioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

I singoli account consentono a un utente di accedere in due modi:

- **Account di accesso locale**. L'utente esegue la registrazione nel sito, immettendo un nome utente e una password. L'app archivia l'hash della password nel database delle appartenenze. Quando l'utente esegue l'accesso, il sistema ASP.NET Identity verifica la password.
- **Accesso social**. L'utente accede con un servizio esterno, ad esempio Facebook, Microsoft o Google. L'app crea comunque una voce per l'utente nel database delle appartenenze, ma non archivia le credenziali. L'utente esegue l'autenticazione eseguendo l'accesso al servizio esterno.

Questo articolo esamina lo scenario di accesso locale. Per l'accesso locale e sociale, l'API Web USA OAuth2 per autenticare le richieste. Tuttavia, i flussi di credenziali sono diversi per l'accesso locale e sociale.

In questo articolo illustrerò una semplice app che consente all'utente di accedere e inviare chiamate AJAX autenticate a un'API Web. È possibile scaricare il codice di esempio [qui](https://github.com/MikeWasson/LocalAccountsApp). Il file Leggimi descrive come creare l'esempio da zero in Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

L'app di esempio USA knockout. js per l'associazione dati e jQuery per l'invio di richieste AJAX. Mi concentrerò sulle chiamate AJAX, quindi non è necessario saperne di più su knockout. js per questo articolo.

Lungo il percorso, descrivo:

- Cosa sta facendo l'app sul lato client.
- Cosa accade nel server.
- Traffico HTTP al centro.

Prima di tutto, è necessario definire una terminologia di OAuth2.

- *Risorsa*. Alcuni dati che possono essere protetti.
- *Server di risorse*. Server che ospita la risorsa.
- *Proprietario della risorsa*. Entità che può concedere l'autorizzazione per accedere a una risorsa. (In genere l'utente).
- *Client*: l'app che vuole accedere alla risorsa. In questo articolo, il client è un Web browser.
- *Token di accesso*. Token che concede l'accesso a una risorsa.
- *Token di porta*. Tipo particolare di token di accesso, con la proprietà che chiunque può utilizzare il token. In altre parole, un client non necessita di una chiave crittografica o di un altro segreto per usare un bearer token. Per questo motivo, i token di porta devono essere usati solo su un HTTPS e devono avere un tempo di scadenza relativamente breve.
- *Server di autorizzazione*. Server che fornisce i token di accesso.

Un'applicazione può fungere sia da server di autorizzazione che da server di risorse. Il modello di progetto API Web segue questo modello.

## <a name="local-login-credential-flow"></a>Flusso credenziali di accesso locale

Per l'accesso locale, l'API Web usa il [flusso di password del proprietario della risorsa](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definito in OAuth2.

1. L'utente immette un nome e una password nel client.
2. Il client invia queste credenziali al server di autorizzazione.
3. Il server di autorizzazione autentica le credenziali e restituisce un token di accesso.
4. Per accedere a una risorsa protetta, il client include il token di accesso nell'intestazione dell'autorizzazione della richiesta HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Quando si selezionano **singoli account** nel modello di progetto API Web, il progetto include un server di autorizzazione che convalida le credenziali utente e rilascia i token. Il diagramma seguente mostra lo stesso flusso di credenziali in termini di componenti dell'API Web.

![](individual-accounts-in-web-api/_static/image4.png)

In questo scenario, i controller dell'API Web fungono da server di risorse. Un filtro di autenticazione convalida i token di accesso e l'attributo **[autoautorizz]** viene usato per proteggere una risorsa. Quando un controller o un'azione ha l'attributo **[autorizzate]** , è necessario autenticare tutte le richieste a tale controller o azione. In caso contrario, l'autorizzazione viene negata e l'API Web restituisce un errore 401 (non autorizzato).

Il server di autorizzazione e il filtro di autenticazione chiamano entrambi un componente [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) che gestisce i dettagli di OAuth2. La progettazione verrà descritta in modo più dettagliato più avanti in questa esercitazione.

## <a name="sending-an-unauthorized-request"></a>Invio di una richiesta non autorizzata

Per iniziare, eseguire l'app e fare clic sul pulsante **Call API (chiama API** ). Al termine della richiesta, nella casella dei **risultati** dovrebbe essere visualizzato un messaggio di errore. Questo perché la richiesta non contiene un token di accesso, quindi la richiesta non è autorizzata.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Il pulsante **chiama API** invia una richiesta AJAX a ~/API/values, che richiama un'azione del controller API Web. Di seguito è illustrata la sezione del codice JavaScript che invia la richiesta AJAX. Nell'app di esempio, tutto il codice dell'app JavaScript si trova nel file Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Fino a quando l'utente non esegue l'accesso, non esiste alcun bearer token e pertanto non è presente alcuna intestazione di autorizzazione nella richiesta. In questo modo, la richiesta restituirà un errore 401.

Di seguito è illustrata la richiesta HTTP. (Ho utilizzato [Fiddler](http://www.telerik.com/fiddler) per acquisire il traffico http).

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Si noti che la risposta include un'intestazione WWW-Authenticate con la richiesta impostata su Bearer. Indica che il server prevede un bearer token.

## <a name="register-a-user"></a>Registrare un utente

Nella sezione **Register** dell'app immettere un messaggio di posta elettronica e una password e fare clic sul pulsante **Register (registra** ).

Non è necessario usare un indirizzo di posta elettronica valido per questo esempio, ma una vera app conferma l'indirizzo. (Vedere [creare un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Per la password, usare un valore simile a "Password1!", con una lettera maiuscola, una lettera minuscola, un numero e un carattere non alfanumerico. Per semplificare l'app, ho lasciato la convalida lato client, pertanto se si verifica un problema con il formato della password, si otterrà un errore 400 (richiesta non valida).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Il pulsante **Register** invia una richiesta post a ~/API/account/Register/. Il corpo della richiesta è un oggetto JSON che include il nome e la password. Ecco il codice JavaScript che invia la richiesta:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Questa richiesta viene gestita dalla classe `AccountController`. Internamente, `AccountController` utilizza ASP.NET Identity per gestire il database delle appartenenze.

Se l'app viene eseguita localmente da Visual Studio, gli account utente vengono archiviati nel database locale, nella tabella AspNetUsers. Per visualizzare le tabelle in Visual Studio, fare clic sul menu **Visualizza** , selezionare **Esplora server**, quindi **connessioni dati**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Ottenere un token di accesso

Finora non è stato eseguito alcun OAuth, ma ora verrà visualizzato il server di autorizzazione OAuth in azione quando si richiede un token di accesso. Nell'area **accesso** dell'app di esempio, immettere il messaggio di posta elettronica e la password e fare clic su **Accedi**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Il pulsante **Accedi** invia una richiesta all'endpoint token. Il corpo della richiesta contiene i dati con codifica URL form seguenti:

- Concedi\_tipo: "password"
- nome utente: &lt;l'indirizzo di posta elettronica dell'utente&gt;
- password: &lt;password&gt;

Ecco il codice JavaScript che invia la richiesta AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Se la richiesta ha esito positivo, il server di autorizzazione restituisce un token di accesso nel corpo della risposta. Si noti che il token viene archiviato nell'archiviazione della sessione, da usare in un secondo momento quando si inviano richieste all'API. A differenza di alcune forme di autenticazione, ad esempio l'autenticazione basata su cookie, il browser non includerà automaticamente il token di accesso nelle richieste successive. L'applicazione deve eseguire questa operazione in modo esplicito. Questo è un aspetto positivo, perché limita le [vulnerabilità di CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

È possibile osservare che la richiesta contiene le credenziali dell'utente. Per fornire la sicurezza a livello di trasporto, è *necessario* utilizzare HTTPS.

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Per migliorare la leggibilità, ho rientrato il JSON e troncato il token di accesso, che è molto lungo.

Le proprietà `access_token`, `token_type`e `expires_in` sono definite dalla specifica OAuth2. Le altre proprietà (`userName`, `.issued`e `.expires`) sono solo a scopo informativo. Nel file/Providers/ApplicationOAuthProvider.cs è possibile trovare il codice che aggiunge le proprietà aggiuntive nel metodo `TokenEndpoint`.

## <a name="send-an-authenticated-request"></a>Inviare una richiesta autenticata

Ora che si dispone di un bearer token, è possibile effettuare una richiesta autenticata all'API. Questa operazione viene eseguita impostando l'intestazione di autorizzazione nella richiesta. Fare di nuovo clic sul pulsante **Call API (chiama API** ) per visualizzarlo.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Disconnetti

Poiché il browser non memorizza nella cache le credenziali o il token di accesso, la disconnessione è semplicemente una cosa che consente di dimenticare il token, rimuovendo il token dall'archiviazione della sessione:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Informazioni sul modello di progetto dei singoli account

Quando si selezionano **singoli account** nel modello di progetto applicazione Web ASP.NET, il progetto include:

- Un server di autorizzazione OAuth2.
- Un endpoint dell'API Web per la gestione degli account utente
- Modello EF per l'archiviazione degli account utente.

Di seguito sono riportate le principali classi dell'applicazione che implementano queste funzionalità:

- `AccountController` Fornisce un endpoint API Web per la gestione degli account utente. L'azione `Register` è l'unica che è stata usata in questa esercitazione. Altri metodi della classe supportano la reimpostazione della password, gli account di accesso di social networking e altre funzionalità.
- `ApplicationUser`, definito in/Models/IdentityModels.cs. Questa classe rappresenta il modello EF per gli account utente nel database delle appartenenze.
- `ApplicationUserManager`, definito in/app\_Start/IdentityConfig. cs, questa classe deriva da [usermanager](https://msdn.microsoft.com/library/dn613290.aspx) ed esegue operazioni sugli account utente, ad esempio la creazione di un nuovo utente, la verifica delle password e così via, e rende automaticamente permanente le modifiche apportate al database.
- `ApplicationOAuthProvider` Questo oggetto si connette al middleware OWIN ed elabora gli eventi generati dal middleware. Deriva da [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurazione del server di autorizzazione

In StartupAuth.cs il codice seguente configura il server di autorizzazione OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

La proprietà `TokenEndpointPath` è il percorso URL dell'endpoint del server di autorizzazione. Questo è l'URL usato dall'app per ottenere i token di porta.

La proprietà `Provider` specifica un provider che si connette al middleware OWIN ed elabora gli eventi generati dal middleware.

Di seguito è riportato il flusso di base quando l'app vuole ottenere un token:

1. Per ottenere un token di accesso, l'app invia una richiesta a ~/token.
2. Il middleware OAuth chiama `GrantResourceOwnerCredentials` sul provider.
3. Il provider chiama il `ApplicationUserManager` per convalidare le credenziali e creare un'identità delle attestazioni.
4. Se l'operazione ha esito positivo, il provider crea un ticket di autenticazione, usato per generare il token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Il middleware OAuth non sa nulla sugli account utente. Il provider comunica tra il middleware e ASP.NET Identity. Per ulteriori informazioni sull'implementazione del server di autorizzazione, vedere [OWIN OAuth 2,0 Authorization server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configurazione dell'API Web per l'uso di token di porta

Nel metodo `WebApiConfig.Register` il codice seguente configura l'autenticazione per la pipeline dell'API Web:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

La classe **HostAuthenticationFilter** consente l'autenticazione usando i token di porta.

Il metodo **SuppressDefaultHostAuthentication** indica all'API Web di ignorare qualsiasi autenticazione eseguita prima che la richiesta raggiunga la pipeline dell'API Web, tramite IIS o il middleware OWIN. In questo modo, è possibile limitare l'autenticazione dell'API Web usando solo i token di connessione.

> [!NOTE]
> In particolare, la parte MVC dell'app potrebbe usare l'autenticazione basata su form, in cui le credenziali vengono archiviate in un cookie. Per impedire attacchi CSRF, l'autenticazione basata su cookie richiede l'uso di token antifalsificazione. Questo è un problema per le API Web, perché non esiste un modo pratico per l'API Web di inviare il token anti-falsificazione al client. (Per altre informazioni su questo problema, vedere [prevenzione degli attacchi CSRF nell'API Web](preventing-cross-site-request-forgery-csrf-attacks.md)). La chiamata a **SuppressDefaultHostAuthentication** garantisce che l'API Web non sia vulnerabile agli attacchi CSRF dalle credenziali archiviate nei cookie.

Quando il client richiede una risorsa protetta, ecco cosa accade nella pipeline dell'API Web:

1. Il filtro **HostAuthentication** chiama il middleware OAuth per convalidare il token.
2. Il middleware converte il token in un'identità delle attestazioni.
3. A questo punto, la richiesta viene *autenticata* , ma non *autorizzata*.
4. Il filtro di autorizzazione esamina l'identità delle attestazioni. Se le attestazioni autorizzano l'utente per la risorsa, la richiesta è autorizzata. Per impostazione predefinita, l'attributo **[autorizzate]** autorizzerà qualsiasi richiesta autenticata. Tuttavia, è possibile autorizzare in base al ruolo o ad altre attestazioni. Per altre informazioni, vedere [autenticazione e autorizzazione nell'API Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Se i passaggi precedenti hanno esito positivo, il controller restituisce la risorsa protetta. In caso contrario, il client riceve un errore 401 (non autorizzato).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET Identity](../../../identity/index.md)
- [Informazioni sulle funzionalità di sicurezza nel modello di Spa per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Post di Blog MSDN di Hongye Sun.
- [Dissezione del modello di account singoli dell'API Web-parte 2: account locali](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Post di Blog di Domenico Baier.
- [Autenticazione host e API Web con OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una spiegazione efficace di `SuppressDefaultHostAuthentication` e `HostAuthenticationFilter` da Brock Allen.
- [Personalizzazione delle informazioni del profilo in ASP.NET Identity nei modelli di Visual studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Post di Blog MSDN di Rastogi.
- Per [la gestione della durata delle richieste per la classe usermanager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Post di Blog MSDN di Suhas Joshi, con una spiegazione corretta della classe `UserManager`.
