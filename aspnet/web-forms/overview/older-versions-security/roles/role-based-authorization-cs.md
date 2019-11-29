---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorizzazione basata sui ruoli (C#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione inizia con un'occhiata al modo in cui il Framework dei ruoli associa i ruoli di un utente al contesto di sicurezza. Esamina quindi come applicare l'URL basato sui ruoli...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 46153ab310bdee814baaa53c372fb92f8a23ce11
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619943"
---
# <a name="role-based-authorization-c"></a>Autorizzazione basata sui ruoli (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Questa esercitazione inizia con un'occhiata al modo in cui il Framework dei ruoli associa i ruoli di un utente al contesto di sicurezza. Viene quindi esaminato come applicare le regole di autorizzazione degli URL basate sui ruoli. In seguito, verrà esaminato l'utilizzo di metodi dichiarativi e programmatici per modificare i dati visualizzati e la funzionalità offerta da una pagina ASP.NET.

## <a name="introduction"></a>Introduzione

Nell'esercitazione <a id="_msoanchor_1"> </a>sull' [*autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) è stato illustrato come usare l'autorizzazione URL per specificare quali utenti possono visitare un determinato set di pagine. Con un po' di markup in `Web.config`, è possibile indicare a ASP.NET di consentire solo agli utenti autenticati di visitare una pagina. In alternativa, è possibile stabilire che siano consentiti solo gli utenti Tito e Bob oppure indicare che sono stati consentiti tutti gli utenti autenticati eccetto Sam.

Oltre all'autorizzazione URL, sono state esaminate anche le tecniche dichiarative e programmatiche per controllare i dati visualizzati e la funzionalità offerta da una pagina basata sull'utente che visita. In particolare, è stata creata una pagina che elenca il contenuto della directory corrente. Chiunque può visitare questa pagina, ma solo gli utenti autenticati possono visualizzare il contenuto dei file e solo Tito potrebbe eliminare i file.

Applicare le regole di autorizzazione in base all'utente può crescere in un incubo di contabilità. Un approccio più gestibile consiste nell'usare l'autorizzazione basata sui ruoli. La buona notizia è che gli strumenti a disposizione per applicare le regole di autorizzazione funzionano correttamente con i ruoli come per gli account utente. Le regole di autorizzazione URL possono specificare ruoli anziché utenti. Il controllo LoginView, che esegue il rendering di output diversi per utenti autenticati e anonimi, può essere configurato in modo da visualizzare contenuti diversi in base ai ruoli dell'utente connesso. E l'API roles include metodi per determinare i ruoli dell'utente connesso.

Questa esercitazione inizia con un'occhiata al modo in cui il Framework dei ruoli associa i ruoli di un utente al contesto di sicurezza. Viene quindi esaminato come applicare le regole di autorizzazione degli URL basate sui ruoli. In seguito, verrà esaminato l'utilizzo di metodi dichiarativi e programmatici per modificare i dati visualizzati e la funzionalità offerta da una pagina ASP.NET. Iniziamo!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Informazioni sul modo in cui i ruoli sono associati al contesto di sicurezza di un utente

Ogni volta che una richiesta entra nella pipeline ASP.NET, viene associata a un contesto di sicurezza, che include le informazioni che identificano il richiedente. Quando si usa l'autenticazione basata su form, viene usato un ticket di autenticazione come token di identità. <a id="_msoanchor_2"> </a>Come illustrato nelle esercitazioni sulla configurazione dell'autenticazione basata su <a id="_msoanchor_3"> </a>form e [*dell'* ](../introduction/an-overview-of-forms-authentication-cs.md) [*autenticazione basata su form e sugli argomenti avanzati*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) , il `FormsAuthenticationModule` è responsabile della determinazione dell'identità del richiedente, operazione eseguita durante l' [`AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se viene trovato un ticket di autenticazione non scaduto valido, il `FormsAuthenticationModule` lo decodifica per verificare l'identità del richiedente. Crea un nuovo oggetto `GenericPrincipal` e lo assegna all'oggetto `HttpContext.User`. Lo scopo di un'entità, come `GenericPrincipal`, consiste nell'identificare il nome dell'utente autenticato e i ruoli a cui appartiene. Questo scopo è evidente dal fatto che tutti gli oggetti Principal hanno una proprietà `Identity` e un metodo di `IsInRole(roleName)`. Il `FormsAuthenticationModule`, tuttavia, non è interessato alla registrazione delle informazioni sui ruoli e l'oggetto `GenericPrincipal` creato non specifica alcun ruolo.

Se il Framework dei ruoli è abilitato, il [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) modulo HTTP viene eseguito dopo l'`FormsAuthenticationModule` e identifica i ruoli dell'utente autenticato durante l' [evento`PostAuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), che viene attivato dopo l'evento `AuthenticateRequest`. Se la richiesta viene eseguita da un utente autenticato, il `RoleManagerModule` sovrascrive l'oggetto `GenericPrincipal` creato dal `FormsAuthenticationModule` e lo sostituisce con un [oggetto`RolePrincipal`](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La classe `RolePrincipal` usa l'API Roles per determinare a quali ruoli appartiene l'utente.

Nella figura 1 viene illustrato il flusso di lavoro della pipeline ASP.NET quando si utilizza l'autenticazione basata su form e il Framework dei ruoli. Il `FormsAuthenticationModule` viene eseguito per primo, identifica l'utente tramite il ticket di autenticazione e crea un nuovo oggetto `GenericPrincipal`. Successivamente, il `RoleManagerModule` passaggi in e sovrascrive l'oggetto `GenericPrincipal` con un oggetto `RolePrincipal`.

Se un utente anonimo visita il sito, né il `FormsAuthenticationModule` né il `RoleManagerModule` crea un oggetto Principal.

[![gli eventi della pipeline ASP.NET per un utente autenticato quando si usa l'autenticazione basata su form e il Framework dei ruoli](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Figura 1**: eventi della pipeline ASP.NET per un utente autenticato quando si usa l'autenticazione basata su form e il Framework dei ruoli ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Memorizzazione nella cache delle informazioni sui ruoli in un cookie

Il metodo di `IsInRole(roleName)` dell'oggetto `RolePrincipal` chiama `Roles.GetRolesForUser` per ottenere i ruoli per l'utente allo scopo di determinare se l'utente è un membro di *roleName*. Quando si usa il `SqlRoleProvider`, viene eseguita una query sul database dell'archivio dei ruoli. Quando si usano le regole di autorizzazione degli URL basate sui ruoli, il metodo di `IsInRole` `RolePrincipal`verrà chiamato a ogni richiesta a una pagina protetta dalle regole di autorizzazione degli URL basati sui ruoli. Anziché dover cercare le informazioni sui ruoli nel database a ogni richiesta, il Framework dei ruoli include un'opzione che consente di memorizzare nella cache i ruoli dell'utente in un cookie.

Se il Framework dei ruoli è configurato per memorizzare nella cache i ruoli dell'utente in un cookie, il `RoleManagerModule` crea il cookie durante l' [evento`EndRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)della pipeline ASP.NET. Questo cookie viene usato nelle richieste successive nella `PostAuthenticateRequest`, ovvero quando viene creato l'oggetto `RolePrincipal`. Se il cookie è valido e non è scaduto, i dati nel cookie vengono analizzati e usati per popolare i ruoli dell'utente, salvando così il `RolePrincipal` dalla necessità di effettuare una chiamata alla classe `Roles` per determinare i ruoli dell'utente. Nella figura 2 viene illustrato questo flusso di lavoro.

[![le informazioni sul ruolo dell'utente possono essere archiviate in un cookie per migliorare le prestazioni](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Figura 2**: le informazioni sul ruolo dell'utente possono essere archiviate in un cookie per migliorare le prestazioni ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image6.png))

Per impostazione predefinita, il meccanismo dei cookie della cache dei ruoli è disabilitato. Può essere abilitato tramite il markup di configurazione `<roleManager>` in `Web.config`. È stato illustrato l'uso dell' [elemento`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) per specificare i <a id="_msoanchor_4"> </a>provider di ruoli nell'esercitazione [*creazione e gestione dei ruoli*](creating-and-managing-roles-cs.md) , quindi è necessario che questo elemento sia già presente nel file di `Web.config` dell'applicazione. Le impostazioni dei cookie della cache dei ruoli vengono specificate come attributi dell'elemento `<roleManager>` e sono riepilogate nella tabella 1.

> [!NOTE]
> Le impostazioni di configurazione elencate nella tabella 1 specificano le proprietà del cookie della cache dei ruoli risultante. Per altre informazioni sui cookie, sul loro funzionamento e sulle relative proprietà, vedere [l'esercitazione](http://www.quirksmode.org/js/cookies.html)relativa ai cookie.

| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrizione</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Valore booleano che indica se viene utilizzata la memorizzazione nella cache del cookie. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Nome del cookie della cache dei ruoli. Il valore predefinito è ". ASPXROLES ".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Percorso per il cookie dei nomi di ruoli. L'attributo Path consente a uno sviluppatore di limitare l'ambito di un cookie a una particolare gerarchia di directory. Il valore predefinito è "/", che informa il browser di inviare il cookie del ticket di autenticazione a qualsiasi richiesta effettuata al dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica le tecniche utilizzate per proteggere il cookie della cache dei ruoli. I valori consentiti sono: `All` (impostazione predefinita). `Encryption`; `None`; e `Validation`. Per ulteriori informazioni su questi livelli di <a id="_anchor_5"> </a>protezione, fare riferimento al passaggio 3 dell'esercitazione relativa alla [*configurazione dell'autenticazione basata su form e agli argomenti avanzati*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Valore booleano che indica se è necessaria una connessione SSL per trasmettere il cookie di autenticazione. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valore booleano che indica se il timeout del cookie viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è `false`. Questo valore è pertinente solo quando `createPersistentCookie` è impostato su `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Specifica il tempo, in minuti, trascorso il quale il cookie del ticket di autenticazione scade. Il valore predefinito è `30`. Questo valore è pertinente solo quando `createPersistentCookie` è impostato su `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valore booleano che specifica se il cookie della cache dei ruoli è un cookie di sessione o un cookie permanente. Se `false` (impostazione predefinita), viene utilizzato un cookie di sessione, che viene eliminato quando il browser viene chiuso. Se `true`, viene utilizzato un cookie permanente; scade `cookieTimeout` numero di minuti dopo la creazione o dopo la visita precedente, a seconda del valore di `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Specifica il valore del dominio del cookie. Il valore predefinito è una stringa vuota, che fa sì che il browser usi il dominio da cui è stato emesso, ad esempio www.yourdomain.com. In questo caso, il cookie <strong>non</strong> verrà inviato quando si effettuano richieste a sottodomini, ad esempio admin.yourdomain.com. Se si desidera che il cookie venga passato a tutti i sottodomini, è necessario personalizzare l'attributo `domain`, impostandolo su "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Specifica il numero massimo di nomi di ruoli memorizzati nella cache del cookie. Il valore predefinito è 25. Il `RoleManagerModule` non crea un cookie per gli utenti che appartengono a più di `maxCachedResults` ruoli. Di conseguenza, il metodo di `IsInRole` dell'oggetto `RolePrincipal` utilizzerà la classe `Roles` per determinare i ruoli dell'utente. Il motivo `maxCachedResults` esiste perché molti agenti utente non consentono cookie di dimensioni superiori a 4.096 byte. Questo limite è quindi finalizzato a ridurre la probabilità di superare questo limite di dimensioni. Se sono presenti nomi di ruoli estremamente lunghi, è consigliabile specificare un valore di `maxCachedResults` inferiore; al contrario, se sono presenti nomi di ruolo estremamente brevi, probabilmente è possibile aumentare questo valore. |

**Tabella 1:** Opzioni di configurazione dei cookie della cache dei ruoli

Configurare l'applicazione per l'uso di cookie di cache dei ruoli non permanenti. A tale scopo, aggiornare l'elemento `<roleManager>` in `Web.config` per includere i seguenti attributi correlati ai cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

L'elemento `<roleManager>` è stato aggiornato aggiungendo tre attributi: `cacheRolesInCookie`, `createPersistentCookie`e `cookieProtection`. Impostando `cacheRolesInCookie` su `true`, il `RoleManagerModule` memorizza automaticamente i ruoli dell'utente in un cookie anziché dover cercare le informazioni sul ruolo dell'utente per ogni richiesta. In modo esplicito impostare gli attributi `createPersistentCookie` e `cookieProtection` rispettivamente su `false` e `All`. Tecnicamente, non avevo bisogno di specificare i valori per questi attributi poiché li ho appena assegnati ai valori predefiniti, ma li inseriamo per renderlo esplicitamente chiaro che non utilizzo cookie persistenti e che il cookie sia crittografato e convalidato.

Questo è tutto. Nel Framework dei ruoli i ruoli degli utenti vengono memorizzati nella cache dei cookie. Se il browser dell'utente non supporta i cookie o se i cookie vengono eliminati o persi, in qualche modo non è un problema: l'oggetto `RolePrincipal` utilizzerà semplicemente la classe `Roles` nel caso in cui non sia disponibile alcun cookie (o uno non valido o scaduto).

> [!NOTE]
> Modelli di Microsoft &amp; Practices Group sconsiglia l'utilizzo di cookie di cache dei ruoli permanenti. Poiché il possesso del cookie di cache dei ruoli è sufficiente per dimostrare l'appartenenza al ruolo, se un pirata informatico può ottenere l'accesso al cookie di un utente valido, può rappresentare tale utente. La probabilità che ciò accada aumenti se il cookie viene reso permanente nel browser dell'utente. Per altre informazioni su questa raccomandazione sulla sicurezza, oltre ad altri problemi di sicurezza, vedere l' [elenco delle domande di sicurezza per ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Passaggio 1: definizione delle regole di autorizzazione degli URL basate sui ruoli

Come illustrato nell'esercitazione <a id="_msoanchor_6"> </a> [*sull'autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) , l'autorizzazione URL consente di limitare l'accesso a un set di pagine in base all'utente o al ruolo. Le regole di autorizzazione URL vengono digitate in `Web.config` usando l' [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` e `<deny>` elementi figlio. Oltre alle regole di autorizzazione correlate all'utente descritte nelle esercitazioni precedenti, ogni `<allow>` e `<deny>` elemento figlio può includere anche:

- Un ruolo particolare
- Elenco delimitato da virgole di ruoli

Ad esempio, le regole di autorizzazione URL concedono l'accesso a tali utenti nei ruoli amministratori e supervisori, ma negano l'accesso a tutti gli altri:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

L'elemento `<allow>` nel markup precedente indica che i ruoli amministratori e supervisori sono consentiti. l'elemento `<deny>` indica che *tutti* gli utenti sono stati negati.

Verrà ora configurata l'applicazione in modo che le pagine `ManageRoles.aspx`, `UsersAndRoles.aspx`e `CreateUserWizardWithRoles.aspx` siano accessibili solo agli utenti nel ruolo di amministratore, mentre la pagina `RoleBasedAuthorization.aspx` rimane accessibile a tutti i visitatori.

A tale scopo, iniziare aggiungendo un file di `Web.config` alla cartella `Roles`.

[![aggiungere un file Web. config alla Directory Roles](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Figura 3**: aggiungere un File di `Web.config` alla directory `Roles` ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image9.png))

Aggiungere quindi il markup di configurazione seguente a `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

L'elemento `<authorization>` nella sezione `<system.web>` indica che solo gli utenti del ruolo Administrators possono accedere alle risorse di ASP.NET nella directory `Roles`. L'elemento `<location>` definisce un set alternativo di regole di autorizzazione URL per la pagina `RoleBasedAuthorization.aspx`, consentendo a tutti gli utenti di visitare la pagina.

Dopo aver salvato le modifiche apportate alla `Web.config`, accedere come utente non appartenente al ruolo Administrators, quindi provare a visitare una delle pagine protette. Il `UrlAuthorizationModule` rileverà che non si è autorizzati a visitare la risorsa richiesta. di conseguenza, il `FormsAuthenticationModule` reindirizzare l'utente alla pagina di accesso. Nella pagina di accesso verrà quindi reindirizzati alla pagina `UnauthorizedAccess.aspx` (vedere la figura 4). Questo reindirizzamento finale dalla pagina di accesso a `UnauthorizedAccess.aspx` si verifica a causa del codice aggiunto alla pagina di accesso nel passaggio 2 dell' <a id="_msoanchor_7"> </a>esercitazione [*sull'autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) . In particolare, la pagina di accesso reindirizza automaticamente qualsiasi utente autenticato a `UnauthorizedAccess.aspx` se la QueryString contiene un parametro di `ReturnUrl`, poiché questo parametro indica che l'utente è arrivato alla pagina di accesso dopo aver tentato di visualizzare una pagina che non è stata autorizzata a visualizzare.

[![solo gli utenti del ruolo Administrators possono visualizzare le pagine protette](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Figura 4**: solo gli utenti del ruolo Administrators possono visualizzare le pagine protette ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image12.png))

Disconnettersi e quindi accedere come utente appartenente al ruolo Administrators. A questo punto si dovrebbe essere in grado di visualizzare le tre pagine protette.

[![Tito può visitare la pagina UsersAndRoles. aspx perché si trova nel ruolo Administrators](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Figura 5**: Tito può visitare la pagina `UsersAndRoles.aspx` perché si trova nel ruolo Administrators ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image15.png))

> [!NOTE]
> Quando si specificano le regole di autorizzazione URL, per ruoli o utenti, è importante tenere presente che le regole vengono analizzate una alla volta, dall'alto verso il basso. Non appena viene rilevata una corrispondenza, all'utente viene concesso o negato l'accesso, a seconda se la corrispondenza è stata trovata in un elemento `<allow>` o `<deny>`. **Se non viene trovata alcuna corrispondenza, all'utente viene concesso l'accesso.** Di conseguenza, se si desidera limitare l'accesso a uno o più account utente, è fondamentale utilizzare un elemento `<deny>` come ultimo elemento nella configurazione dell'autorizzazione URL. **Se le regole di autorizzazione dell'URL non includono un** **elemento`<deny>`, a tutti gli utenti verrà concesso l'accesso.** Per una discussione più approfondita sul modo in cui vengono analizzate le regole di autorizzazione URL, vedere la sezione "informazioni sull'uso della `UrlAuthorizationModule` le regole di autorizzazione per concedere o negare l'accesso <a id="_msoanchor_8"> </a>" dell'esercitazione [*sull'autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Passaggio 2: limitazione della funzionalità in base ai ruoli dell'utente attualmente connesso

L'autorizzazione URL consente di specificare in modo semplice le regole di autorizzazione grossolane che indicano quali identità sono consentite e quali negano la visualizzazione di una pagina specifica o di tutte le pagine di una cartella e delle relative sottocartelle. Tuttavia, in alcuni casi potrebbe essere necessario consentire a tutti gli utenti di visitare una pagina, ma limitare le funzionalità della pagina in base ai ruoli degli utenti visitati. Questo può comportare la visualizzazione o la disattivazione dei dati in base al ruolo dell'utente o l'offerta di funzionalità aggiuntive per gli utenti che appartengono a un particolare ruolo.

Le regole di autorizzazione basate sui ruoli con granularità fine possono essere implementate in modo dichiarativo o a livello di codice o tramite una combinazione dei due. Nella sezione successiva verrà illustrato come implementare l'autorizzazione di granularità fine dichiarativa tramite il controllo LoginView. In seguito, si esamineranno le tecniche programmatiche. Prima di poter esaminare l'applicazione di regole di autorizzazione granulari, è necessario creare prima di tutto una pagina la cui funzionalità dipende dal ruolo dell'utente che la visita.

Viene ora creata una pagina in cui sono elencati tutti gli account utente nel sistema in GridView. GridView includerà il nome utente, l'indirizzo di posta elettronica, la data dell'ultimo accesso e i commenti relativi all'utente. Oltre a visualizzare le informazioni di ogni utente, GridView includerà le funzionalità di modifica ed eliminazione. Questa pagina verrà creata inizialmente con la funzionalità di modifica ed eliminazione disponibile per tutti gli utenti. Nelle sezioni "utilizzo del controllo LoginView" e "funzionalità di limitazione a livello di codice" viene illustrato come abilitare o disabilitare queste funzionalità in base al ruolo dell'utente visitato.

> [!NOTE]
> La pagina ASP.NET da compilare usa un controllo GridView per visualizzare gli account utente. Poiché questa serie di esercitazioni è incentrata sull'autenticazione basata su form, sull'autorizzazione, sugli account utente e sui ruoli, non desidero dedicare troppo tempo alla discussione dei lavori interni del controllo GridView. Sebbene in questa esercitazione siano disponibili istruzioni dettagliate per la configurazione di questa pagina, non vengono esaminati i dettagli del motivo per cui sono state apportate determinate scelte o l'effetto sulle proprietà specifiche dell'output sottoposto a rendering. Per un esame approfondito del controllo GridView, vedere la serie di esercitazioni sull' *[uso dei dati in ASP.NET 2,0](../../data-access/index.md)* .

Per iniziare, aprire la pagina `RoleBasedAuthorization.aspx` nella cartella `Roles`. Trascinare un controllo GridView dalla pagina nella finestra di progettazione e impostare la relativa `ID` su `UserGrid`. Nel momento in cui si scriverà il codice che chiama il metodo `Membership.GetAllUsers` e associa l'oggetto `MembershipUserCollection` risultante a GridView. Il `MembershipUserCollection` contiene un oggetto `MembershipUser` per ogni account utente nel sistema. gli oggetti `MembershipUser` hanno proprietà quali `UserName`, `Email`, `LastLoginDate`e così via.

Prima di scrivere il codice che associa gli account utente alla griglia, è necessario definire i campi di GridView. Dallo smart tag di GridView, fare clic sul collegamento "modifica colonne" per aprire la finestra di dialogo campi (vedere la figura 6). Da qui, deselezionare la casella di controllo "genera automaticamente campi" nell'angolo in basso a sinistra. Poiché si desidera che GridView includa la modifica e l'eliminazione delle funzionalità, aggiungere un oggetto CommandField e impostare la relativa `ShowEditButton` e `ShowDeleteButton` proprietà su true. Successivamente, aggiungere quattro campi per visualizzare le proprietà `UserName`, `Email`, `LastLoginDate`e `Comment`. Usare un BoundField per le due proprietà di sola lettura (`UserName` e `LastLoginDate`) e TemplateFields per i due campi modificabili (`Email` e `Comment`).

Il primo BoundField Visualizza la proprietà `UserName`; impostare le proprietà `HeaderText` e `DataField` su "UserName". Questo campo non sarà modificabile, quindi impostare la relativa proprietà `ReadOnly` su true. Configurare la `LastLoginDate` BoundField impostando la relativa `HeaderText` su "ultimo accesso" e la relativa `DataField` su "LastLoginDate". Si consentirà di formattare l'output di questo BoundField in modo che venga visualizzata solo la data (anziché la data e l'ora). A tale scopo, impostare la proprietà `HtmlEncode` di BoundField su false e la relativa proprietà `DataFormatString` su "{0:d}". Impostare anche la proprietà `ReadOnly` su true.

Impostare le proprietà `HeaderText` dei due TemplateFields su "email" e "comment".

[![i campi di GridView possono essere configurati tramite la finestra di dialogo campi](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Figura 6**: i campi di GridView possono essere configurati tramite la finestra di dialogo campi ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image18.png))

A questo punto è necessario definire il `ItemTemplate` e `EditItemTemplate` per i TemplateFields "email" e "comment". Aggiungere un controllo Web Label a ogni `ItemTemplate` s e associare le proprietà `Text` rispettivamente alle proprietà `Email` e `Comment`.

Per il TemplateField "email", aggiungere una casella di testo denominata `Email` al `EditItemTemplate` e associare la relativa proprietà `Text` alla proprietà `Email` usando l'associazione dati bidirezionale. Aggiungere un oggetto RequiredFieldValidator e RegularExpressionValidator al `EditItemTemplate` per assicurarsi che un visitatore che modifica la proprietà del messaggio di posta elettronica abbia immesso un indirizzo di posta elettronica valido. Per il TemplateField "comment", aggiungere una casella di testo a più righe denominata `Comment` al relativo `EditItemTemplate`. Impostare le proprietà `Columns` e `Rows` della casella di testo su 40 e 4 rispettivamente, quindi associare la relativa proprietà `Text` alla proprietà `Comment` mediante l'associazione dati bidirezionale.

Dopo aver configurato questi TemplateFields, il markup dichiarativo dovrebbe essere simile al seguente:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Quando si modifica o si elimina un account utente, è necessario essere a conoscenza del valore della proprietà `UserName` dell'utente. Impostare la proprietà `DataKeyNames` di GridView su "UserName" in modo che queste informazioni siano disponibili tramite la raccolta di `DataKeys` di GridView.

Infine, aggiungere un controllo ValidationSummary alla pagina e impostare la relativa proprietà `ShowMessageBox` su true e la relativa proprietà `ShowSummary` su false. Con queste impostazioni, il ValidationSummary visualizzerà un avviso sul lato client se l'utente tenta di modificare un account utente con un indirizzo di posta elettronica mancante o non valido.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Il markup dichiarativo della pagina è ora completato. L'attività successiva consiste nell'associare il set di account utente a GridView. Aggiungere un metodo denominato `BindUserGrid` alla classe code-behind della pagina `RoleBasedAuthorization.aspx` che associa la `MembershipUserCollection` restituita da `Membership.GetAllUsers` al GridView `UserGrid`. Chiamare questo metodo dal gestore eventi `Page_Load` nella prima pagina visita.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 7, viene visualizzato un controllo GridView che elenca le informazioni relative a ogni account utente nel sistema.

[![GridView UserGrid elenca le informazioni su ogni utente nel sistema](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Figura 7**: la `UserGrid` GridView elenca le informazioni su ogni utente nel sistema ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image21.png))

> [!NOTE]
> Il `UserGrid` GridView elenca tutti gli utenti in un'interfaccia non di paging. Questa semplice interfaccia della griglia non è adatta per scenari in cui sono presenti diverse dozzine di utenti. Un'opzione consiste nel configurare GridView per abilitare il paging. Il metodo `Membership.GetAllUsers` dispone di due overload: uno che non accetta parametri di input e restituisce tutti gli utenti e uno che accetta valori integer per l'indice e le dimensioni della pagina e restituisce solo il subset specificato di utenti. Il secondo overload può essere usato per una pagina in modo più efficiente attraverso gli utenti poiché restituisce solo il subset preciso di account utente piuttosto che *tutti* . Se si dispone di migliaia di account utente, è consigliabile prendere in considerazione un'interfaccia basata su filtri, una che mostra solo gli utenti il cui nome utente inizia con un carattere selezionato, ad esempio. Il [`Membership.FindUsersByName method`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) è ideale per la creazione di un'interfaccia utente basata su filtri. Si esaminerà la creazione di un'interfaccia di questo tipo in un'esercitazione futura.

Il controllo GridView offre funzionalità predefinite di modifica ed eliminazione del supporto quando il controllo è associato a un controllo origine dati configurato correttamente, ad esempio SqlDataSource o ObjectDataSource. Il `UserGrid` GridView, tuttavia, ha i dati associati a livello di programmazione; Pertanto, è necessario scrivere il codice per eseguire queste due attività. In particolare, è necessario creare gestori eventi per gli eventi `RowEditing`, `RowCancelingEdit`, `RowUpdating`e `RowDeleting` di GridView, che vengono generati quando un visitatore fa clic sui pulsanti di modifica, annullamento, aggiornamento o eliminazione di GridView.

Per iniziare, creare i gestori eventi per gli eventi `RowEditing`, `RowCancelingEdit`e `RowUpdating` di GridView, quindi aggiungere il codice seguente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

I gestori eventi `RowEditing` e `RowCancelingEdit` semplicemente impostare la proprietà `EditIndex` di GridView, quindi riassociare l'elenco di account utente alla griglia. La cosa interessante si verifica nel gestore dell'evento `RowUpdating`. Questo gestore dell'evento inizia garantendo che i dati siano validi, quindi acquisisce il valore `UserName` dell'account utente modificato dalla raccolta `DataKeys`. Viene fatto riferimento a livello di codice alle caselle di testo `Email` e `Comment` nei due TemplateFields ' `EditItemTemplate` s. Le proprietà `Text` contengono l'indirizzo di posta elettronica e il commento modificati.

Per aggiornare un account utente tramite l'API di appartenenza, è necessario prima di tutto ottenere le informazioni sull'utente, che vengono eseguite tramite una chiamata a `Membership.GetUser(userName)`. Le proprietà `Email` e `Comment` dell'oggetto `MembershipUser` restituite vengono quindi aggiornate con i valori immessi nelle due caselle di testo dall'interfaccia di modifica. Infine, queste modifiche vengono salvate con una chiamata a [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Il gestore dell'evento `RowUpdating` viene completato ripristinando GridView alla relativa interfaccia di pre-modifica.

Successivamente, creare il gestore dell'evento `RowDeleting`, quindi aggiungere il codice seguente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Il gestore eventi precedente inizia con l'acquisizione del valore `UserName` dalla raccolta di `DataKeys` di GridView. Questo `UserName` valore viene quindi passato al [metodo`DeleteUser`](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)della classe di appartenenza. Il metodo `DeleteUser` Elimina l'account utente dal sistema, inclusi i dati di appartenenza correlati, ad esempio i ruoli a cui appartiene l'utente. Dopo l'eliminazione dell'utente, il `EditIndex` della griglia viene impostato su-1 (se l'utente ha fatto clic su Elimina mentre un'altra riga si trovava in modalità di modifica) e viene chiamato il metodo `BindUserGrid`.

> [!NOTE]
> Il pulsante Elimina non richiede alcun tipo di conferma da parte dell'utente prima di eliminare l'account utente. Si consiglia di aggiungere una forma di conferma utente per ridurre le probabilità che un account venga eliminato accidentalmente. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta di una conferma lato client durante l'eliminazione](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

Verificare che la pagina funzioni come previsto. Dovrebbe essere possibile modificare l'indirizzo di posta elettronica e il commento di qualsiasi utente, nonché eliminare qualsiasi account utente. Poiché la pagina `RoleBasedAuthorization.aspx` è accessibile a tutti gli utenti, anche a visitatori anonimi, può visitare questa pagina e modificare ed eliminare gli account utente. Verrà ora aggiornata questa pagina in modo che solo gli utenti nei ruoli supervisore e amministratore possano modificare l'indirizzo di posta elettronica e il commento di un utente e solo gli amministratori possano eliminare un account utente.

La sezione "utilizzo del controllo LoginView" esamina l'utilizzo del controllo LoginView per visualizzare le istruzioni specifiche per il ruolo dell'utente. Se una persona del ruolo amministratore visita questa pagina, vengono visualizzate le istruzioni su come modificare ed eliminare gli utenti. Se un utente nel ruolo Supervisore raggiunge questa pagina, vengono visualizzate le istruzioni per la modifica degli utenti. Se il visitatore è anonimo o non è presente nel ruolo Supervisore o amministratore, verrà visualizzato un messaggio che informa che non è possibile modificare o eliminare le informazioni sull'account utente. Nella sezione "funzionalità di limitazione a livello di codice" verrà scritto il codice che Mostra o nasconde i pulsanti modifica ed Elimina in base al ruolo dell'utente.

### <a name="using-the-loginview-control"></a>Uso del controllo LoginView

Come illustrato nelle esercitazioni precedenti, il controllo LoginView è utile per visualizzare interfacce diverse per utenti autenticati e anonimi, ma il controllo LoginView può essere usato anche per visualizzare markup diversi in base ai ruoli dell'utente. Viene usato un controllo LoginView per visualizzare istruzioni diverse in base al ruolo dell'utente visitato.

Per iniziare, aggiungere un oggetto LoginView sopra il `UserGrid` GridView. Come illustrato in precedenza, il controllo LoginView include due modelli predefiniti: `AnonymousTemplate` e `LoggedInTemplate`. Immettere un breve messaggio in entrambi i modelli che informa l'utente che non è possibile modificare o eliminare le informazioni utente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Oltre alle `AnonymousTemplate` e `LoggedInTemplate`, il controllo LoginView può includere i *RoleGroup*, che sono modelli specifici del ruolo. Ogni RoleGroup contiene una singola proprietà, `Roles`, che specifica a quali ruoli viene applicato il RoleGroup. La proprietà `Roles` può essere impostata su un singolo ruolo (ad esempio "Administrators") o su un elenco delimitato da virgole di ruoli (ad esempio, "amministratori, supervisori").

Per gestire i RoleGroup, fare clic sul collegamento "modifica RoleGroup" dallo smart tag del controllo per visualizzare l'editor della raccolta RoleGroup. Aggiungere due nuovi RoleGroup. Impostare la proprietà `Roles` del primo RoleGroup su "Administrators" e la seconda su "Supervisors".

[![gestire i modelli specifici del ruolo LoginView mediante l'editor della raccolta RoleGroup](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Figura 8**: gestire i modelli specifici del ruolo di LoginView mediante l'editor della raccolta RoleGroup ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image24.png))

Fare clic su OK per chiudere l'editor della raccolta RoleGroup; Questo Aggiorna il markup dichiarativo di LoginView per includere una sezione di `<RoleGroups>` con un elemento `<asp:RoleGroup>` figlio per ogni RoleGroup definito nell'editor della raccolta RoleGroup. Inoltre, l'elenco a discesa "views" nello smart tag di LoginView, che inizialmente elencava solo i `AnonymousTemplate` e `LoggedInTemplate`, include ora anche i RoleGroup aggiunti.

Modificare i RoleGroup in modo che gli utenti del ruolo supervisori siano in grado di visualizzare le istruzioni su come modificare gli account utente, mentre gli utenti del ruolo Administrators vengono visualizzate le istruzioni per la modifica e l'eliminazione. Dopo avere apportato queste modifiche, il markup dichiarativo di LoginView dovrebbe essere simile al seguente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Dopo aver apportato queste modifiche, salvare la pagina e visitarla tramite un browser. Per prima cosa, visitare la pagina come utente anonimo. Verrà visualizzato il messaggio "l'utente non è connesso al sistema. Non è quindi possibile modificare o eliminare le informazioni utente ". Eseguire quindi l'accesso come utente autenticato, ma non nel ruolo Supervisore e amministratore. Questa volta dovrebbe essere visualizzato il messaggio "l'utente non è un membro del ruolo di supervisore o amministratore. Non è quindi possibile modificare o eliminare le informazioni utente ".

Accedere quindi come utente membro del ruolo Supervisore. Questa volta dovrebbe essere visualizzato il messaggio specifico del ruolo Supervisors (vedere la figura 9). Se si accede come utente con il ruolo di amministratore, verrà visualizzato il messaggio specifico del ruolo Administrators (vedere la figura 10).

[![Bruce viene visualizzato il messaggio specifico del ruolo Supervisors](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Figura 9**: Bruce viene visualizzato il messaggio specifico del ruolo Supervisors ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image27.png))

[![Tito viene visualizzato il messaggio specifico del ruolo Administrators](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Figura 10**: Tito viene visualizzato il messaggio specifico del ruolo Administrators ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image30.png))

Mentre gli screenshot delle figure 9 e 10 mostrano, il LoginView esegue solo il rendering di un modello, anche se si applicano più modelli. Bruce e Tito sono entrambi utenti connessi, ma l'oggetto LoginView esegue il rendering solo del oggetto RoleGroup corrispondente e non della `LoggedInTemplate`. Inoltre, Tito appartiene ai ruoli di amministratore e supervisore, ma il controllo LoginView esegue il rendering del modello specifico del ruolo Administrators anziché dei supervisori uno.

Nella figura 11 viene illustrato il flusso di lavoro utilizzato dal controllo LoginView per determinare il modello di cui eseguire il rendering. Si noti che se è stato specificato più di un oggetto RoleGroup, il modello LoginView esegue il rendering del *primo* RoleGroup che corrisponde a. In altre parole, se i supervisore è stato inserito come primo RoleGroup e amministratori come secondo, quando Tito visitava questa pagina avrebbe visualizzato il messaggio supervisore.

[![il flusso di lavoro del controllo LoginView per determinare il modello di cui eseguire il rendering](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Figura 11**: flusso di lavoro del controllo LoginView per determinare il modello di cui eseguire il rendering ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitazione delle funzionalità a livello di codice

Mentre il controllo LoginView Visualizza istruzioni diverse in base al ruolo dell'utente che visita la pagina, i pulsanti modifica e Annulla rimangono visibili a tutti. È necessario nascondere a livello di codice i pulsanti modifica ed Elimina per i visitatori anonimi e gli utenti che non hanno né il ruolo di supervisore né amministratore. È necessario nascondere il pulsante Elimina per tutti gli utenti che non sono amministratori. Per eseguire questa operazione, verrà scritto un frammento di codice che fa riferimento a livello di codice agli LinkButton di modifica ed eliminazione di CommandField e imposta le proprietà `Visible` su `false`, se necessario.

Il modo più semplice per fare riferimento a controlli a livello di codice in un oggetto CommandField consiste nel convertirlo prima in un modello. A tale scopo, fare clic sul collegamento "modifica colonne" dallo smart tag di GridView, selezionare CommandField dall'elenco dei campi correnti, quindi fare clic sul collegamento "Converti questo campo in un TemplateField". In questo modo, CommandField viene trasformato in un TemplateField con una `ItemTemplate` e `EditItemTemplate`. Il `ItemTemplate` contiene le LinkButton di modifica ed eliminazione mentre la `EditItemTemplate` ospita le LinkButton di aggiornamento e di annullamento.

[![convertire CommandField in TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Figura 12**: convertire CommandField in TemplateField ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image36.png))

Aggiornare gli LinkButton di modifica ed eliminazione nell'`ItemTemplate`, impostando le relative proprietà `ID` sui valori rispettivamente di `EditButton` e `DeleteButton`.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Ogni volta che i dati sono associati a GridView, GridView enumera i record nella relativa proprietà `DataSource` e genera un oggetto `GridViewRow` corrispondente. Quando viene creato ogni `GridViewRow` oggetto, viene generato l'evento `RowCreated`. Per nascondere i pulsanti modifica ed Elimina per utenti non autorizzati, è necessario creare un gestore eventi per questo evento e fare riferimento a a livello di codice agli LinkButton di modifica ed eliminazione, impostando di conseguenza le proprietà `Visible`.

Creare un gestore eventi `RowCreated` evento, quindi aggiungere il codice seguente:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Tenere presente che l'evento `RowCreated` viene attivato per *tutte* le righe GridView, inclusa l'intestazione, il piè di pagina, l'interfaccia cercapersone e così via. Si vuole fare riferimento solo a livello di codice ai controlli LinkButton di modifica ed eliminazione se si tratta di una riga di dati non in modalità di modifica (poiché la riga in modalità di modifica contiene pulsanti Aggiorna e Annulla anziché modifica ed Elimina). Questo controllo è gestito dall'istruzione `if`.

Se si tratta di una riga di dati che non si trova in modalità di modifica, viene fatto riferimento ai controlli LinkButton di modifica ed eliminazione e le relative proprietà `Visible` vengono impostate in base ai valori booleani restituiti dal metodo `IsInRole(roleName)` dell'oggetto `User`. L'oggetto utente fa riferimento all'entità creata dal `RoleManagerModule`; di conseguenza, il metodo `IsInRole(roleName)` usa l'API Roles per determinare se il visitatore corrente appartiene a *roleName*.

> [!NOTE]
> È possibile che sia stata utilizzata direttamente la classe Roles, sostituendo la chiamata a `User.IsInRole(roleName)` con una chiamata al [metodo`Roles.IsUserInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). In questo esempio ho deciso di usare il metodo di `IsInRole(roleName)` dell'oggetto Principal perché è più efficiente rispetto all'uso diretto dell'API Roles. In precedenza in questa esercitazione è stato configurato Gestione ruoli per memorizzare nella cache i ruoli dell'utente in un cookie. I dati dei cookie memorizzati nella cache vengono utilizzati solo quando viene chiamato il metodo di `IsInRole(roleName)` dell'entità; le chiamate dirette all'API Roles coinvolgono sempre un viaggio nell'archivio dei ruoli. Anche se i ruoli non vengono memorizzati nella cache in un cookie, la chiamata al metodo di `IsInRole(roleName)` dell'oggetto Principal è in genere più efficiente perché, quando viene chiamata per la prima volta durante una richiesta, memorizza nella cache i risultati. L'API Roles, d'altra parte, non esegue alcuna operazione di memorizzazione nella cache. Poiché l'evento `RowCreated` viene generato una volta per ogni riga in GridView, l'uso di `User.IsInRole(roleName)` comporta solo un viaggio nell'archivio dei ruoli, mentre `Roles.IsUserInRole(roleName)` richiede *N* viaggi, dove *n* è il numero di account utente visualizzati nella griglia.

La proprietà `Visible` del pulsante modifica è impostata su `true` se l'utente che visita questa pagina si trova nel ruolo amministratori o supervisori; in caso contrario, è impostato su `false`. La proprietà `Visible` del pulsante Elimina è impostata su `true` solo se l'utente è nel ruolo Administrators.

Testare questa pagina tramite un browser. Se si visita la pagina come visitatore anonimo o come utente che non è né un supervisore né un amministratore, CommandField è vuoto; esiste ancora, ma come sottile scheggia senza i pulsanti modifica o Elimina.

> [!NOTE]
> È possibile nascondere completamente CommandField quando un non supervisore e non amministratore visita la pagina. Lo lascio come esercizio per il lettore.

[![i pulsanti modifica ed Elimina sono nascosti per i supervisori e non amministratori](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Figura 13**: i pulsanti modifica ed Elimina sono nascosti per i supervisori e non amministratori ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image39.png))

Se un utente che appartiene al ruolo Supervisore (ma non al ruolo Administrators) visita, vedrà solo il pulsante modifica.

[![mentre il pulsante modifica è disponibile per i supervisori, il pulsante Elimina è nascosto](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Figura 14**: mentre il pulsante modifica è disponibile per i supervisori, il pulsante Elimina è nascosto ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image42.png))

Se un amministratore visita, può accedere ai pulsanti modifica ed Elimina.

[![i pulsanti modifica ed Elimina sono disponibili solo per gli amministratori](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Figura 15**: i pulsanti modifica ed Elimina sono disponibili solo per gli amministratori ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Passaggio 3: applicazione di regole di autorizzazione basate sui ruoli a classi e metodi

Nel passaggio 2 sono state limitate le funzionalità di modifica agli utenti nei ruoli supervisori e amministratori e vengono eliminate solo le funzionalità per gli amministratori. Questa operazione è stata eseguita nascondendo gli elementi dell'interfaccia utente associati per utenti non autorizzati tramite tecniche di programmazione. Tali misure non garantiscono che un utente non autorizzato non sarà in grado di eseguire un'azione privilegiata. Potrebbero essere presenti elementi dell'interfaccia utente che vengono aggiunti in un secondo momento o che non sono stati nascosti per utenti non autorizzati. O un pirata informatico può individuare un altro modo per ottenere la pagina ASP.NET per l'esecuzione del metodo desiderato.

Un modo semplice per evitare che un utente non autorizzato possa accedere a una particolare parte della funzionalità consiste nel decorare tale classe o metodo con l' [attributo`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il Runtime .NET usa una classe o esegue uno dei relativi metodi, verifica che il contesto di sicurezza corrente disponga dell'autorizzazione. L'attributo `PrincipalPermission` fornisce un meccanismo tramite il quale è possibile definire queste regole.

Nell' <a id="_msoanchor_9"> </a>esercitazione sull' [*autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) è stato esaminato l'uso dell'attributo `PrincipalPermission`. In particolare, è stato illustrato come decorare il `SelectedIndexChanged` di GridView e `RowDeleting` gestore eventi in modo che possano essere eseguiti solo dagli utenti autenticati e da Tito, rispettivamente. L'attributo `PrincipalPermission` funziona anche con i ruoli.

Viene ora illustrato l'utilizzo dell'attributo `PrincipalPermission` sui `RowUpdating` e `RowDeleting` gestori di eventi di GridView per impedire l'esecuzione di utenti non autorizzati. È sufficiente aggiungere l'attributo appropriato in cima a ogni definizione di funzione:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

L'attributo per il gestore dell'evento `RowUpdating` impone che solo gli utenti nei ruoli amministratori o supervisori possano eseguire il gestore eventi, in cui l'attributo nel gestore dell'evento `RowDeleting` limita l'esecuzione agli utenti del ruolo Administrators.

> [!NOTE]
> L'attributo `PrincipalPermission` viene rappresentato come una classe nello spazio dei nomi `System.Security.Permissions`. Assicurarsi di aggiungere un'istruzione `using System.Security.Permissions` nella parte superiore del file di classe code-behind per importare questo spazio dei nomi.

Se, in qualche modo, un non amministratore tenta di eseguire il gestore dell'evento `RowDeleting` o se un non supervisore o non amministratore tenta di eseguire il gestore dell'evento `RowUpdating`, il Runtime .NET genererà un `SecurityException`.

[![se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Figura 16**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'`SecurityException` ([fare clic per visualizzare l'immagine con dimensioni complete](role-based-authorization-cs/_static/image48.png))

Oltre alle pagine ASP.NET, molte applicazioni hanno anche un'architettura che include diversi livelli, ad esempio la logica di business e i livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e offrono classi e metodi per l'esecuzione di funzionalità relative alla logica di business e ai dati. L'attributo `PrincipalPermission` è utile per applicare le regole di autorizzazione anche a questi livelli.

Per altre informazioni sull'uso dell'attributo `PrincipalPermission` per definire le regole di autorizzazione per le classi e i metodi, vedere la voce del Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/) [aggiunta di regole di autorizzazione ai livelli business e dati usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come specificare regole di autorizzazione granularità grossolana e fine in base ai ruoli dell'utente. ASP. La funzionalità di autorizzazione URL di NET consente a uno sviluppatore di pagine di specificare a quali identità è consentito o negato l'accesso alle pagine. Come illustrato nell'esercitazione relativa all' <a id="_msoanchor_10"> </a> [*autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) , le regole di autorizzazione degli URL possono essere applicate a livello di utente. Possono anche essere applicati in base al ruolo, come illustrato nel passaggio 1 di questa esercitazione.

Le regole di autorizzazione granularità fine possono essere applicate in modo dichiarativo o a livello di codice. Nel passaggio 2 è stato esaminato l'uso della funzionalità RoleGroup del controllo LoginView per eseguire il rendering di un output diverso in base ai ruoli dell'utente visitato. Sono stati esaminati anche i modi per determinare a livello di codice se un utente appartiene a un ruolo specifico e come modificare di conseguenza la funzionalità della pagina.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Aggiunta di regole di autorizzazione ai livelli business e dati mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2.0: utilizzo dei ruoli](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Elenco di domande di sicurezza per ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentazione tecnica per l'elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione includono tali e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](assigning-roles-to-users-cs.md)
> [Successivo](creating-and-managing-roles-vb.md)
