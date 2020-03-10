---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Autenticazione di Windows Azure | Microsoft Docs
author: Rick-Anderson
description: Microsoft ASP.NET Tools per Windows Azure Active Directory semplifica l'abilitazione dell'autenticazione per le applicazioni Web ospitate in siti Web di Microsoft Azure....
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce98effe18dd739504fb0d5453bae8a46c3ba102
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557862"
---
# <a name="windows-azure-authentication"></a>Autenticazione di Windows Azure

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Microsoft ASP.NET Tools per Windows Azure Active Directory semplifica l'abilitazione dell'autenticazione per le applicazioni Web ospitate in [siti Web di Windows Azure](https://www.windowsazure.com/home/features/web-sites/). È possibile usare l'autenticazione di Windows Azure per autenticare gli utenti di Office 365 dall'organizzazione, gli account aziendali sincronizzati dal Active Directory locale o gli utenti creati nel dominio Windows Azure Active Directory personalizzato. L'abilitazione dell'autenticazione di Windows Azure consente di configurare l'applicazione per l'autenticazione degli utenti tramite un singolo tenant di [windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .
>
> Lo strumento di autenticazione di Windows Azure ASP.NET non è supportato per i ruoli Web in un servizio cloud, ma in una versione futura verrà pianificato. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) è supportato nei ruoli Web di Windows Azure.
>
> Per informazioni dettagliate su come configurare la sincronizzazione tra il Active Directory locale e il tenant di Windows Azure Active Directory, vedere [usare AD FS 2,0 per implementare e gestire Single Sign-on](https://technet.microsoft.com/library/jj205462.aspx).
>
> Windows Azure Active Directory è attualmente disponibile come [servizio in anteprima gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="requirements"></a>Requisiti:

- Visual Studio 2012 o [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) o [web tools extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Strumenti di Microsoft ASP.NET per windows Azure Active Directory-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) o [Microsoft ASP.net tools per windows Azure Active Directory-Visual Studio Express 2012 per il Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Creare un'applicazione Web ASP.NET con Visual Studio 2012

È possibile creare qualsiasi applicazione Web con Visual Studio 2012, in questa esercitazione viene usato il modello Intranet di ASP.NET MVC.

1. Creare una nuova applicazione Intranet ASP.NET MVC 4 e accettare tutte le impostazioni predefinite. (Deve essere **un in rete e non nel progetto** **ter** NET).
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Abilitare l'autenticazione di Windows Azure (quando si è un amministratore globale del principio)

Se non si dispone di un tenant di Windows Azure Active Directory esistente (ad esempio, tramite un account di Office 365 esistente), è possibile creare un nuovo tenant effettuando l'iscrizione a un [nuovo account di windows Azure Active Directory](https://g.microsoftonline.com/0AX00en/5).

1. Dal menu progetto selezionare **Abilita autenticazione di Microsoft Azure**:

   ![](windows-azure-authentication/_static/image2.png)

2. Immettere il dominio per il tenant di Windows Azure Active Directory (ad esempio, contoso.onmicrosoft.com) e fare clic su **Abilita**:

![](windows-azure-authentication/_static/image3.png)

3. Nella finestra di dialogo autenticazione Web accedere come amministratore del tenant di Windows Azure Active Directory:

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Abilitare Windows Azure da un non amministratore del principio

Se non si dispone dei privilegi di amministratore globale per il tenant di Windows Azure Active Directory, è possibile deselezionare la casella di controllo per il provisioning dell'applicazione.

![](windows-azure-authentication/_static/image6.png)

Nella finestra di dialogo verranno visualizzati il **dominio**, l' **ID dell'entità applicazione** e l' **URL di risposta** necessari per il provisioning dell'applicazione con un Azure Active Directory principio. È necessario fornire queste informazioni a un utente che dispone di privilegi sufficienti per eseguire il provisioning dell'applicazione. Per informazioni dettagliate su come usare il cmdlet per creare manualmente l'entità servizio, vedere[come implementare Single Sign-on con l'applicazione Windows Azure Active Directory-ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) .
Una volta eseguito correttamente il provisioning dell'applicazione, è possibile fare clic su **continua per aggiornare il file Web. config con le impostazioni selezionate**. Se si desidera continuare a sviluppare l'applicazione in attesa del provisioning, è possibile fare clic su **Chiudi per ricordare le impostazioni nel file di progetto**. La volta successiva che si richiama Abilita autenticazione di Microsoft Azure e si deseleziona la casella di controllo provisioning, verranno visualizzate le stesse impostazioni ed è possibile fare clic su **continua**, quindi su **Applica queste impostazioni in Web. config**.

1. Attendere che l'applicazione sia configurata per l'autenticazione di Windows Azure ed eseguito il provisioning con Windows Azure Active Directory.
2. Una volta abilitata l'autenticazione di Windows Azure per l'applicazione, fare clic su **Chiudi:**

    ![](windows-azure-authentication/_static/image7.png)
3. Premere F5 per eseguire l'applicazione. Viene automaticamente reindirizzato alla pagina di accesso. Per accedere all'applicazione, usare le credenziali utente del principio di directory.

    ![](windows-azure-authentication/_static/image1.jpg)
4. Poiché l'applicazione sta attualmente utilizzando un certificato di prova autofirmato, si riceverà un avviso dal browser che indica che il certificato non è stato emesso da un'autorità di certificazione attendibile.

    Questo avviso può essere ignorato in modo sicuro durante lo sviluppo locale facendo clic su **continua al sito Web:**

    ![](windows-azure-authentication/_static/image8.png)
5. A questo punto è stato effettuato l'accesso all'applicazione con l'autenticazione di Windows Azure.

    ![](windows-azure-authentication/_static/image2.jpg)

L'abilitazione dell'autenticazione di Windows Azure apporta le seguenti modifiche all'applicazione:

- Al progetto viene aggiunta una classe anti-cross-site request falsificazione ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) ( *app\_Start\AntiXsrfConfig.cs* ).
- Il `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` pacchetti NuGet viene aggiunto al progetto.
- Le impostazioni di Windows Identity Foundation nell'applicazione verranno configurate in modo da accettare i token di sicurezza dal tenant di Windows Azure Active Directory. Fare clic sull'immagine seguente per visualizzare una visualizzazione espansa delle modifiche apportate al file *Web. config* .

     ![](windows-azure-authentication/_static/image9.png)
- Verrà eseguito il provisioning di un'entità servizio per l'applicazione nel tenant di Windows Azure Active Directory.
- HTTPS è abilitato.

## <a name="deploy-the-application-to-windows-azure"></a>Distribuire l'applicazione in Windows Azure

Per istruzioni complete, vedere [distribuzione di un'applicazione web ASP.NET in un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Per pubblicare un'applicazione utilizzando l'autenticazione di Windows Azure in un sito Web di Azure:

1. Fare clic con il pulsante destro del mouse sull'applicazione e scegliere **pubblica:**

    ![](windows-azure-authentication/_static/image3.jpg)
2. Dalla finestra di dialogo Pubblica sito Web scaricare e importare un profilo di pubblicazione per il sito Web di Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. La scheda **connessione** Mostra l' **URL di destinazione** , ovvero l'URL pubblico per l'applicazione. Fare clic su **convalida connessione** per testare la connessione:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Se il sito Web di Azure è stato pubblicato in precedenza, è consigliabile selezionare l'impostazione **Rimuovi file aggiuntivi nella destinazione** per assicurarsi che l'applicazione venga pubblicata in modo corretto. Si noti che la casella di controllo **Abilita autenticazione di Microsoft Azure** è selezionata.

    ![](windows-azure-authentication/_static/image10.png)
5. Facoltativo: nella scheda **Anteprima** fare clic su **Avvia anteprima** per visualizzare i file distribuiti.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Fare clic su **pubblica.**

    Verrà richiesto di abilitare l'autenticazione di Windows Azure per l'host di destinazione. Fare clic su **Abilita** per continuare:

    ![](windows-azure-authentication/_static/image11.png)
7. Immettere le credenziali di amministratore per il tenant di Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Una volta che l'applicazione è stata pubblicata correttamente, viene aperto un browser per il sito Web pubblicato.

    > [!NOTE]
    > Potrebbero essere necessari fino a cinque minuti (in genere devono essere minori) affinché l'applicazione venga sottoposta a provisioning completo con Windows Azure Active Directory dopo aver abilitato l'autenticazione di Windows Azure per l'host di destinazione. Quando si esegue l'applicazione per la prima volta, se si riceve l'errore ACS50001: Impossibile trovare la relying party con il nome ' [Realm]', attendere alcuni minuti e provare a eseguire di nuovo l'applicazione.
9. Quando richiesto, accedere come utente nella directory:

    ![](windows-azure-authentication/_static/image8.jpg)
10. L'accesso all'applicazione ospitata in Azure è stato eseguito con l'autenticazione di Windows Azure.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemi noti

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>L'autorizzazione basata sui ruoli ha esito negativo quando si usa l'autenticazione di Windows Azure

L'autenticazione di Windows Azure attualmente non fornisce l'attestazione del ruolo necessaria in modo che possa essere eseguita l'autorizzazione basata sui ruoli. Il ruolo dell'utente autenticato deve essere recuperato manualmente da Windows Azure Active Directory.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>Se si passa a un'applicazione con l'autenticazione di Windows Azure, viene restituito l'errore "ACS20016 il dominio dell'utente che ha eseguito l'accesso (live.com) non corrisponde ad alcun dominio consentito di questo STS"

Se è già stato effettuato l'accesso a un account Microsoft, ad esempio hotmail.com, live.com, outlook.com, e si tenta di accedere a un'applicazione che ha abilitato l'autenticazione di Windows Azure, è possibile che venga rilevata una risposta di errore 400 perché il dominio dell'account Microsoft non è riconosciuto da Windows Azure Active Directory. Per accedere all'applicazione, effettuare prima la disconnessione dall'account Microsoft.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>L'accesso a un'applicazione con l'autenticazione di Windows Azure abilitata e un X509CertificateValidationMode diverso da nessuno genera errori di convalida del certificato per il certificato accounts.accesscontrol.windows.net

La convalida del certificato non è obbligatoria e deve essere lasciata disabilitata. L'identificazione personale del certificato dell'autorità emittente viene convalidata da WSFederationAuthenticationModule.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Quando si tenta di abilitare l'autenticazione di Windows Azure, nella finestra di dialogo autenticazione Web viene visualizzato l'errore "ACS20016: il dominio dell'utente che ha eseguito l'accesso (contoso.onmicrosoft.com) non corrisponde ad alcun dominio consentito di questo servizio token di servizio".

Questo errore può essere visualizzato quando è stato eseguito correttamente l'accesso con un account di Windows Azure Active Directory diverso dall'interno dello stesso processo di Visual Studio. Disconnettersi dall'account specificato oppure riavviare Visual Studio. Se in precedenza è stato effettuato l'accesso ed è stata selezionata l'opzione "Mantieni l'accesso", potrebbe essere necessario cancellare i cookie del browser.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012: la richiesta non è un messaggio del protocollo WS-Federation valido

Questo problema può verificarsi se è già stato effettuato l'accesso con un altro ID Microsoft a uno dei servizi di Azure. Usare una finestra del browser privata come InPrivate in IE o in incognito in Chrome o cancellare tutti i cookie.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Strumenti di Microsoft ASP.NET per Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Funzionalità di Windows Azure: identità](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: sviluppare app per l'organizzazione](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: sviluppare app per più organizzazioni](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Come implementare Single Sign-On con Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Single Sign-on con Windows Azure Active Directory: un'approfondita](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Usare AD FS 2,0 per implementare e gestire Single Sign-On](https://technet.microsoft.com/library/jj205462.aspx)
