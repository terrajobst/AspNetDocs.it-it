---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 con l'autenticazione a due fattori. È necessario completare la creazione di un'app Web ASP.NET MVC 5 sicura con...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457596"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 con l'autenticazione a due fattori. Prima di procedere, è necessario completare [la creazione di un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) . È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Il download contiene helper di debug che consentono di testare la conferma della posta elettronica e SMS senza configurare un messaggio di posta elettronica o un provider SMS.
> 
> Questa esercitazione è stata scritta da [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

- [Creare un'app MVC ASP.NET](#createMvc)
- [Configurare SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Risorse aggiuntive](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'app MVC ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: è necessario completare [la creazione di un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password prima di](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) procedere. Per completare questa esercitazione, è necessario installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Forms supporta anche ASP.NET Identity, quindi è possibile seguire una procedura simile in un'app Web Form.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **account utente singoli**. Se si vuole ospitare l'app in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione si distribuirà in Azure. È possibile [aprire un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)gratuitamente.
3. Impostare il [progetto per l'utilizzo di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Individuazione delle credenziali utente del provider SMS**  
  
   Twilio:  
   Dalla scheda **Dashboard** dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione**.  
  
   ASPSMS:  
   Dalle impostazioni dell'account, passare a **userKey** e copiarlo insieme alla **password**definita autonomamente.  
  
   Questi valori vengono archiviati in un secondo momento nel file *Web. config* all'interno delle chiavi `"SMSAccountIdentification"` e `"SMSAccountPassword"`.
4. **Specifica di SenderID/originator**  
  
   Twilio:  
   Dalla scheda **numeri** copiare il numero di telefono Twilio.  
  
   ASPSMS:  
   Nel menu **Sblocca autori** sbloccare uno o più autori o scegliere un originatore alfanumerico (non supportato da tutte le reti).  
  
   Il valore verrà archiviato in un secondo momento nel file *Web. config* all'interno della chiave `"SMSAccountFrom"`.
5. **Trasferimento delle credenziali del provider SMS nell'app**  
  
   Rendere disponibili le credenziali e il numero di telefono del mittente per l'app. Per semplificare le operazioni, questi valori vengono archiviati nel file *Web. config* . Quando si esegue la distribuzione in Azure, è possibile archiviare i valori in modo sicuro nella sezione **impostazioni app** della scheda Configura del sito Web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sicurezza: non archiviare mai dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice riportato sopra per semplificare l'esempio. Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementazione del trasferimento dati al provider SMS**  
  
   Configurare la classe `SmsService` nel file *App\_Start\IdentityConfig.cs* .  
  
   A seconda del provider SMS utilizzato, attivare la sezione **Twilio** o **ASPSMS** : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aggiornare la visualizzazione Razor di *Views\Manage\Index.cshtml* : (Nota: non rimuovere solo i commenti nel codice di uscita, usare il codice seguente).  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Verificare che i metodi di azione `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` nel `ManageController` dispongano dell'attributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Eseguire l'app e accedere con l'account registrato in precedenza.
10. Fare clic sull'ID utente, che attiva il metodo di azione `Index` in `Manage` controller.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Scegli Aggiungi.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Il metodo di azione `AddPhoneNumber` Visualizza una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. In pochi secondi si riceverà un messaggio di testo con il codice di verifica. Immetterla e premere **invio**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La visualizzazione gestione mostra che è stato aggiunto il numero di telefono.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'app generata dal modello è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Fare clic su Abilita 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Disconnettersi, quindi accedere di nuovo. Se è stata abilitata la posta elettronica (vedere l' [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare SMS o messaggio di posta elettronica per 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Viene visualizzata la pagina Verifica codice in cui è possibile immettere il codice (da SMS o posta elettronica).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Se si fa clic sulla casella di controllo **memorizza questo browser** , non sarà necessario usare 2FA per accedere quando si usa il browser e il dispositivo in cui è stata selezionata la casella. Se gli utenti malintenzionati non riescono ad accedere al dispositivo, abilitando 2FA e facendo clic sul pulsante **memorizza questo browser** fornirà un comodo accesso alla password, mantenendo al tempo stesso la protezione 2FA avanzata per tutti gli accessi da dispositivi non attendibili. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.

Questa esercitazione offre una rapida introduzione all'abilitazione di 2FA in una nuova app ASP.NET MVC. L' [autenticazione a due fattori dell'esercitazione con SMS e messaggi di posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) viene illustrata in dettaglio nel codice sottostante l'esempio.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Viene illustrato in dettaglio l'autenticazione a due fattori
- [Collegamenti a ASP.NET Identity risorse consigliate](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Esamina più dettagliatamente il recupero delle password e la conferma dell'account.
- [App MVC 5 con accesso a Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con l'autorizzazione Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere altri dati al database di identità.
- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Questa esercitazione aggiunge la distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione dell'app in Facebook e connessione dell'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurazione di SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Come configurare l' C# ambiente di sviluppo MVC e ASP.NET](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
