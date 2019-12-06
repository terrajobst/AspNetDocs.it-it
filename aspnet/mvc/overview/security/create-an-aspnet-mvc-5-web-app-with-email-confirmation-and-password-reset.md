---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creare un'app Web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazioneC#della password () | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 con la conferma della posta elettronica e la reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. CA...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899702"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Creare un'app Web ASP.NET MVC 5 sicura con accesso, messaggi di posta elettronica di conferma e reimpostazione della password (C#)

di [Rick Anderson]((https://twitter.com/RickAndMSFT))

Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 con la conferma della posta elettronica e la reimpostazione della password usando il sistema di appartenenze ASP.NET Identity.

Per una versione aggiornata di questa esercitazione che usa .NET Core, vedere la pagina relativa alla conferma dell'account e al ripristino della password in ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'app MVC ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: per completare questa esercitazione, è necessario installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Forms supporta anche ASP.NET Identity, quindi è possibile seguire una procedura simile in un'app Web Form.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **account utente singoli**. Se si vuole ospitare l'app in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione si distribuirà in Azure. È possibile [aprire un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)gratuitamente.
3. Impostare il [progetto per l'utilizzo di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Eseguire l'app, fare clic sul collegamento **Register** e registrare un utente. A questo punto, l'unica convalida sul messaggio di posta elettronica è l'attributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
5. In Esplora server passare a **Data Connections\DefaultConnection\Tables\AspNetUsers**, fare clic con il pulsante destro del mouse e selezionare **Apri definizione tabella**.

    Nella figura seguente viene illustrato lo schema `AspNetUsers`:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Fare clic con il pulsante destro del mouse sulla tabella **AspNetUsers** e scegliere **Mostra dati tabella**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto, il messaggio di posta elettronica non è stato confermato.
7. Fare clic sulla riga e selezionare Elimina. Questo messaggio di posta elettronica verrà aggiunto di nuovo nel passaggio successivo e verrà inviato un messaggio di posta elettronica di conferma.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

Una procedura consigliata consiste nel confermare l'indirizzo di posta elettronica di una nuova registrazione utente per verificare che non stiano rappresentando un altro utente, ovvero che non sono stati registrati con la posta elettronica di un altro utente. Si supponga di avere un forum di discussione che si vuole impedire che `"bob@example.com"` venga registrato come `"joe@contoso.com"`. Senza la conferma della posta elettronica, `"joe@contoso.com"` possibile ricevere messaggi di posta elettronica indesiderati dall'app. Supponiamo che Bob sia stato accidentalmente registrato come `"bib@example.com"` e non sia stato notato, non sarebbe in grado di usare il ripristino della password perché l'app non ha la posta elettronica corretta. La conferma tramite posta elettronica offre solo una protezione limitata dai bot e non fornisce protezione da spammer determinati, ma ha molti alias di posta elettronica di lavoro che possono usare per la registrazione.

In genere si desidera impedire ai nuovi utenti di inviare dati al sito Web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo. <a id="build"></a>Nelle sezioni riportate di seguito verrà abilitata la conferma tramite posta elettronica e verrà modificato il codice per impedire l'accesso degli utenti appena registrati fino alla conferma della posta elettronica.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Associare SendGrid

Le istruzioni riportate in questa sezione non sono aggiornate. Per istruzioni aggiornate, vedere [configurare il provider di posta elettronica SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) .

Sebbene questa esercitazione mostri solo come aggiungere notifiche tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare messaggi di posta elettronica usando SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. Nella Console di Gestione pacchetti immettere il comando seguente: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Passare alla [pagina di iscrizione di Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registrarsi per ottenere un account SendGrid gratuito. Configurare SendGrid aggiungendo codice simile al seguente in *app_start/identityconfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

È necessario aggiungere quanto segue:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Per semplificare questo esempio, verranno archiviate le impostazioni dell'app nel file *Web. config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sicurezza: non archiviare mai dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviati in appSetting. In Azure è possibile archiviare in modo sicuro questi valori nella scheda **[Configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** della portale di Azure. Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

### <a name="enable-email-confirmation-in-the-account-controller"></a>Abilitare la conferma della posta elettronica nel controller account

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verificare che il file *Views\Account\ConfirmEmail.cshtml* abbia la sintassi Razor corretta. Il carattere @ nella prima riga potrebbe essere mancante. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Eseguire l'app e fare clic sul collegamento Register (registra). Una volta inviato il modulo di registrazione, si è connessi.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Controllare l'account di posta elettronica e fare clic sul collegamento per confermare la posta elettronica.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedi conferma posta elettronica prima dell'accesso

Al momento del completamento del modulo di registrazione, l'utente ha eseguito l'accesso. In genere si vuole confermare la posta elettronica prima di registrarli in. Nella sezione seguente il codice verrà modificato in modo da richiedere che i nuovi utenti dispongano di un messaggio di posta elettronica confermato prima dell'accesso (autenticato). Aggiornare il metodo `HttpPost Register` con le modifiche evidenziate di seguito:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Impostando come commento il metodo `SignInAsync`, l'utente non verrà connesso mediante la registrazione. La riga di `TempData["ViewBagLink"] = callbackUrl;` può essere usata per [eseguire il debug dell'app](#dbg) e verificare la registrazione senza inviare messaggi di posta elettronica. `ViewBag.Message` viene utilizzato per visualizzare le istruzioni di conferma. L' [esempio di download](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene codice per testare la conferma della posta elettronica senza configurare la posta elettronica e può essere usato anche per eseguire il debug dell'applicazione.

Creare un file di `Views\Shared\Info.cshtml` e aggiungere il markup Razor seguente:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Aggiungere l' [attributo autorizzi](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) al metodo di azione `Contact` del controller Home. È possibile fare clic sul collegamento **contatto** per verificare che gli utenti anonimi non dispongano dell'accesso e che gli utenti autenticati dispongano dell'accesso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

È necessario aggiornare anche il metodo di azione `HttpPost Login`:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aggiornare la visualizzazione *Views\Shared\Error.cshtml* per visualizzare il messaggio di errore:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Eliminare tutti gli account nella tabella **AspNetUsers** che contengono l'alias di posta elettronica con cui si vuole eseguire il test. Eseguire l'app e verificare che non sia possibile accedere fino a quando non è stato confermato l'indirizzo di posta elettronica. Dopo aver verificato l'indirizzo di posta elettronica, fare clic sul collegamento **contatto** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Ripristino/reimpostazione della password

Rimuovere i caratteri di commento dal metodo di azione `HttpPost ForgotPassword` nel controller dell'account:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Rimuovere i caratteri di commento dal `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) nel file di visualizzazione Razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Nella pagina di accesso verrà ora visualizzato un collegamento per reimpostare la password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Invia nuovamente il collegamento di conferma tramite posta elettronica

Quando un utente crea un nuovo account locale, riceve un messaggio di posta elettronica con un collegamento di conferma che è necessario usare per poter accedere. Se l'utente elimina accidentalmente il messaggio di posta elettronica di conferma o il messaggio di posta elettronica non arriva mai, sarà necessario inviare nuovamente il collegamento di conferma. Le modifiche al codice seguenti illustrano come abilitare questa operazione.

Aggiungere il seguente metodo helper alla fine del file *Controllers\AccountController.cs* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aggiornare il metodo Register per l'uso del nuovo Helper:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aggiornare il metodo login per inviare di nuovo la password se l'account utente non è stato confermato:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinare account di accesso di social networking e locali

È possibile combinare account locali e di social networking facendo clic sul collegamento di posta elettronica. Nella sequenza seguente **RickAndMSFT@gmail.com** viene creato inizialmente come account di accesso locale, ma è possibile creare prima l'account come log di social networking, quindi aggiungere un account di accesso locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Fare clic sul collegamento **Gestisci** . Si notino gli **account di accesso esterni: 0** associati a questo account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Fai clic sul collegamento a un altro servizio di accesso e accetta le richieste dell'app. I due account sono stati combinati e sarà possibile accedere con entrambi gli account. Potrebbe essere necessario che gli utenti aggiungano gli account locali nel caso in cui il servizio di autenticazione dei log sociali sia inattivo o più probabilmente abbiano perso l'accesso al proprio account di social networking.

Nella figura seguente Tom è un log di social networking, che è possibile visualizzare dagli account di **accesso esterni: 1** visualizzati nella pagina.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Quando si fa clic su **Seleziona una password** è possibile aggiungere un log locale associato allo stesso account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Conferma tramite posta elettronica più dettagliata

La conferma dell'account dell'esercitazione [e il ripristino della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) passano a questo argomento con ulteriori dettagli.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'app

Se non si riceve un messaggio di posta elettronica contenente il collegamento:

- Controllare la cartella della posta indesiderata.
- Accedere all'account SendGrid e fare clic sul [collegamento attività posta elettronica](https://sendgrid.com/logs/index).

Per testare il collegamento di verifica senza posta elettronica, scaricare l' [esempio completato](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Nella pagina verrà visualizzato il collegamento di conferma e i codici di conferma.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Collegamenti a ASP.NET Identity risorse consigliate](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Esamina più dettagliatamente il recupero delle password e la conferma dell'account.
- [App MVC 5 con accesso a Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con l'autorizzazione Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere altri dati al database di identità.
- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Questa esercitazione aggiunge la distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione dell'app in Facebook e connessione dell'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurazione di SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
