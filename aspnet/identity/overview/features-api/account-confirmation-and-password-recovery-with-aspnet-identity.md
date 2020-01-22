---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Conferma dell'account & il ripristino della passwordC#-ASP.NET Identity ()-ASP.NET 4. x
author: HaoK
description: Prima di eseguire questa esercitazione, è necessario completare la creazione di un'app Web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password. Questa esercitazione...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b2c88280df39aa81d60f9508910e8fe5d6db6b8
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519115"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Conferma dell'account e recupero della password conC#ASP.NET Identity ()

> Prima di eseguire questa esercitazione, è necessario completare [la creazione di un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Questa esercitazione contiene ulteriori dettagli e Mostra come configurare la posta elettronica per la conferma dell'account locale e consentire agli utenti di reimpostare la password dimenticata in ASP.NET Identity.

Un account utente locale richiede all'utente di creare una password per l'account e la password viene archiviata (in modo sicuro) nell'app Web. ASP.NET Identity supporta anche gli account di social networking, che non richiedono che l'utente crei una password per l'app. Gli [account di social](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) networking usano una terza parte (ad esempio Google, Twitter, Facebook o Microsoft) per autenticare gli utenti. Questo argomento illustra quanto segue:

- [Creare un'app MVC ASP.NET](#createMvc) ed esplorare ASP.NET Identity funzionalità.
- [Compilare l'esempio di identità](#build)
- [Configurare la conferma della posta elettronica](#email)

I nuovi utenti registrano l'alias di posta elettronica, che crea un account locale.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Selezionando il pulsante Register, viene inviato un messaggio di posta elettronica di conferma contenente un token di convalida all'indirizzo di posta elettronica.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

All'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Selezionare il collegamento per confermare l'account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Ripristino/reimpostazione della password

Gli utenti locali che dimenticano la password possono avere un token di sicurezza inviato al proprio account di posta elettronica, consentendo loro di reimpostare la password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
L'utente riceverà presto un messaggio di posta elettronica con un collegamento che consente di reimpostare la password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Se si seleziona il collegamento, verrà visualizzata la pagina Reimposta.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Se si seleziona il pulsante **Reimposta** , viene confermata la reimpostazione della password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Creare un'app Web ASP.NET

Per iniziare, installare ed eseguire [Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. I Web Form supportano anche ASP.NET Identity, quindi è possibile seguire una procedura simile in un'app Web Form.
2. Modificare l'autenticazione per **singoli account utente**.
3. Eseguire l'app, selezionare il collegamento **Register** e registrare un utente. A questo punto, l'unica convalida sul messaggio di posta elettronica è l'attributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
4. In Esplora server passare a **Data Connections\DefaultConnection\Tables\AspNetUsers**, fare clic con il pulsante destro del mouse e selezionare **Apri definizione tabella**.

    Nella figura seguente viene illustrato lo schema `AspNetUsers`:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Fare clic con il pulsante destro del mouse sulla tabella **AspNetUsers** e scegliere **Mostra dati tabella**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   A questo punto, il messaggio di posta elettronica non è stato confermato.

L'archivio dati predefinito per ASP.NET Identity è Entity Framework, ma è possibile configurarlo per l'uso di altri archivi dati e per aggiungere campi aggiuntivi. Vedere la sezione [risorse aggiuntive](#addRes) alla fine di questa esercitazione.

La [classe Startup OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) viene chiamata quando l'app viene avviata e richiama il metodo `ConfigureAuth` nell' *app\_Start\Startup.auth.cs*, che configura la pipeline OWIN e inizializza ASP.NET Identity. Esaminare il metodo `ConfigureAuth`. Ogni chiamata di `CreatePerOwinContext` registra un callback, salvato nella `OwinContext`, che verrà chiamato una volta per ogni richiesta per creare un'istanza del tipo specificato. È possibile impostare un punto di rottura nel costruttore e `Create` metodo di ogni tipo (`ApplicationDbContext, ApplicationUserManager`) e verificare che vengano chiamati a ogni richiesta. Un'istanza di `ApplicationDbContext` e `ApplicationUserManager` viene archiviata nel contesto OWIN, a cui è possibile accedere in tutta l'applicazione. ASP.NET Identity hook nella pipeline OWIN tramite il middleware dei cookie. Per ulteriori informazioni, vedere [la pagina relativa alla gestione della durata per richiesta per la classe usermanager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Quando si modifica il profilo di sicurezza, viene generato e archiviato un nuovo indicatore di sicurezza nel campo `SecurityStamp` della tabella *AspNetUsers* . Si noti che il campo `SecurityStamp` è diverso dal cookie di sicurezza. Il cookie di sicurezza non è archiviato nella tabella `AspNetUsers` (o altrove nel database di identità). Il token del cookie di sicurezza è autofirmato tramite [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con le informazioni relative a `UserId, SecurityStamp` e ora di scadenza.

Il middleware dei cookie controlla il cookie per ogni richiesta. Il metodo `SecurityStampValidator` nella classe `Startup` raggiunge il database e controlla periodicamente il timbro di sicurezza, come specificato con il `validateInterval`. Questo problema si verifica solo ogni 30 minuti (in questo esempio), a meno che non si modifichi il profilo di sicurezza. È stato scelto l'intervallo di 30 minuti per ridurre al minimo i viaggi al database. Per ulteriori informazioni, vedere [l'esercitazione sull'autenticazione a due fattori](index.md) .

Per i commenti nel codice, il metodo `UseCookieAuthentication` supporta l'autenticazione dei cookie. Il campo `SecurityStamp` e il codice associato forniscono un livello di sicurezza aggiuntivo per l'app. quando si cambia la password, si verrà disconnessi dal browser con cui si è effettuato l'accesso. Il metodo `SecurityStampValidator.OnValidateIdentity` consente all'app di convalidare il token di sicurezza quando l'utente esegue l'accesso, che viene usato quando si modifica una password o si usa l'account di accesso esterno. Questa operazione è necessaria per garantire che tutti i token (cookie) generati con la vecchia password vengano invalidati. Nel progetto di esempio, se si modifica la password degli utenti, viene generato un nuovo token per l'utente, tutti i token precedenti vengono invalidati e il campo `SecurityStamp` viene aggiornato.

Il sistema di identità consente di configurare l'app in modo che quando viene modificato il profilo di sicurezza degli utenti (ad esempio, quando l'utente modifica la password o modifica l'accesso associato, ad esempio da Facebook, Google, account Microsoft e così via), l'utente viene disconnesso da tutti istanze del browser. Ad esempio, l'immagine seguente mostra l'app di [esempio Single Sign](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) -out, che consente all'utente di disconnettersi da tutte le istanze del browser (in questo caso, IE, Firefox e Chrome) selezionando un pulsante. In alternativa, l'esempio consente di disconnettersi solo da una specifica istanza del browser.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

L'app di [esempio Single Sign](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) -out mostra come ASP.NET Identity consente di rigenerare il token di sicurezza. Questa operazione è necessaria per garantire che tutti i token (cookie) generati con la vecchia password vengano invalidati. Questa funzionalità offre un livello aggiuntivo di sicurezza per l'applicazione. Quando si cambia la password, si viene disconnessi da dove è stato effettuato l'accesso a questa applicazione.

Il file *Start\IdentityConfig.cs dell'App\_* contiene le classi `ApplicationUserManager`, `EmailService` e `SmsService`. Le classi `EmailService` e `SmsService` implementano ciascuna l'interfaccia `IIdentityMessageService`, quindi sono disponibili metodi comuni in ogni classe per la configurazione di posta elettronica e SMS. Sebbene questa esercitazione mostri solo come aggiungere notifiche tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare messaggi di posta elettronica usando SMTP e altri meccanismi.

La classe `Startup` contiene anche una piastra calda per aggiungere account di accesso ai social network (Facebook, Twitter e così via). per altre informazioni, vedere l' [app MVC 5 dell'esercitazione con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) .

Esaminare la classe `ApplicationUserManager` contenente le informazioni sull'identità degli utenti e configurare le funzionalità seguenti:

- Requisiti di complessità della password.
- Blocco utente (tentativi e tempo).
- Autenticazione a due fattori (2FA). Illustrerò 2FA e SMS in un'altra esercitazione.
- Associazione dei servizi di posta elettronica e SMS. (Si tratterà di un SMS in un'altra esercitazione).

La classe `ApplicationUserManager` deriva dalla classe `UserManager<ApplicationUser>` generica. `ApplicationUser` deriva da [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva dalla classe `IdentityUser` generica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Le proprietà precedenti coincidono con le proprietà nella tabella `AspNetUsers`, illustrata in precedenza.

Gli argomenti generici in `IUser` consentono di derivare una classe utilizzando tipi diversi per la chiave primaria. Vedere l'esempio [ChangePK](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) che Mostra come modificare la chiave primaria da stringa a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) viene definito in *Models\IdentityModels.cs* come:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Il codice evidenziato sopra genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity e l'autenticazione dei cookie OWIN sono basate su attestazioni, pertanto il Framework richiede che l'app generi una `ClaimsIdentity` per l'utente. `ClaimsIdentity` dispone di informazioni su tutte le attestazioni per l'utente, ad esempio il nome dell'utente, l'età e i ruoli a cui appartiene l'utente. In questa fase è anche possibile aggiungere altre attestazioni per l'utente.

Il metodo `AuthenticationManager.SignIn` OWIN passa l'`ClaimsIdentity` e firma l'utente:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

L' [app MVC 5 con l'accesso a Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Mostra come aggiungere altre proprietà alla classe `ApplicationUser`.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare che il messaggio di posta elettronica di un nuovo utente venga registrato per verificare che non stiano usando un altro utente, ovvero che non sono stati registrati con il messaggio di posta elettronica di un altro utente. Si supponga di avere un forum di discussione che si vuole impedire che `"bob@example.com"` venga registrato come `"joe@contoso.com"`. Senza la conferma della posta elettronica, `"joe@contoso.com"` possibile ricevere messaggi di posta elettronica indesiderati dall'app. Supponiamo che Bob sia stato accidentalmente registrato come `"bib@example.com"` e non sia stato notato, non sarebbe in grado di usare il ripristino della password perché l'app non ha la posta elettronica corretta. La conferma tramite posta elettronica offre solo una protezione limitata dai bot e non fornisce protezione da spammer determinati, ma ha molti alias di posta elettronica di lavoro che possono usare per la registrazione. Nell'esempio seguente, l'utente non sarà in grado di modificare la password fino a quando l'account non è stato confermato (selezionando un collegamento di conferma ricevuto per l'account di posta elettronica con cui è stato registrato.) È possibile applicare questo flusso di lavoro ad altri scenari, ad esempio inviando un collegamento per confermare e reimpostare la password per i nuovi account creati dall'amministratore, inviando all'utente un messaggio di posta elettronica al momento della modifica del profilo e così via. In genere si desidera impedire ai nuovi utenti di inviare dati al sito Web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Compilare un esempio più completo

In questa sezione si userà NuGet per scaricare un esempio più completo che verrà usato con.

1. Creare un nuovo progetto Web ASP.NET ***vuoto*** .
2. Nella console di gestione pacchetti immettere i comandi seguenti: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   In questa esercitazione verrà usato [SendGrid](http://sendgrid.com/) per inviare messaggi di posta elettronica. Il pacchetto di `Identity.Samples` installa il codice che verrà usato.
3. Impostare il [progetto per l'utilizzo di SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Testare la creazione dell'account locale eseguendo l'app, selezionando il collegamento **Register** e pubblicando il modulo di registrazione.
5. Selezionare il collegamento demo email che simula la conferma della posta elettronica.
6. Rimuovere il codice di conferma del collegamento di posta elettronica demo dall'esempio (il codice `ViewBag.Link` nel controller account. Vedere i metodi di azione `DisplayEmail` e `ForgotPasswordConfirmation` e le visualizzazioni Razor).

> [!WARNING]
> Se si modifica una delle impostazioni di sicurezza in questo esempio, le app Productions dovranno sottoporsi a un controllo di sicurezza che chiama in modo esplicito le modifiche apportate.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Esaminare il codice in app\_Start\IdentityConfig.cs

Nell'esempio viene illustrato come creare un account e aggiungerlo al ruolo di *amministratore* . È necessario sostituire il messaggio di posta elettronica nell'esempio con il messaggio di posta elettronica che verrà usato per l'account amministratore. Il modo più semplice per creare un account amministratore è a livello di codice nel metodo `Seed`. Ci auguriamo che in futuro sia presente uno strumento che consente di creare e amministrare utenti e ruoli. Il codice di esempio consente di creare e gestire utenti e ruoli, ma è necessario innanzitutto disporre di un account amministratori per eseguire le pagine ruoli e Amministratore utenti. In questo esempio, l'account amministratore viene creato quando viene creato il seeding del database.

Modificare la password e modificare il nome in un account in cui è possibile ricevere notifiche tramite posta elettronica.

> [!WARNING]
> Sicurezza: non archiviare mai dati sensibili nel codice sorgente.

Come indicato in precedenza, la chiamata `app.CreatePerOwinContext` nella classe Startup aggiunge i callback al metodo `Create` delle classi Content del database App, gestione utenti e gestione ruoli. La pipeline OWIN chiama il metodo `Create` su queste classi per ogni richiesta e archivia il contesto per ogni classe. Il controller di account espone il gestore utenti dal contesto HTTP (che contiene il contesto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Quando un utente registra un account locale, viene chiamato il metodo `HTTP Post Register`:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Il codice precedente usa i dati del modello per creare un nuovo account utente usando il messaggio di posta elettronica e la password immessi. Se l'alias di posta elettronica è presente nell'archivio dati, la creazione dell'account ha esito negativo e il modulo viene nuovamente visualizzato. Il metodo `GenerateEmailConfirmationTokenAsync` crea un token di conferma sicuro e lo archivia nell'archivio dati ASP.NET Identity. Il metodo [URL. Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) crea un collegamento che contiene il `UserId` e il token di conferma. Questo collegamento viene quindi inviato tramite posta elettronica all'utente, l'utente può selezionare il collegamento nell'app di posta elettronica per confermare l'account.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurare la conferma della posta elettronica

Vai alla [pagina di iscrizione di Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registrati per ottenere un account gratuito. Aggiungere codice simile al seguente per configurare SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> I client di posta elettronica accettano spesso solo SMS (nessun HTML). È necessario fornire il messaggio in formato testo e HTML. Nell'esempio precedente di SendGrid questa operazione viene eseguita con il `myMessage.Text` e `myMessage.Html` codice illustrato in precedenza.

Il codice seguente illustra come inviare messaggi di posta elettronica usando la classe [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) in cui `message.Body` restituisce solo il collegamento.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sicurezza: non archiviare mai dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviati in appSetting. In Azure è possibile archiviare in modo sicuro questi valori nella scheda **[Configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** della portale di Azure. Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

Immettere le credenziali di SendGrid, eseguire l'app, registrarsi con un alias di posta elettronica può selezionare il collegamento confirm (conferma) nel messaggio di posta elettronica. Per informazioni su come eseguire questa operazione con l'account di posta elettronica di [Outlook.com](http://outlook.com) , vedere la pagina relativa alla configurazione [ C# SMTP di John atten per l'host SMTP Outlook.com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) e alla sua[ASP.NET Identity 2,0: impostazione della convalida dell'account e](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) dei post di autorizzazione a due fattori.

Quando un utente seleziona il pulsante **Register** , viene inviato un messaggio di posta elettronica di conferma contenente un token di convalida al proprio indirizzo di posta elettronica.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

All'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Esaminare il codice

Nel codice seguente viene illustrato il metodo `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Il metodo ha esito negativo automaticamente se l'indirizzo di posta elettronica dell'utente non è stato confermato. Se è stato pubblicato un errore per un indirizzo di posta elettronica non valido, gli utenti malintenzionati potrebbero usare tali informazioni per trovare un ID utente valido (alias di posta elettronica) per attaccare.

Il codice seguente illustra il metodo `ConfirmEmail` nel controller di account che viene chiamato quando l'utente seleziona il collegamento di conferma nel messaggio di posta elettronica inviato:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Una volta usato, il token della password dimenticata è invalidato. La seguente modifica del codice nel metodo `Create` (nel file *App\_Start\IdentityConfig.cs* ) imposta i token in modo che scadano tra 3 ore.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con il codice precedente, la password dimenticata e i token di conferma della posta elettronica scadranno tra 3 ore. Il `TokenLifespan` predefinito è un giorno.

Il codice seguente illustra il metodo di conferma della posta elettronica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Per rendere l'app più sicura, ASP.NET Identity supporta l'autenticazione a due fattori (2FA). Vedere [ASP.NET Identity 2,0: impostazione della convalida dell'account e dell'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) di John atten. Sebbene sia possibile impostare il blocco degli account per gli errori dei tentativi di accesso alla password, questo approccio rende l'accesso vulnerabile ai blocchi [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Si consiglia di usare il blocco degli account solo con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- L' [app MVC 5 con l'accesso a Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Mostra anche come aggiungere informazioni sul profilo alla tabella users.
- [ASP.NET MVC and Identity 2,0: informazioni sulle nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) di John atten.
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) di Rastogi.
