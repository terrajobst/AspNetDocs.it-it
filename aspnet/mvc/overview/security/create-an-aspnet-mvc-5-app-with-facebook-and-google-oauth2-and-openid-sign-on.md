---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creare un'app MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-onC#() | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere con OAuth 2,0 con credenziali da un preautenti esterno...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566080"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Creare un'app ASP.NET MVC 5 con l'accesso OAuth2 di Facebook, Twitter, LinkedIn e Google (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra come creare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere usando [OAuth 2,0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterno, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google. Per semplicità, questa esercitazione è incentrata sull'uso delle credenziali da Facebook e Google.
> 
> L'abilitazione di queste credenziali nei siti Web offre un vantaggio significativo perché milioni di utenti dispongono già di account con questi provider esterni. Questi utenti potrebbero essere più inclini ad iscriversi al sito se non è necessario creare e ricordare un nuovo set di credenziali.
> 
> Vedere anche [app MVC 5 ASP.NET con SMS e posta elettronica con autenticazione a due fattori](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Nell'esercitazione viene inoltre illustrato come aggiungere i dati del profilo per l'utente e come utilizzare l'API di appartenenza per aggiungere ruoli. Questa esercitazione è stata scritta da [Rick Anderson](https://blogs.msdn.com/rickAndy) (Seguimi su Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Introduzione

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva. Per informazioni su Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, STEAM, Stack Exchange, TripIt, TIC, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 ed eseguire il debug localmente senza avvisi SSL.

Fare clic su **nuovo progetto** nella pagina **iniziale** oppure è possibile usare il menu e selezionare **file**, quindi **nuovo progetto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Creazione della prima applicazione

Fare clic su **nuovo progetto**, quindi selezionare oggetto **visivo C#**  a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Assegnare al progetto il nome "MvcAuth" e quindi fare clic su **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Nella finestra di dialogo **nuovo progetto ASP.NET** fare clic su **MVC**. Se l'autenticazione non corrisponde a **singoli account utente**, fare clic sul pulsante **Modifica autenticazione** e selezionare **singoli account utente**. Selezionando **host nel cloud**, l'app sarà molto facile da ospitare in Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se è stato selezionato **host nel cloud**, completare la finestra di dialogo Configura.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Usare NuGet per eseguire l'aggiornamento alla versione più recente del middleware OWIN

Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Nel menu a sinistra selezionare **aggiornamenti** . È possibile fare clic sul pulsante **Aggiorna tutto** oppure è possibile cercare solo i pacchetti OWIN (mostrati nell'immagine successiva):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Nell'immagine seguente vengono visualizzati solo i pacchetti OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Dalla console di gestione pacchetti (PMC), è possibile immettere il comando `Update-Package`, che aggiornerà tutti i pacchetti.

Premere **F5** o **CTRL + F5** per eseguire l'applicazione. Nell'immagine seguente il numero di porta è 1234. Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.

A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti **Home**, **informazioni**, **contatti**, **registrazione** e **accesso** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configurazione di SSL nel progetto

Per connettersi a provider di autenticazione come Google e Facebook, sarà necessario configurare IIS-Express per l'uso di SSL. È importante usare SSL dopo l'accesso e non eseguire il fallback a HTTP, il cookie di accesso è altrettanto segreto come nome utente e password e senza usare SSL lo si invia in testo non crittografato attraverso la rete. Inoltre, è già stato impiegato il tempo necessario per eseguire l'handshake e proteggere il canale (ovvero la maggior parte di ciò che rende HTTPS più lento rispetto a HTTP) prima di eseguire la pipeline MVC, quindi il reindirizzamento a HTTP dopo l'accesso non renderà la richiesta corrente o futura richieste molto più veloci.

1. In **Esplora soluzioni**fare clic sul progetto **MvcAuth** .
2. Premere il tasto F4 per visualizzare le proprietà del progetto. In alternativa, dal menu **Visualizza** è possibile selezionare **finestra Proprietà**.
3. Impostare **SSL abilitato** su true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiare l'URL SSL (che verrà `https://localhost:44300/` a meno che non siano stati creati altri progetti SSL).
5. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MvcAuth** e scegliere **proprietà**.
6. Selezionare la scheda **Web** , quindi incollare l'URL SSL nella casella **URL progetto** . Salvare il file (CTL + S). Questo URL sarà necessario per configurare le app di autenticazione di Facebook e Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Aggiungere l'attributo [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al controller di `Home` per richiedere che tutte le richieste usino HTTPS. Un approccio più sicuro consiste nell'aggiungere il filtro [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) all'applicazione. Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo di autorizzazione&quot; nell'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla in app Azure Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Di seguito è riportata una parte del controller Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Premere CTRL+F5 per eseguire l'applicazione. Se il certificato è stato installato in passato, è possibile ignorare il resto di questa sezione e passare alla [creazione di un'app Google per OAuth 2 e connettere l'app al progetto](#goog). in caso contrario, seguire le istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leggere la finestra di dialogo **avviso di sicurezza** e quindi fare clic su **Sì** se si vuole installare il certificato che rappresenta localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. In Internet Explorer viene visualizzata la pagina *Home* e non viene visualizzato alcun avviso SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome accetta anche il certificato e visualizza il contenuto HTTPS senza un avviso. Firefox usa il proprio archivio certificati, quindi verrà visualizzato un avviso. Per l'applicazione è possibile fare clic con il pulsante destro del mouse sul **rischio**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto

> [!WARNING]
> Per le istruzioni di Google OAuth correnti, vedere la pagina relativa alla [configurazione dell'autenticazione di Google in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Passare a [Google Developers Console](https://console.developers.google.com/).
2. Se non è ancora stato creato un progetto, selezionare le **credenziali** nella scheda a sinistra e quindi selezionare **Crea**.
3. Nella scheda a sinistra fare clic su **credenziali**.
4. Fare clic su **create Credentials** Then **OAuth client ID**. 

    1. Nella finestra di dialogo **Crea ID client** , salvare l' **applicazione Web** predefinita per il tipo di applicazione.
    2. Impostare le origini **JavaScript autorizzate** sull'URL SSL usato sopra (`https://localhost:44300/`, a meno che non siano stati creati altri progetti SSL)
    3. Impostare l' **URI di reindirizzamento autorizzato** su:  
         `https://localhost:44300/signin-google`
5. Fare clic sulla voce di menu della schermata di consenso OAuth, quindi impostare l'indirizzo di posta elettronica e il nome del prodotto. Una volta completato il modulo, fare clic su **Salva**.
6. Fare clic sulla voce di menu libreria, cercare **Google + API**, fare clic su di essa e quindi premere Abilita.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L'immagine seguente mostra le API abilitate.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Da gestione API Google API, visitare la scheda **credenziali** per ottenere l' **ID client**. Scaricare per salvare un file JSON con i segreti dell'applicazione. Copiare e incollare i **ClientID** e **ClientSecret** nel metodo `UseGoogleAuthentication` trovato nel file *Startup.auth.cs* nella cartella *app_start* . I valori **ClientID** e **ClientSecret** riportati di seguito sono esempi e non funzionano.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicurezza: non archiviare mai dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice riportato sopra per semplificare l'esempio. Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili a ASP.NET e al servizio app Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Premere **CTRL+F5** per compilare ed eseguire l'applicazione. Fare clic sul collegamento **Accedi** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. In **Usa un altro servizio per accedere**fare clic su **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se non si verificano i passaggi precedenti, verrà ricevuto un errore HTTP 401. Controllare nuovamente i passaggi precedenti. Se non si dispone di un'impostazione obbligatoria (ad esempio **nome prodotto**), aggiungere l'elemento mancante e salvare; per il corretto funzionamento dell'autenticazione possono essere necessari alcuni minuti.
10. Si verrà reindirizzati al sito Google in cui verranno immesse le credenziali.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Dopo avere immesso le credenziali, verrà richiesto di concedere le autorizzazioni all'applicazione Web appena creata:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Fare clic **Accept**. A questo punto si verrà reindirizzati alla pagina **Register** dell'applicazione MvcAuth in cui è possibile registrare il proprio account Google. Sarà possibile modificare il nome di registrazione dell'indirizzo di posta elettronica locale usato per l'account Gmail, anche se in generale è preferibile mantenere l'alias di posta elettronica predefinito, ovvero quello usato per l'autenticazione. Fare clic su **Register**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Creazione dell'app in Facebook e connessione dell'app al progetto

> [!WARNING]
> Per le attuali istruzioni di autenticazione di Facebook OAuth2, vedere [configurazione dell'autenticazione Facebook](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Scegliere **Esplora server**dal menu **Visualizza** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Espandere **DefaultConnection (MvcAuth)** , espandere **tabelle**, fare clic con il pulsante destro del mouse su **AspNetUsers** e scegliere **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Aggiunta di dati di profilo alla classe utente

In questa sezione si aggiungeranno la data di nascita e la Home page ai dati utente durante la registrazione, come illustrato nella figura seguente.

![reg con Home Town e compleanno](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Aprire il file *Models\IdentityModels.cs* e aggiungere le proprietà date di nascita e Home Town:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Aprire il file *Models\AccountViewModels.cs* e le proprietà set birth date e Home town in `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Aprire il file *Controllers\AccountController.cs* e aggiungere il codice per la data di nascita e la Home Page nel metodo di azione `ExternalLoginConfirmation` come illustrato:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Aggiungere la data di nascita e la Home page al file *Views\Account\ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Eliminare il database delle appartenenze in modo che sia possibile registrare di nuovo l'account Facebook con l'applicazione e verificare che sia possibile aggiungere le nuove informazioni relative alla data di nascita e al profilo della città Home.

Da **Esplora soluzioni**fare clic sull'icona **Mostra tutti i file** , quindi fare clic con il pulsante destro del mouse su *aggiungi\_Data\aspnet-MvcAuth-&lt;datestamp&gt;. MDF* e scegliere **Elimina**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dal menu **strumenti** fare clic su gestione **pacchetti NuGet**, quindi su **console di gestione pacchetti** (PMC). Immettere i comandi seguenti nella console di gestione pacchetti.

1. Enable-Migrations
2. Aggiunta-migrazione init
3. Update-database

Eseguire l'applicazione e usare FaceBook e Google per accedere e registrare alcuni utenti.

## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Scegliere **Esplora server**dal menu **Visualizza** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Fare clic con il pulsante destro del mouse su **AspNetUsers** e scegliere **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

I campi `HomeTown` e `BirthDate` sono visualizzati di seguito.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Disconnettere l'app ed effettuare l'accesso con un altro account

Se si accede all'app con Facebook, e si esegue la disconnessione e si tenta di accedere di nuovo con un account Facebook diverso (usando lo stesso browser), si accederà immediatamente all'account Facebook precedente usato. Per usare un altro account, è necessario passare a Facebook e disconnettersi da Facebook. La stessa regola si applica a qualsiasi altro provider di autenticazione di terze parti. In alternativa, è possibile accedere con un altro account usando un browser diverso.

## <a name="next-steps"></a>Passaggi successivi

Vedere [Introducing the Yahoo and LinkedIn OAuth Security Providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) di Jerrie Pelser for Yahoo and LinkedIn instructions. Vedere i pulsanti di accesso di social networking Jerrie per ASP.NET MVC 5 per ottenere i pulsanti Abilita accesso social.

Seguire l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua questa esercitazione e Mostra quanto segue:

1. Come distribuire l'app in Azure.
2. Come proteggere l'app con i ruoli.
3. Come proteggere l'app con i filtri [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizza](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) .
4. Come usare l'API di appartenenza per aggiungere utenti e ruoli.

Invia commenti e suggerimenti su come questa esercitazione è stata apprezzata e cosa possiamo migliorare. È anche possibile richiedere nuovi argomenti in [Mostra come usare il codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). È anche possibile richiedere e votare le nuove funzionalità da aggiungere a ASP.NET. È possibile, ad esempio, votare uno strumento per la [creazione e la gestione di utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Per una spiegazione corretta del funzionamento di ASP.NET External Authentication Services, vedere [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services)di Robert McMurray. Nell'articolo di Robert viene inoltre illustrato in dettaglio l'abilitazione dell'autenticazione Microsoft e Twitter. L'eccellente [esercitazione EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) di Tom Dykstra illustra come usare la Entity Framework.
