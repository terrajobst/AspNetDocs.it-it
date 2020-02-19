---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Creare un'app Web Form ASP.NET con l'autenticazione a due fattori diC#SMS () | Microsoft Docs
author: Erikre
description: Questa esercitazione illustra come creare un'app Web Form ASP.NET con l'autenticazione a due fattori. Questa esercitazione è stata progettata per completare l'esercitazione denominata CR...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77466421"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Creare un'app Web Forms ASP.NET con l'autenticazione a due fattori SMS (C#)

di [Erik Reitan](https://github.com/Erikre)

[Scaricare l'app Web Form ASP.NET con la posta elettronica e l'autenticazione a due fattori SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Questa esercitazione illustra come creare un'app Web Form ASP.NET con l'autenticazione a due fattori. Questa esercitazione è stata progettata per completare l'esercitazione intitolata [creare un'app web forms ASP.NET sicura con registrazione utente, conferma tramite posta elettronica e reimpostazione della password](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Questa esercitazione è stata inoltre basata sull' [esercitazione MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)di Rick Anderson.

## <a name="introduction"></a>Introduzione

Questa esercitazione illustra i passaggi necessari per creare un'applicazione Web Form ASP.NET che supporta l'autenticazione a due fattori con Visual Studio. L'autenticazione a due fattori è un passaggio di autenticazione utente aggiuntivo. Questo passaggio aggiuntivo genera un PIN (Personal Identification Number) univoco durante l'accesso. Il PIN viene comunemente inviato all'utente come messaggio di posta elettronica o SMS. L'utente dell'app immette quindi il PIN come misura di autenticazione aggiuntiva durante l'accesso.

### <a name="tutorial-tasks-and-information"></a>Attività e informazioni dell'esercitazione:

- [Creare un'app Web Form ASP.NET](#createWebForms)
- [Configurare SMS e autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori per l'utente registrato](#use2FA)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'app Web Form ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare anche [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva. Sarà inoltre necessario creare un account [Twilio](https://www.twilio.com/try-twilio) , come illustrato di seguito.

> [!NOTE]
> Importante: per completare questa esercitazione, è necessario installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

1. Creare un nuovo progetto (**File** -&gt; **nuovo progetto**) e selezionare il modello **applicazione Web ASP.NET** insieme alla versione .NET Framework 4.5.2 dalla finestra di dialogo **nuovo progetto** .
2. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **Web Form** . Lasciare l'autenticazione predefinita come **account utente singoli**. Quindi, fare clic su **OK** per creare il nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Abilitare Secure Sockets Layer (SSL) per il progetto. Attenersi alla procedura disponibile nella sezione **abilitare SSL per il progetto** della serie di [esercitazioni introduzione con Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. In Visual Studio aprire la **console di gestione pacchetti** (**strumenti** -&gt; gestione **pacchetti NuGet** -&gt; **console di gestione pacchetti**) e immettere il comando seguente:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configurare SMS e autenticazione a due fattori

Questa esercitazione USA Twilio, ma è possibile usare qualsiasi provider SMS.

1. Creare un account [Twilio](https://www.twilio.com/try-twilio) .
2. Dalla scheda **Dashboard** dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione.** Che sarà possibile aggiungere all'app in un secondo momento.
3. Dalla scheda **numeri** copiare anche il numero di **telefono** Twilio.
4. Rendere disponibili per l'app il **SID dell'account**Twilio, il **token di autenticazione** e il numero di **telefono** . Per semplificare l'operazione, questi valori vengono archiviati nel file *Web. config* . Quando si esegue la distribuzione in Azure, è possibile archiviare i valori in modo sicuro nella sezione **appSettings** della scheda Configura del sito Web. Inoltre, quando si aggiunge il numero di telefono, utilizzare solo numeri.   
   Si noti che è anche possibile aggiungere le credenziali di SendGrid. SendGrid è un servizio di notifica tramite posta elettronica. Per informazioni dettagliate su come abilitare SendGrid, vedere la sezione "associare SendGrid" dell'esercitazione intitolata [creare un'app Web form ASP.NET protetta con registrazione utente, conferma tramite posta elettronica e reimpostazione della password.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sicurezza: non archiviare mai dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviati nella sezione **appSettings** del file *Web. config* . In Azure è possibile archiviare in modo sicuro questi valori nella scheda **[Configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** della portale di Azure. Per informazioni correlate, vedere l'argomento di Rick Anderson intitolato [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
5. Configurare la classe `SmsService` nel file *App\_Start\IdentityConfig.cs* apportando le modifiche seguenti evidenziate in giallo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Aggiungere le seguenti istruzioni `using` all'inizio del file *IdentityConfig.cs* : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aggiornare il file *account/Manage. aspx* rimuovendo le righe evidenziate in giallo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Nel gestore `Page_Load` del code-behind *Manage.aspx.cs* , rimuovere il commento dalla riga di codice evidenziata in giallo, in modo che venga visualizzato come segue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Per aggiornare il gestore `Page_Load` aggiungendo il codice seguente evidenziato in giallo, nel codebehind dell' *Account*/*TwoFactorAuthenticationSignIn.aspx.cs*: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Con la modifica del codice precedente, il DropDownList "providers" contenente le opzioni di autenticazione non verrà reimpostato sul primo valore. Ciò consentirà all'utente di selezionare correttamente tutte le opzioni da usare durante l'autenticazione, non solo la prima.
10. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *default. aspx* e selezionare **Imposta come pagina iniziale**.
11. Eseguendo il test dell'app, compilare prima l'app (**Ctrl**+**MAIUSC**+**B**), quindi eseguire l'app (**F5**) e selezionare **Register (registra** ) per creare un nuovo account utente o selezionare **Accedi** se l'account utente è già stato registrato.
12. Quando l'utente ha eseguito l'accesso, fare clic sull'ID utente (indirizzo di posta elettronica) nella barra di spostamento per visualizzare la pagina **Gestisci account** (Manage. aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Fare clic su **Aggiungi** accanto a **numero di telefono** nella pagina **Gestisci account** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Aggiungere il numero di telefono in cui si desidera ricevere messaggi SMS (SMS) e fare clic sul pulsante **Submit (Invia** ).   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    A questo punto, l'App utilizzerà le credenziali di *Web. config* per contattare Twilio. Un messaggio SMS (messaggio di testo) verrà inviato al telefono associato all'account utente. È possibile verificare che il messaggio Twilio sia stato inviato visualizzando il dashboard di Twilio.
15. In pochi secondi il telefono associato all'account utente riceverà un messaggio di testo contenente il codice di verifica. Immettere il codice di verifica e premere **invio**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Abilitare l'autenticazione a due fattori per un utente registrato

A questo punto, è stata abilitata l'autenticazione a due fattori per l'app. Per poter usare l'autenticazione a due fattori, un utente può semplicemente modificare le impostazioni usando l'interfaccia utente. 

1. Come utente dell'app è possibile abilitare l'autenticazione a due fattori per l'account specifico facendo clic sull'ID utente (alias di posta elettronica) nella barra di spostamento per visualizzare la pagina **Gestisci account** . Fare quindi clic sul collegamento **Enable (Abilita** ) per abilitare l'autenticazione a due fattori per l'account.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Disconnettersi, quindi eseguire di nuovo l'accesso. Se è stata abilitata la posta elettronica, è possibile selezionare SMS o posta elettronica per l'autenticazione a due fattori. Se la posta elettronica non è stata abilitata, vedere l'esercitazione intitolata [creare un'app web forms ASP.NET sicura con registrazione utente, conferma tramite posta elettronica e reimpostazione della password](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Viene visualizzata la pagina autenticazione a due fattori in cui è possibile immettere il codice (da SMS o posta elettronica).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Se si fa clic sulla casella di controllo **memorizza questo browser** , non sarà necessario usare l'autenticazione a due fattori per accedere quando si usa il browser e il dispositivo in cui è stata selezionata la casella. Se gli utenti malintenzionati non riescono ad accedere al dispositivo, abilitando l'autenticazione a due fattori e facendo clic sul pulsante **ricorda questo browser** fornirà un comodo accesso alla password, mantenendo al tempo stesso la protezione avanzata dell'autenticazione a due fattori per tutti gli accessi da dispositivi non attendibili. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Collegamenti a ASP.NET Identity risorse consigliate](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Distribuire un'app Web Form ASP.NET sicura con appartenenza, OAuth e database SQL in un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie di esercitazioni su Web Form ASP.NET-aggiungere un provider OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Serie di esercitazioni su Web Form ASP.NET-Abilita SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Creazione dell'app in Facebook e connessione dell'app al progetto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
