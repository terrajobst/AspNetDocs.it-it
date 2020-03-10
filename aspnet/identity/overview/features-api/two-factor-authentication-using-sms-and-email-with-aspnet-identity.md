---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity-ASP.NET 4. x
author: HaoK
description: In questa esercitazione verrà illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica. Questo articolo è stato scritto da Rick Anderson (@RickAndMSFT), Pr...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616683"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity

di [Hao Kung](https://github.com/HaoK), Manuel [Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> In questa esercitazione verrà illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica.
> 
> Questo articolo è stato scritto da Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. L'esempio NuGet è stato scritto principalmente da Hao Kung.

Questo argomento illustra quanto segue:

- [Compilazione dell'esempio di identità](#build)
- [Configurare SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Come registrare un provider di autenticazione a due fattori](#reg)
- [Combinare account di accesso di social networking e locali](#combine)
- [Blocco degli account da attacchi di forza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilazione dell'esempio di identità

In questa sezione si userà NuGet per scaricare un esempio che verrà usato con. Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.

> [!NOTE]
> Avviso: per completare questa esercitazione, è necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) .

1. Creare un nuovo progetto Web ASP.NET ***vuoto*** .
2. Nella console di gestione pacchetti immettere i comandi seguenti:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In questa esercitazione si userà [SendGrid](http://sendgrid.com/) per inviare messaggi di posta elettronica e [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) per SMS. Il pacchetto di `Identity.Samples` installa il codice che verrà usato.
3. Impostare il [progetto per l'utilizzo di SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Facoltativo*: seguire le istruzioni riportate nell' [esercitazione di conferma tramite posta elettronica](account-confirmation-and-password-recovery-with-aspnet-identity.md) per associare SendGrid, quindi eseguire l'app e registrare un account di posta elettronica.
5. *Facoltativo:* Rimuovere il codice di conferma del collegamento di posta elettronica demo dall'esempio (il codice `ViewBag.Link` nel controller account. Vedere i metodi di azione `DisplayEmail` e `ForgotPasswordConfirmation` e le visualizzazioni Razor).
6. *Facoltativo:* Rimuovere il codice `ViewBag.Status` dai controller gestione e account e dalle visualizzazioni Razor *Views\Account\VerifyCode.cshtml* e *Views\Manage\VerifyPhoneNumber.cshtml* . In alternativa, è possibile usare la visualizzazione `ViewBag.Status` per verificare il funzionamento di questa app localmente senza dover associare e inviare messaggi di posta elettronica e SMS.

> [!NOTE]
> Avviso: se si modifica una delle impostazioni di sicurezza in questo esempio, le app Productions dovranno sottoporsi a un controllo di sicurezza che richiama in modo esplicito le modifiche apportate.

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurare SMS per l'autenticazione a due fattori

Questa esercitazione fornisce istruzioni per l'uso di Twilio o ASPSMS, ma è possibile usare qualsiasi altro provider SMS.

1. **Creazione di un account utente con un provider SMS**  
  
   Creare un account [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Installazione di pacchetti aggiuntivi o aggiunta di riferimenti al servizio**  
  
   Twilio:  
   Nella Console di Gestione pacchetti immettere il comando seguente:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   È necessario aggiungere il seguente riferimento al servizio:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Individuazione delle credenziali utente del provider SMS**  
  
   Twilio:  
   Dalla scheda **Dashboard** dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione**.  
  
   ASPSMS:  
   Dalle impostazioni dell'account, passare a **userKey** e copiarlo insieme alla **password**definita autonomamente.  
  
   Questi valori vengono archiviati in un secondo momento nelle variabili `SMSAccountIdentification` e `SMSAccountPassword`.
4. **Specifica di SenderID/originator**  
  
   Twilio:  
   Dalla scheda **numeri** copiare il numero di telefono Twilio.  
  
   ASPSMS:  
   Nel menu **Sblocca autori** sbloccare uno o più autori o scegliere un originatore alfanumerico (non supportato da tutte le reti).  
  
   Questo valore verrà archiviato in un secondo momento nella variabile `SMSAccountFrom`.
5. **Trasferimento delle credenziali del provider SMS nell'app**  
  
   Rendere disponibili le credenziali e il numero di telefono del mittente per l'app:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sicurezza: non archiviare mai dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice riportato sopra per semplificare l'esempio. Vedere l'ASP.NET MVC di Jon atten [: Mantieni le impostazioni private dal controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementazione del trasferimento dati al provider SMS**  
  
   Configurare la classe `SmsService` nel file *App\_Start\IdentityConfig.cs* .  
  
   A seconda del provider SMS utilizzato, attivare la sezione **Twilio** o **ASPSMS** : 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Eseguire l'app e accedere con l'account registrato in precedenza.
8. Fare clic sull'ID utente, che attiva il metodo di azione `Index` in `Manage` controller.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Fare clic su Aggiungi.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. In pochi secondi si riceverà un messaggio di testo con il codice di verifica. Immetterla e premere **invio**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La visualizzazione gestione mostra che è stato aggiunto il numero di telefono.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Esaminare il codice

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Il metodo di azione `Index` nel controller `Manage` imposta il messaggio di stato in base all'azione precedente e fornisce collegamenti per modificare la password locale o aggiungere un account locale. Il metodo `Index` Visualizza anche lo stato o il numero di telefono 2FA, gli account di accesso esterni, 2FA abilitati e ricorda il metodo 2FA per il browser (descritto più avanti). Facendo clic sull'ID utente (indirizzo di posta elettronica) nella barra del titolo non viene passato alcun messaggio. Facendo clic sul **numero di telefono: Rimuovi** collegamento passa `Message=RemovePhoneSuccess` come stringa di query.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Il metodo di azione `AddPhoneNumber` Visualizza una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Facendo clic sul pulsante **Invia codice di verifica** il numero di telefono viene inserito nel metodo di azione HTTP post `AddPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Il metodo `GenerateChangePhoneNumberTokenAsync` genera il token di sicurezza che verrà impostato nel messaggio SMS. Se il servizio SMS è stato configurato, il token viene inviato come stringa &quot;il codice di sicurezza è &lt;token&gt;&quot;. Il metodo `SmsService.SendAsync` a viene chiamato in modo asincrono, quindi l'app viene reindirizzata al metodo di azione `VerifyPhoneNumber`, che visualizza la finestra di dialogo seguente, in cui è possibile immettere il codice di verifica.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Una volta immesso il codice e fatto clic su Submit, il codice viene inserito nel metodo di azione HTTP POST `VerifyPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Il metodo `ChangePhoneNumberAsync` controlla il codice di sicurezza inviato. Se il codice è corretto, il numero di telefono viene aggiunto al campo `PhoneNumber` della tabella `AspNetUsers`. Se la chiamata ha esito positivo, viene chiamato il metodo `SignInAsync`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Il parametro `isPersistent` imposta se la sessione di autenticazione viene mantenute in più richieste.

Quando si modifica il profilo di sicurezza, viene generato e archiviato un nuovo indicatore di sicurezza nel campo `SecurityStamp` della tabella *AspNetUsers* . Si noti che il campo `SecurityStamp` è diverso dal cookie di sicurezza. Il cookie di sicurezza non è archiviato nella tabella `AspNetUsers` (o altrove nel database di identità). Il token del cookie di sicurezza è autofirmato tramite [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con le informazioni relative a `UserId, SecurityStamp` e ora di scadenza.

Il middleware dei cookie controlla il cookie per ogni richiesta. Il metodo `SecurityStampValidator` nella classe `Startup` raggiunge il database e controlla periodicamente il timbro di sicurezza, come specificato con il `validateInterval`. Questo problema si verifica solo ogni 30 minuti (in questo esempio), a meno che non si modifichi il profilo di sicurezza. È stato scelto l'intervallo di 30 minuti per ridurre al minimo i viaggi al database.

Il metodo di `SignInAsync` deve essere chiamato quando viene apportata una modifica al profilo di sicurezza. Quando viene modificato il profilo di sicurezza, il database aggiorna il campo `SecurityStamp` e senza chiamare il metodo di `SignInAsync` si rimarrà connessi *solo* fino alla successiva esecuzione della pipeline OWIN al database (il `validateInterval`). Per eseguire questa verifica, modificare il metodo `SignInAsync` in modo che venga restituito immediatamente e impostare la proprietà cookie `validateInterval` da 30 minuti a 5 secondi:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con le modifiche al codice sopra riportate, è possibile modificare il profilo di sicurezza, ad esempio modificando lo stato di **due fattori abilitati**, e si verrà disconnessi in 5 secondi in caso di errore del metodo `SecurityStampValidator.OnValidateIdentity`. Rimuovere la riga di ritorno nel metodo di `SignInAsync`, apportare un'altra modifica del profilo di sicurezza e non verrà effettuata la disconnessione. Il metodo `SignInAsync` genera un nuovo cookie di sicurezza.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'app di esempio è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Fare clic su Abilita 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Disconnettersi, quindi eseguire di nuovo l'accesso. Se è stata abilitata la posta elettronica (vedere l' [esercitazione precedente](account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare SMS o messaggio di posta elettronica per 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Viene visualizzata la pagina Verifica codice in cui è possibile immettere il codice (da SMS o posta elettronica).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Se si fa clic sulla casella di controllo **memorizza questo browser** , non sarà più necessario usare 2FA per accedere a tale computer e browser. Se si Abilita 2FA e si fa clic sul pulsante **ricorda, questo browser** fornirà una protezione 2FA avanzata da utenti malintenzionati che tentano di accedere all'account, purché non dispongano dell'accesso al computer. Questa operazione può essere eseguita in qualsiasi computer privato utilizzato regolarmente. Impostando **ricorda questo browser**, si ottiene la maggiore sicurezza di 2FA dai computer che non vengono usati regolarmente e si ottiene la comodità di non dover passare attraverso 2FA nei propri computer. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Come registrare un provider di autenticazione a due fattori

Quando si crea un nuovo progetto MVC, il file *IdentityConfig.cs* contiene il codice seguente per registrare un provider di autenticazione a due fattori:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Aggiungere un numero di telefono per 2FA

Il metodo di azione `AddPhoneNumber` nel controller `Manage` genera un token di sicurezza e lo invia al numero di telefono fornito.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Dopo l'invio del token, viene reindirizzato al metodo di azione `VerifyPhoneNumber`, in cui è possibile immettere il codice per registrare SMS per 2FA. SMS 2FA non viene usato fino a quando non si verifica il numero di telefono.

## <a name="enabling-2fa"></a>Abilitazione di 2FA

Il metodo di azione `EnableTFA` Abilita 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Si noti che è necessario chiamare il `SignInAsync` perché Enable 2FA è una modifica al profilo di sicurezza. Quando 2FA è abilitato, l'utente dovrà usare 2FA per accedere, usando gli approcci 2FA registrati (SMS e posta elettronica nell'esempio).

È possibile aggiungere altri provider 2FA, ad esempio generatori di codice a matrice, oppure è possibile scrivere i propri (vedere [uso di Google Authenticator con ASP.NET Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> I codici 2FA vengono generati usando [un algoritmo di password monouso basato sul tempo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e i codici sono validi per sei minuti. Se si immettono più di sei minuti per l'immissione del codice, verrà ricevuto un messaggio di errore di codice non valido.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinare account di accesso di social networking e locali

È possibile combinare account locali e di social networking facendo clic sul collegamento di posta elettronica. Nella sequenza seguente &quot;RickAndMSFT@gmail.com&quot; viene creato per la prima volta come account di accesso locale, ma è possibile creare prima l'account come log di social networking, quindi aggiungere un account di accesso locale.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Fare clic sul collegamento **Gestisci** . Si notino gli account di accesso di social network (0) esterni associati a questo account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Fai clic sul collegamento a un altro servizio di accesso e accetta le richieste dell'app. I due account sono stati combinati e sarà possibile accedere con entrambi gli account. Potrebbe essere necessario che gli utenti aggiungano gli account locali nel caso in cui il servizio di autenticazione dei log sociali sia inattivo o più probabilmente abbiano perso l'accesso al proprio account di social networking.

Nella figura seguente Tom è un log di social networking, che è possibile visualizzare dagli account di **accesso esterni: 1** visualizzati nella pagina.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Quando si fa clic su **Seleziona una password** è possibile aggiungere un log locale associato allo stesso account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blocco degli account da attacchi di forza bruta

È possibile proteggere gli account nell'app da attacchi con dizionario abilitando il blocco degli utenti. Il codice seguente nel metodo `ApplicationUserManager Create` configura lock out:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Il codice precedente Abilita il blocco solo per l'autenticazione a due fattori. Sebbene sia possibile abilitare il blocco per gli account di accesso modificando `shouldLockout` su true nel metodo `Login` del controller di account, è consigliabile non abilitare il blocco per gli account di accesso perché rende l'account vulnerabile agli attacchi di accesso [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Nel codice di esempio, il blocco è disabilitato per l'account amministratore creato nel metodo `ApplicationDbInitializer Seed`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Richiedere a un utente di avere un account di posta elettronica convalidato

Il codice seguente richiede che un utente disponga di un account di posta elettronica convalidato prima di poter eseguire l'accesso:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Come SignInManager verifica il requisito di 2FA

Sia il registro locale che quello di social networking verificano se 2FA è abilitato. Se 2FA è abilitato, il metodo di accesso `SignInManager` restituisce `SignInStatus.RequiresVerification`e l'utente verrà reindirizzato al metodo di azione `SendCode`, in cui sarà necessario immettere il codice per completare il log in sequenza. Se per l'utente è impostato RememberMe sul cookie locale Users, il `SignInManager` restituirà `SignInStatus.Success` e non sarà necessario passare attraverso 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Nel codice seguente viene illustrato il metodo di azione `SendCode`. Viene creato un [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) con tutti i metodi 2FA abilitati per l'utente. Il [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene passato all'helper [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , che consente all'utente di selezionare l'approccio 2FA, in genere posta elettronica e SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Una volta che l'utente ha inserito l'approccio 2FA, viene chiamato il metodo di azione `HTTP POST SendCode`, il `SignInManager` invia il codice 2FA e l'utente viene reindirizzato al metodo di azione `VerifyCode` in cui è possibile immettere il codice per completare l'accesso.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Blocco 2FA

Sebbene sia possibile impostare il blocco degli account per gli errori dei tentativi di accesso alla password, questo approccio rende l'accesso vulnerabile ai blocchi [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Si consiglia di usare il blocco degli account solo con 2FA. Quando viene creata la `ApplicationUserManager`, il codice di esempio imposta il blocco 2FA e `MaxFailedAccessAttemptsBeforeLockout` su cinque. Dopo che un utente ha eseguito l'accesso (tramite un account locale o un account di social networking), ogni tentativo non riuscito in 2FA viene archiviato e se viene raggiunto il numero massimo di tentativi, l'utente viene bloccato per cinque minuti (è possibile impostare l'ora di blocco con `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET Identity risorse consigliate](../getting-started/aspnet-identity-recommended-resources.md) Elenco completo di Blog di identità, video, esercitazioni e collegamenti eccezionali.
- L' [app MVC 5 con l'accesso a Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Mostra anche come aggiungere informazioni sul profilo alla tabella users.
- [ASP.NET MVC and Identity 2,0: informazioni sulle nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) di John atten.
- [Conferma dell'account e recupero della password con ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) di Rastogi.
- [ASP.NET Identity 2,0: configurazione della convalida dell'account e dell'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) di John atten.
