---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Creare un'app Web Form ASP.NET sicura con registrazione utente, conferma tramite posta elettronica e reimpostazione della password (C#) | Microsoft Docs
author: Erikre
description: Questa esercitazione illustra come creare un'app Web Form ASP.NET con la registrazione dell'utente, la conferma tramite posta elettronica e la reimpostazione della password usando il membro ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625153"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Creare un'app Web Forms ASP.NET sicura con registrazione utente, messaggi di posta elettronica di conferma e reimpostazione della password (C#)

di [Erik Reitan](https://github.com/Erikre)

> Questa esercitazione illustra come creare un'app Web Form ASP.NET con la registrazione dell'utente, la conferma tramite posta elettronica e la reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. Questa esercitazione è basata sull' [esercitazione su MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)di Rick Anderson.

## <a name="introduction"></a>Introduzione

Questa esercitazione illustra i passaggi necessari per creare un'applicazione Web Form ASP.NET con Visual Studio e ASP.NET 4,5 per creare un'app Web form sicura con registrazione utente, conferma tramite posta elettronica e reimpostazione della password.

### <a name="tutorial-tasks-and-information"></a>Attività e informazioni dell'esercitazione:

- [Creare un'app Web Form ASP.NET](#createWebForms)
- [Associare SendGrid](#SG)
- [Richiedi conferma posta elettronica prima dell'accesso](#require)
- [Ripristino e reimpostazione della password](#reset)
- [Invia nuovamente il collegamento di conferma tramite posta elettronica](#rsend)
- [Risoluzione dei problemi dell'app](#dbg)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'app Web Form ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare anche [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: per completare questa esercitazione, è necessario installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

1. Creare un nuovo progetto (**File** -&gt; **nuovo progetto**) e selezionare il modello **applicazione Web ASP.NET** e la versione più recente del .NET Framework dalla finestra di dialogo **nuovo progetto** .
2. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **Web Form** . Lasciare l'autenticazione predefinita come **account utente singoli**. Se si vuole ospitare l'app in Azure, lasciare selezionata la casella **di controllo host nel cloud** .   
 Quindi, fare clic su **OK** per creare il nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Abilitare Secure Sockets Layer (SSL) per il progetto. Attenersi alla procedura disponibile nella sezione **abilitare SSL per il progetto** della serie di [esercitazioni introduzione con Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Eseguire l'app, fare clic sul collegamento **Register** e registrare un nuovo utente. A questo punto, l'unica convalida sul messaggio di posta elettronica è basata sull'attributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) per assicurarsi che l'indirizzo di posta elettronica sia ben formato. Il codice viene modificato per aggiungere la conferma tramite posta elettronica. Chiudere la finestra del browser.
5. In **Esplora server** di Visual Studio (**visualizza** -&gt; **Esplora server**) passare a **Data Connections\DefaultConnection\Tables\AspNetUsers**, fare clic con il pulsante destro del mouse e selezionare **Apri definizione tabella**. 

    Nella figura seguente viene illustrato lo schema della tabella `AspNetUsers`:

    ![Schema della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. In **Esplora server**fare clic con il pulsante destro del mouse sulla tabella **AspNetUsers** e scegliere **Mostra dati tabella**.  
  
    ![Dati della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto, il messaggio di posta elettronica per l'utente registrato non è stato confermato.
7. Fare clic sulla riga e selezionare Elimina per eliminare l'utente. Questo messaggio di posta elettronica verrà aggiunto di nuovo nel passaggio successivo e verrà inviato un messaggio di conferma all'indirizzo di posta elettronica.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile confermare il messaggio di posta elettronica durante la registrazione di un nuovo utente per verificare che non stiano usando un altro utente, ovvero che non sono stati registrati con la posta elettronica di un altro utente. Si supponga di avere un forum di discussione che si vuole impedire che `"bob@cpandl.com"` venga registrato come `"joe@contoso.com"`. Senza la conferma della posta elettronica, `"joe@contoso.com"` possibile ricevere messaggi di posta elettronica indesiderati dall'app. Supponiamo che Bob sia stato accidentalmente registrato come `"bib@cpandl.com"` e non sia stato notato, non sarebbe in grado di usare il ripristino della password perché l'app non ha la posta elettronica corretta. La conferma tramite posta elettronica offre solo una protezione limitata dai bot e non fornisce protezione da spammer determinati.

In genere si vuole impedire ai nuovi utenti di inviare dati al sito Web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo. Nelle sezioni riportate di seguito verrà abilitata la conferma tramite posta elettronica e verrà modificato il codice per impedire l'accesso degli utenti appena registrati fino alla conferma della posta elettronica. In questa esercitazione verrà usato il servizio di posta elettronica SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Associare SendGrid

SendGrid ha modificato l'API perché questa esercitazione è stata scritta. Per le istruzioni SendGrid correnti, vedere [SendGrid](http://sendgrid.com/) o [abilitare la conferma dell'account e il recupero della password](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Sebbene questa esercitazione mostri solo come aggiungere notifiche tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare messaggi di posta elettronica usando SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. In Visual Studio aprire la **console di gestione pacchetti** (**strumenti** -&gt; gestione **pacchetti NuGet** -&gt; **console di gestione pacchetti**) e immettere il comando seguente:  
    `Install-Package SendGrid`
2. Accedere alla [pagina di iscrizione di Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registrarsi per ottenere un account SendGrid gratuito. È anche possibile iscriversi per ottenere un account SendGrid gratuito direttamente nel [sito di SendGrid](http://www.sendgrid.com).
3. Da **Esplora soluzioni** aprire il file *IdentityConfig.cs* nell' *app\_* cartella di avvio e aggiungere il codice seguente evidenziato in giallo alla classe `EmailService` per configurare **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Aggiungere inoltre le seguenti istruzioni `using` all'inizio del file *IdentityConfig.cs* : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Per semplificare l'esempio, archiviare i valori dell'account del servizio di posta elettronica nella sezione `appSettings` del file *Web. config* . Aggiungere il codice XML seguente evidenziato in giallo al file *Web. config* nella radice del progetto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sicurezza: non archiviare mai dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviati nella sezione **appSetting** del file *Web. config* . In Azure è possibile archiviare in modo sicuro questi valori nella scheda **[Configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** della portale di Azure. Per informazioni correlate, vedere l'argomento di Rick Anderson intitolato [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Aggiungere i valori del servizio di posta elettronica per riflettere i valori di autenticazione SendGrid (nome utente e password) in modo da poter inviare messaggi di posta elettronica dall'app. Assicurarsi di usare il nome dell'account SendGrid anziché l'indirizzo di posta elettronica fornito da SendGrid.

### <a name="enable-email-confirmation"></a>Abilita conferma posta elettronica

 Per abilitare la conferma tramite posta elettronica, è necessario modificare il codice di registrazione attenendosi alla procedura seguente.  

1. Nella cartella *account* aprire il code-behind *Register.aspx.cs* e aggiornare il metodo `CreateUser_Click` per abilitare le modifiche evidenziate di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *default. aspx* e selezionare **Imposta come pagina iniziale**.
3. Eseguire l'app premendo **F5.** Dopo la visualizzazione della pagina, fare clic sul collegamento **Register (registra** ) per visualizzare la pagina Register (registra).
4. Immettere la posta elettronica e la password, quindi fare clic sul pulsante **Register (registra** ) per inviare un messaggio di posta elettronica tramite SendGrid.  
   Lo stato corrente del progetto e del codice consentirà all'utente di accedere dopo aver completato il modulo di registrazione, anche se non ha confermato il proprio account.
5. Controllare l'account di posta elettronica e fare clic sul collegamento per confermare la posta elettronica.  
   Una volta inviato il modulo di registrazione, si verrà connessi.  
    ![Sito Web di esempio-accesso](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedi conferma posta elettronica prima dell'accesso

Anche se l'account di posta elettronica è stato confermato, a questo punto non è necessario fare clic sul collegamento contenuto nel messaggio di posta elettronica di verifica per eseguire l'accesso completo. Nella sezione seguente verrà modificato il codice che richiede che i nuovi utenti dispongano di un messaggio di posta elettronica confermato prima dell'accesso (autenticato).

1. In **Esplora soluzioni** di Visual Studio aggiornare l'evento `CreateUser_Click` nel code-behind *Register.aspx.cs* contenuto nella cartella *account* con le modifiche evidenziate di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aggiornare il metodo `LogIn` nel code-behind *login.aspx.cs* con le modifiche evidenziate seguenti: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

 Ora che è stato implementato il codice per verificare se l'indirizzo di posta elettronica di un utente è stato confermato, è possibile controllare la funzionalità nelle pagine di **registrazione** e di **accesso** . 

1. Eliminare tutti gli account nella tabella **AspNetUsers** che contengono l'alias di posta elettronica che si desidera testare.
2. Eseguire l'app (**F5**) e verificare che non sia possibile registrarsi come utente fino a quando non viene confermato l'indirizzo di posta elettronica.
3. Prima di confermare il nuovo account tramite l'indirizzo di posta elettronica appena inviato, provare ad accedere con il nuovo account.  
 Si noterà che non è possibile accedere e che è necessario avere un account di posta elettronica confermato.
4. Dopo aver confermato l'indirizzo di posta elettronica, accedere all'app.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Ripristino e reimpostazione della password

1. In Visual Studio rimuovere i caratteri di commento dal metodo `Forgot` nel code-behind *Forgot.aspx.cs* contenuto nella cartella *account* , in modo che il metodo venga visualizzato come segue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Aprire la pagina *login. aspx* . Sostituire il markup vicino alla fine della sezione **LoginForm** come evidenziato di seguito: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Aprire il code-behind *login.aspx.cs* e rimuovere il commento dalla seguente riga di codice evidenziata in giallo dal gestore dell'evento `Page_Load`: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Eseguire l'app premendo **F5.** Dopo la visualizzazione della pagina, fare clic sul collegamento **Accedi** .
5. Fare clic sul collegamento **password dimenticata?** per visualizzare la pagina **password dimenticata** .
6. Immettere l'indirizzo di posta elettronica e fare clic sul pulsante **Submit (Invia** ) per inviare un messaggio di posta elettronica all'indirizzo che consente di reimpostare la password.   
   Controllare l'account di posta elettronica e fare clic sul collegamento per visualizzare la pagina **Reimposta password** .
7. Nella pagina **Reimposta password** immettere l'indirizzo di posta elettronica, la password e la password confermata. Quindi, fare clic sul pulsante **Reimposta** .  
   Quando la password viene reimpostata correttamente, viene visualizzata la pagina **password modificata** . A questo punto è possibile accedere con la nuova password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Invia nuovamente il collegamento di conferma tramite posta elettronica

Quando un utente crea un nuovo account locale, riceve un messaggio di posta elettronica con un collegamento di conferma che è necessario usare per poter accedere. Se l'utente elimina accidentalmente il messaggio di posta elettronica di conferma o il messaggio di posta elettronica non arriva mai, sarà necessario inviare nuovamente il collegamento di conferma. Le modifiche al codice seguenti illustrano come abilitare questa operazione.

1. In Visual Studio aprire il code-behind **login.aspx.cs** e aggiungere il gestore eventi seguente dopo il gestore dell'evento `LogIn`:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modificare il gestore dell'evento `LogIn` nel code-behind *login.aspx.cs* modificando il codice evidenziato in giallo come indicato di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aggiornare la pagina *login. aspx* aggiungendo il codice evidenziato in giallo come indicato di seguito: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Eliminare tutti gli account nella tabella **AspNetUsers** che contengono l'alias di posta elettronica che si desidera testare.
5. Eseguire l'app (**F5**) e registrare l'indirizzo di posta elettronica.
6. Prima di confermare il nuovo account tramite l'indirizzo di posta elettronica appena inviato, provare ad accedere con il nuovo account.  
   Si noterà che non è possibile accedere e che è necessario avere un account di posta elettronica confermato. Inoltre, è ora possibile inviare nuovamente un messaggio di conferma al proprio account di posta elettronica.
7. Immettere l'indirizzo di posta elettronica e la password, quindi premere il pulsante Invia di nuovo la **conferma** .
8. Dopo aver confermato l'indirizzo di posta elettronica in base al messaggio di posta elettronica appena inviato, accedere all'app.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Risoluzione dei problemi dell'app

Se non si riceve un messaggio di posta elettronica contenente il collegamento per verificare le credenziali:

- Controllare la cartella della posta indesiderata.
- Accedere all'account SendGrid e fare clic sul [collegamento attività posta elettronica](https://sendgrid.com/logs/index).
- Assicurarsi di aver usato il nome dell'account utente SendGrid come valore di *Web. config* anziché l'indirizzo di posta elettronica dell'account SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Collegamenti a ASP.NET Identity risorse consigliate](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie di esercitazioni su Web Form ASP.NET-aggiungere un provider OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Distribuire un'app Web Form ASP.NET sicura con appartenenza, OAuth e database SQL al servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie di esercitazioni su Web Form ASP.NET-Abilita SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
