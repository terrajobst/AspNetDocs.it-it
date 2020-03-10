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
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="aa703-103">Accesso utilizzando siti esterni in un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="aa703-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="aa703-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa703-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa703-105">Questo articolo illustra come accedere al sito di Pagine Web ASP.NET (Razor) con Facebook, Google, Twitter, Yahoo e altri siti, ovvero come supportare OAuth e OpenID nel sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="aa703-106">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="aa703-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="aa703-107">Come abilitare l'accesso da altri siti quando si usa il modello di sito Starter di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="aa703-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="aa703-108">Questa è la funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="aa703-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="aa703-109">Helper `OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="aa703-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa703-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="aa703-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aa703-111">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="aa703-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="aa703-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="aa703-112">WebMatrix 3</span></span>

<span data-ttu-id="aa703-113">Pagine Web ASP.NET include il supporto per i provider [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) .</span><span class="sxs-lookup"><span data-stu-id="aa703-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="aa703-114">Usando questi provider, è possibile consentire agli utenti di accedere al sito usando le credenziali esistenti di Facebook, Twitter, Microsoft e Google.</span><span class="sxs-lookup"><span data-stu-id="aa703-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="aa703-115">Ad esempio, per accedere con un account Facebook, gli utenti possono semplicemente scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui immettono le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="aa703-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="aa703-116">Possono quindi associare l'account di accesso di Facebook al proprio account nel sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="aa703-117">Un miglioramento correlato alle funzionalità di appartenenza alle pagine Web è che gli utenti possono associare più account di accesso (inclusi gli account di accesso da siti di social networking) con un unico account nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="aa703-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="aa703-118">Questa immagine mostra la pagina di accesso del modello di **sito Starter** , in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare l'accesso con un account esterno:</span><span class="sxs-lookup"><span data-stu-id="aa703-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="aa703-120">È possibile abilitare l'appartenenza a OAuth e OpenID rimuovendo il commento da alcune righe di codice nel modello di **sito Starter** .</span><span class="sxs-lookup"><span data-stu-id="aa703-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="aa703-121">I metodi e le proprietà usati per lavorare con i provider OAuth e OpenID si trovano nella classe `WebMatrix.Security.OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="aa703-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="aa703-122">Il modello di **sito Starter** include un'infrastruttura di appartenenza completa, completa con una pagina di accesso, un database di appartenenza e tutto il codice necessario per consentire agli utenti di accedere al sito usando le credenziali locali o quelle di un altro sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="aa703-123">Questa sezione fornisce un esempio di come consentire agli utenti di accedere da siti esterni a un sito basato sul modello di **sito Starter** .</span><span class="sxs-lookup"><span data-stu-id="aa703-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="aa703-124">Dopo aver creato un sito iniziale, è possibile procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="aa703-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="aa703-125">Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno.</span><span class="sxs-lookup"><span data-stu-id="aa703-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="aa703-126">In questo modo vengono fornite le chiavi dell'applicazione necessarie per richiamare la funzionalità di accesso per tali siti.</span><span class="sxs-lookup"><span data-stu-id="aa703-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="aa703-127">Per i siti che usano un provider OpenID (Google), non è necessario creare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="aa703-128">Per tutti questi siti, è necessario disporre di un account per l'accesso e la creazione di applicazioni per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="aa703-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa703-129">Le applicazioni Microsoft accettano solo un URL Live per un sito Web funzionante, pertanto non è possibile usare un URL del sito Web locale per il test degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="aa703-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="aa703-130">Modificare alcuni file nel sito Web per specificare il provider di autenticazione appropriato e inviare un account di accesso al sito che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="aa703-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="aa703-131">Questo articolo fornisce istruzioni separate per le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="aa703-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="aa703-132">Abilitazione degli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="aa703-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="aa703-133">Abilitazione degli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="aa703-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="aa703-134">Abilitazione degli accessi a Twitter</span><span class="sxs-lookup"><span data-stu-id="aa703-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="aa703-135">Abilitazione degli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="aa703-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="aa703-136">Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="aa703-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="aa703-137">Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dalla riga di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="aa703-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="aa703-138">Test dell'account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="aa703-138">Testing Google login</span></span>

1. <span data-ttu-id="aa703-139">Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante **Accedi** .</span><span class="sxs-lookup"><span data-stu-id="aa703-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="aa703-140">Nella pagina *account di accesso* , nella **sezione usare un altro servizio per accedere** , scegliere il pulsante Invia **Google** o **Yahoo** .</span><span class="sxs-lookup"><span data-stu-id="aa703-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="aa703-141">In questo esempio viene usato l'account di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="aa703-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="aa703-142">La pagina Web reindirizza la richiesta alla pagina di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="aa703-142">The web page redirects the request to the Google login page.</span></span>

    ![Accesso a Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="aa703-144">Immettere le credenziali per un account Google esistente.</span><span class="sxs-lookup"><span data-stu-id="aa703-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="aa703-145">Se Google chiede se si vuole consentire a *localhost* di usare le informazioni dell'account, fare clic su **Consenti**.</span><span class="sxs-lookup"><span data-stu-id="aa703-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="aa703-146">Il codice usa il token Google per autenticare l'utente e quindi torna a questa pagina nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="aa703-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="aa703-147">Questa pagina consente agli utenti di associare l'account di accesso di Google a un account esistente nel sito Web oppure di registrare un nuovo account nel sito per associare l'account di accesso esterno a.</span><span class="sxs-lookup"><span data-stu-id="aa703-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="aa703-149">Scegliere il pulsante **associa** .</span><span class="sxs-lookup"><span data-stu-id="aa703-149">Choose the **Associate** button.</span></span> <span data-ttu-id="aa703-150">Il browser torna all'home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="aa703-151">Abilitazione degli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="aa703-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="aa703-152">Accedere al [sito degli sviluppatori di Facebook](https://developers.facebook.com/apps) (accedere se non si è già connessi).</span><span class="sxs-lookup"><span data-stu-id="aa703-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="aa703-153">Scegliere il pulsante **Crea nuova app** , quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="aa703-154">Nella sezione **selezionare il modo in cui l'app si integrerà con Facebook**, scegliere la sezione del **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="aa703-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="aa703-155">Immettere il campo **URL sito** con l'URL del sito, ad esempio `http://www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="aa703-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="aa703-156">Il campo del **dominio** è facoltativo; è possibile usarlo per fornire l'autenticazione per un intero dominio, ad esempio *example.com*.</span><span class="sxs-lookup"><span data-stu-id="aa703-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="aa703-157">Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore al campo **URL sito** per testare il sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="aa703-158">Tuttavia, ogni volta che il numero di porta del sito locale viene modificato, sarà necessario aggiornare il campo **URL sito** dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="aa703-159">Scegliere il pulsante **Salva modifiche** .</span><span class="sxs-lookup"><span data-stu-id="aa703-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="aa703-160">Scegliere di nuovo la scheda **app** e quindi visualizzare la pagina iniziale per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="aa703-161">Copiare i valori di **ID app** e **segreto app** per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="aa703-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="aa703-162">Questi valori vengono passati al provider Facebook nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="aa703-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="aa703-163">Uscire dal sito per sviluppatori Facebook.</span><span class="sxs-lookup"><span data-stu-id="aa703-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="aa703-164">A questo punto è possibile apportare modifiche a due pagine del sito Web in modo che gli utenti possano accedere al sito usando gli account Facebook.</span><span class="sxs-lookup"><span data-stu-id="aa703-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="aa703-165">Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="aa703-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="aa703-166">Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dal codice per il provider OAuth di Facebook.</span><span class="sxs-lookup"><span data-stu-id="aa703-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="aa703-167">Il blocco di codice non commentato ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="aa703-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="aa703-168">Copiare il valore **ID app** dall'applicazione Facebook come valore del parametro `appId` (all'interno delle virgolette).</span><span class="sxs-lookup"><span data-stu-id="aa703-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="aa703-169">Copiare il valore del **segreto dell'app** dall'applicazione Facebook come valore del parametro `appSecret`.</span><span class="sxs-lookup"><span data-stu-id="aa703-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="aa703-170">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="aa703-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="aa703-171">Test dell'account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="aa703-171">Testing Facebook login</span></span>

1. <span data-ttu-id="aa703-172">Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="aa703-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="aa703-173">Nella pagina *account di accesso* , nella sezione **usare un altro servizio per accedere** , scegliere l'icona di **Facebook** .</span><span class="sxs-lookup"><span data-stu-id="aa703-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="aa703-174">La pagina Web reindirizza la richiesta alla pagina di accesso di Facebook.</span><span class="sxs-lookup"><span data-stu-id="aa703-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="aa703-176">Accedere a un account Facebook.</span><span class="sxs-lookup"><span data-stu-id="aa703-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="aa703-177">Il codice usa il token Facebook per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso di Facebook all'account di accesso del sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="aa703-178">Il nome utente o l'indirizzo di posta elettronica viene inserito nel campo del **messaggio di posta elettronica** nel modulo.</span><span class="sxs-lookup"><span data-stu-id="aa703-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="aa703-180">Scegliere il pulsante **associa** .</span><span class="sxs-lookup"><span data-stu-id="aa703-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="aa703-181">Il browser torna al home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="aa703-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="aa703-182">Abilitazione degli accessi a Twitter</span><span class="sxs-lookup"><span data-stu-id="aa703-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="aa703-183">Passare al [sito degli sviluppatori di Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="aa703-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="aa703-184">Scegliere il collegamento **Crea un'app** e quindi accedere al sito.</span><span class="sxs-lookup"><span data-stu-id="aa703-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="aa703-185">Nel modulo **Crea un'applicazione** compilare i campi **nome** e **Descrizione** .</span><span class="sxs-lookup"><span data-stu-id="aa703-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="aa703-186">Nel campo **sito Web** immettere l'URL del sito, ad esempio `http://www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="aa703-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="aa703-187">Se si sta testando il sito localmente (usando un URL come `http://localhost:12345`), Twitter potrebbe non accettare l'URL.</span><span class="sxs-lookup"><span data-stu-id="aa703-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="aa703-188">Tuttavia, potrebbe essere possibile utilizzare l'indirizzo IP di loopback locale, ad esempio `http://127.0.0.1:12345`.</span><span class="sxs-lookup"><span data-stu-id="aa703-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="aa703-189">Questo semplifica il processo di test dell'applicazione in locale.</span><span class="sxs-lookup"><span data-stu-id="aa703-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="aa703-190">Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il campo del **sito Web** dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa703-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="aa703-191">Nel campo **URL callback** immettere un URL per la pagina nel sito Web a cui si vuole che gli utenti tornino dopo l'accesso a Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa703-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="aa703-192">Ad esempio, per inviare utenti al home page del sito Starter (che riconoscerà lo stato di accesso), immettere lo stesso URL immesso nel campo **sito Web** .</span><span class="sxs-lookup"><span data-stu-id="aa703-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="aa703-193">Accettare i termini e scegliere il pulsante **Crea applicazione Twitter** .</span><span class="sxs-lookup"><span data-stu-id="aa703-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="aa703-194">Nella pagina di destinazione **applicazioni** , scegliere l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="aa703-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="aa703-195">Nella scheda **Dettagli** scorrere fino alla fine e scegliere il pulsante **Crea il token di accesso** .</span><span class="sxs-lookup"><span data-stu-id="aa703-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="aa703-196">Nella scheda **Dettagli** copiare i valori di **chiave utente** e **segreto consumer** per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="aa703-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="aa703-197">Questi valori verranno passati al provider Twitter nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="aa703-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="aa703-198">Uscire dal sito Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa703-198">Exit the Twitter site.</span></span>

<span data-ttu-id="aa703-199">È ora possibile apportare modifiche a due pagine del sito Web in modo che gli utenti possano accedere al sito usando gli account Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa703-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="aa703-200">Creare o aprire un sito Pagine Web ASP.NET basato sul modello di sito Starter di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="aa703-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="aa703-201">Aprire la pagina *\_AppStart. cshtml* e rimuovere il commento dal codice per il provider OAuth di Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa703-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="aa703-202">Il blocco di codice non commentato ha un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="aa703-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="aa703-203">Copiare il valore della **chiave utente** dall'applicazione Twitter come valore del parametro `consumerKey` (all'interno delle virgolette).</span><span class="sxs-lookup"><span data-stu-id="aa703-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="aa703-204">Copiare il valore del **segreto del consumer** dall'applicazione Twitter come valore del parametro `consumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="aa703-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="aa703-205">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="aa703-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="aa703-206">Test dell'accesso a Twitter</span><span class="sxs-lookup"><span data-stu-id="aa703-206">Testing Twitter login</span></span>

1. <span data-ttu-id="aa703-207">Eseguire la pagina *default. cshtml* del sito e scegliere il pulsante di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="aa703-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="aa703-208">Nella pagina *account di accesso* , nella sezione **usare un altro servizio per accedere** , scegliere l'icona di **Twitter** .</span><span class="sxs-lookup"><span data-stu-id="aa703-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="aa703-209">La pagina Web reindirizza la richiesta a una pagina di accesso di Twitter per l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="aa703-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="aa703-211">Accedere a un account Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa703-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="aa703-212">Il codice usa il token Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso all'account del sito Web.</span><span class="sxs-lookup"><span data-stu-id="aa703-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="aa703-213">Il nome o l'indirizzo di posta elettronica viene inserito nel campo del **messaggio di posta elettronica** nel modulo.</span><span class="sxs-lookup"><span data-stu-id="aa703-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="aa703-215">Scegliere il pulsante **associa** .</span><span class="sxs-lookup"><span data-stu-id="aa703-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="aa703-216">Il browser torna al home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="aa703-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="aa703-217">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="aa703-217">Additional Resources</span></span>

- [<span data-ttu-id="aa703-218">Personalizzazione del comportamento a livello di sito</span><span class="sxs-lookup"><span data-stu-id="aa703-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="aa703-219">Aggiunta di sicurezza e appartenenza a un sito Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa703-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
