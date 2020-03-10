---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Accesso utilizzando siti esterni in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo illustra come accedere al sito di Pagine Web ASP.NET (Razor) con Facebook, Google, Twitter, Yahoo e altri siti, ovvero come supportare...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638754"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Accesso utilizzando siti esterni in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come accedere al sito di Pagine Web ASP.NET (Razor) con Facebook, Google, Twitter, Yahoo e altri siti, ovvero come supportare OAuth e OpenID nel sito.
> 
> Contenuto dell'esercitazione:
> 
> - Come abilitare l'accesso da altri siti quando si usa il modello di sito Starter di WebMatrix.
> 
> Questa è la funzionalità ASP.NET introdotta nell'articolo:
> 
> - Helper `OAuthWebSecurity`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3

Pagine Web ASP.NET include il supporto per i provider [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) . Usando questi provider, è possibile consentire agli utenti di accedere al sito usando le credenziali esistenti di Facebook, Twitter, Microsoft e Google. Ad esempio, per accedere con un account Facebook, gli utenti possono semplicemente scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui immettono le informazioni utente. Possono quindi associare l'account di accesso di Facebook al proprio account nel sito. Un miglioramento correlato alle funzionalità di appartenenza alle pagine Web è che gli utenti possono associare più account di accesso (inclusi gli account di accesso da siti di social networking) con un unico account nel sito Web.

Questa immagine mostra la pagina di accesso del modello di **sito Starter** , in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare l'accesso con un account esterno:

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

È possibile abilitare l'appartenenza a OAuth e OpenID rimuovendo il commento da alcune righe di codice nel modello di **sito Starter** . I metodi e le proprietà usati per lavorare con i provider OAuth e OpenID si trovano nella classe `WebMatrix.Security.OAuthWebSecurity`. Il modello di **sito Starter** include un'infrastruttura di appartenenza completa, completa con una pagina di accesso, un database di appartenenza e tutto il codice necessario per consentire agli utenti di accedere al sito usando le credenziali locali o quelle di un altro sito.

Questa sezione fornisce un esempio di come consentire agli utenti di accedere da siti esterni a un sito basato sul modello di **sito Starter** . Dopo aver creato un sito iniziale, è possibile procedere come segue:

- Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno. In questo modo vengono fornite le chiavi dell'applicazione necessarie per richiamare la funzionalità di accesso per tali siti.
- Per i siti che usano un provider OpenID (Google), non è necessario creare un'applicazione. Per tutti questi siti, è necessario disporre di un account per l'accesso e la creazione di applicazioni per sviluppatori.

    > [!NOTE]
    > Le applicazioni Microsoft accettano solo un URL Live per un sito Web funzionante, pertanto non è possibile usare un URL del sito Web locale per il test degli account di accesso.
- Modificare alcuni file nel sito Web per specificare il provider di autenticazione appropriato e inviare un account di accesso al sito che si vuole usare.

Questo articolo fornisce istruzioni separate per le attività seguenti:

- [Abilitazione degli account di accesso di Google](#To_enable_Google_logins)
- [Abilitazione degli account di accesso di Facebook](#To_enable_Facebook_logins)
- [Abilitazione degli accessi a Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Abilitazione degli account di accesso di Google

1. Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.
2. Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dalla riga di codice seguente. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Test dell'account di accesso di Google

1. Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante **Accedi** .
2. Nella pagina *account di accesso* , nella **sezione usare un altro servizio per accedere** , scegliere il pulsante Invia **Google** o **Yahoo** . In questo esempio viene usato l'account di accesso di Google. 

    La pagina Web reindirizza la richiesta alla pagina di accesso di Google.

    ![Accesso a Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Immettere le credenziali per un account Google esistente.
4. Se Google chiede se si vuole consentire a *localhost* di usare le informazioni dell'account, fare clic su **Consenti**.

    Il codice usa il token Google per autenticare l'utente e quindi torna a questa pagina nel sito Web. Questa pagina consente agli utenti di associare l'account di accesso di Google a un account esistente nel sito Web oppure di registrare un nuovo account nel sito per associare l'account di accesso esterno a.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Scegliere il pulsante **associa** . Il browser torna all'home page dell'applicazione.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Abilitazione degli account di accesso di Facebook

1. Accedere al [sito degli sviluppatori di Facebook](https://developers.facebook.com/apps) (accedere se non si è già connessi).
2. Scegliere il pulsante **Crea nuova app** , quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.
3. Nella sezione **selezionare il modo in cui l'app si integrerà con Facebook**, scegliere la sezione del **sito Web** .
4. Immettere il campo **URL sito** con l'URL del sito, ad esempio `http://www.example.com`. Il campo del **dominio** è facoltativo; è possibile usarlo per fornire l'autenticazione per un intero dominio, ad esempio *example.com*. 

    > [!NOTE]
    > Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore al campo **URL sito** per testare il sito. Tuttavia, ogni volta che il numero di porta del sito locale viene modificato, sarà necessario aggiornare il campo **URL sito** dell'applicazione.
5. Scegliere il pulsante **Salva modifiche** .
6. Scegliere di nuovo la scheda **app** e quindi visualizzare la pagina iniziale per l'applicazione.
7. Copiare i valori di **ID app** e **segreto app** per l'applicazione e incollarli in un file di testo temporaneo. Questi valori vengono passati al provider Facebook nel codice del sito Web.
8. Uscire dal sito per sviluppatori Facebook.

A questo punto è possibile apportare modifiche a due pagine del sito Web in modo che gli utenti possano accedere al sito usando gli account Facebook.

1. Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.
2. Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dal codice per il provider OAuth di Facebook. Il blocco di codice non commentato ha un aspetto simile al seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copiare il valore **ID app** dall'applicazione Facebook come valore del parametro `appId` (all'interno delle virgolette).
4. Copiare il valore del **segreto dell'app** dall'applicazione Facebook come valore del parametro `appSecret`.
5. Salvare e chiudere il file.

### <a name="testing-facebook-login"></a>Test dell'account di accesso di Facebook

1. Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante di **accesso** .
2. Nella pagina *account di accesso* , nella sezione **usare un altro servizio per accedere** , scegliere l'icona di **Facebook** . 

    La pagina Web reindirizza la richiesta alla pagina di accesso di Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Accedere a un account Facebook. 

    Il codice usa il token Facebook per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso di Facebook all'account di accesso del sito. Il nome utente o l'indirizzo di posta elettronica viene inserito nel campo del **messaggio di posta elettronica** nel modulo.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Scegliere il pulsante **associa** . 

    Il browser torna al home page e si è connessi.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Abilitazione degli accessi a Twitter

1. Passare al [sito degli sviluppatori di Twitter](https://dev.twitter.com/).
2. Scegliere il collegamento **Crea un'app** e quindi accedere al sito.
3. Nel modulo **Crea un'applicazione** compilare i campi **nome** e **Descrizione** .
4. Nel campo **sito Web** immettere l'URL del sito, ad esempio `http://www.example.com`. 

    > [!NOTE]
    > Se si sta testando il sito localmente (usando un URL come `http://localhost:12345`), Twitter potrebbe non accettare l'URL. Tuttavia, potrebbe essere possibile utilizzare l'indirizzo IP di loopback locale, ad esempio `http://127.0.0.1:12345`. Questo semplifica il processo di test dell'applicazione in locale. Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il campo del **sito Web** dell'applicazione.
5. Nel campo **URL callback** immettere un URL per la pagina nel sito Web a cui si vuole che gli utenti tornino dopo l'accesso a Twitter. Ad esempio, per inviare utenti al home page del sito Starter (che riconoscerà lo stato di accesso), immettere lo stesso URL immesso nel campo **sito Web** .
6. Accettare i termini e scegliere il pulsante **Crea applicazione Twitter** .
7. Nella pagina di destinazione **applicazioni** , scegliere l'applicazione creata.
8. Nella scheda **Dettagli** scorrere fino alla fine e scegliere il pulsante **Crea il token di accesso** .
9. Nella scheda **Dettagli** copiare i valori di **chiave utente** e **segreto consumer** per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verranno passati al provider Twitter nel codice del sito Web.
10. Uscire dal sito Twitter.

È ora possibile apportare modifiche a due pagine del sito Web in modo che gli utenti possano accedere al sito usando gli account Twitter.

1. Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.
2. Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dal codice per il provider OAuth di Twitter. Il blocco di codice non commentato ha un aspetto simile al seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copiare il valore della **chiave utente** dall'applicazione Twitter come valore del parametro `consumerKey` (all'interno delle virgolette).
4. Copiare il valore del **segreto del consumer** dall'applicazione Twitter come valore del parametro `consumerSecret`.
5. Salvare e chiudere il file.

### <a name="testing-twitter-login"></a>Test dell'accesso a Twitter

1. Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante di **accesso** .
2. Nella pagina *account di accesso* , nella sezione **usare un altro servizio per accedere** , scegliere l'icona di **Twitter** . 

    La pagina Web reindirizza la richiesta a una pagina di accesso di Twitter per l'applicazione creata.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Accedere a un account Twitter.
4. Il codice usa il token Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso all'account del sito Web. Il nome o l'indirizzo di posta elettronica viene inserito nel campo del **messaggio di posta elettronica** nel modulo.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Scegliere il pulsante **associa** . 

    Il browser torna al home page e si è connessi.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Aggiunta di sicurezza e appartenenza a un sito Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
