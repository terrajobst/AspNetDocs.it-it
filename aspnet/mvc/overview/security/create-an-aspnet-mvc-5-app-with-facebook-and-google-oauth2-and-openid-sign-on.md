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
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457687"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="9cf2e-103">Creare un'app ASP.NET MVC 5 con l'accesso OAuth2 di Facebook, Twitter, LinkedIn e Google (C#)</span><span class="sxs-lookup"><span data-stu-id="9cf2e-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>

<span data-ttu-id="9cf2e-104">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9cf2e-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="9cf2e-105">Questa esercitazione illustra come creare un'applicazione Web MVC 5 ASP.NET che consente agli utenti di accedere usando [OAuth 2,0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterno, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="9cf2e-106">Per semplicità, questa esercitazione è incentrata sull'uso delle credenziali da Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="9cf2e-107">L'abilitazione di queste credenziali nei siti Web offre un vantaggio significativo perché milioni di utenti dispongono già di account con questi provider esterni.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="9cf2e-108">Questi utenti potrebbero essere più inclini ad iscriversi al sito se non è necessario creare e ricordare un nuovo set di credenziali.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="9cf2e-109">Vedere anche [app MVC 5 ASP.NET con SMS e posta elettronica con autenticazione a due fattori](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="9cf2e-110">Nell'esercitazione viene inoltre illustrato come aggiungere i dati del profilo per l'utente e come utilizzare l'API di appartenenza per aggiungere ruoli.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="9cf2e-111">Questa esercitazione è stata scritta da [Rick Anderson](https://blogs.msdn.com/rickAndy) (Seguimi su Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="9cf2e-112">Guida introduttiva</span><span class="sxs-lookup"><span data-stu-id="9cf2e-112">Getting Started</span></span>

<span data-ttu-id="9cf2e-113">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="9cf2e-114">Installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="9cf2e-115">Per informazioni su Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, STEAM, Stack Exchange, TripIt, TIC, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="9cf2e-116">È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 ed eseguire il debug localmente senza avvisi SSL.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>

<span data-ttu-id="9cf2e-117">Fare clic su **nuovo progetto** nella pagina **iniziale** oppure è possibile usare il menu e selezionare **file**, quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="9cf2e-118">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="9cf2e-118">Creating Your First Application</span></span>

<span data-ttu-id="9cf2e-119">Fare clic su **nuovo progetto**, quindi selezionare oggetto **visivo C#**  a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="9cf2e-120">Assegnare al progetto il nome "MvcAuth" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="9cf2e-121">Nella finestra di dialogo **nuovo progetto ASP.NET** fare clic su **MVC**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="9cf2e-122">Se l'autenticazione non corrisponde a **singoli account utente**, fare clic sul pulsante **Modifica autenticazione** e selezionare **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="9cf2e-123">Selezionando **host nel cloud**, l'app sarà molto facile da ospitare in Azure.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="9cf2e-124">Se è stato selezionato **host nel cloud**, completare la finestra di dialogo Configura.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="9cf2e-125">Usare NuGet per eseguire l'aggiornamento alla versione più recente del middleware OWIN</span><span class="sxs-lookup"><span data-stu-id="9cf2e-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="9cf2e-126">Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="9cf2e-127">Nel menu a sinistra selezionare **aggiornamenti** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="9cf2e-128">È possibile fare clic sul pulsante **Aggiorna tutto** oppure è possibile cercare solo i pacchetti OWIN (mostrati nell'immagine successiva):</span><span class="sxs-lookup"><span data-stu-id="9cf2e-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="9cf2e-129">Nell'immagine seguente vengono visualizzati solo i pacchetti OWIN:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="9cf2e-130">Dalla console di gestione pacchetti (PMC), è possibile immettere il comando `Update-Package`, che aggiornerà tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="9cf2e-131">Premere **F5** o **CTRL + F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="9cf2e-132">Nell'immagine seguente il numero di porta è 1234.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="9cf2e-133">Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="9cf2e-134">A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti **Home**, **informazioni**, **contatti**, **registrazione** e **accesso** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="9cf2e-135">Configurazione di SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="9cf2e-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="9cf2e-136">Per connettersi a provider di autenticazione come Google e Facebook, sarà necessario configurare IIS-Express per l'uso di SSL.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="9cf2e-137">È importante usare SSL dopo l'accesso e non eseguire il fallback a HTTP, il cookie di accesso è altrettanto segreto come nome utente e password e senza usare SSL lo si invia in testo non crittografato attraverso la rete.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="9cf2e-138">Inoltre, è già stato impiegato il tempo necessario per eseguire l'handshake e proteggere il canale (ovvero la maggior parte di ciò che rende HTTPS più lento rispetto a HTTP) prima di eseguire la pipeline MVC, quindi il reindirizzamento a HTTP dopo l'accesso non renderà la richiesta corrente o futura richieste molto più veloci.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="9cf2e-139">In **Esplora soluzioni**fare clic sul progetto **MvcAuth** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="9cf2e-140">Premere il tasto F4 per visualizzare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="9cf2e-141">In alternativa, dal menu **Visualizza** è possibile selezionare **finestra Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="9cf2e-142">Impostare **SSL abilitato** su true.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="9cf2e-143">Copiare l'URL SSL (che verrà `https://localhost:44300/` a meno che non siano stati creati altri progetti SSL).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="9cf2e-144">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **MvcAuth** e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="9cf2e-145">Selezionare la scheda **Web** , quindi incollare l'URL SSL nella casella **URL progetto** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="9cf2e-146">Salvare il file (CTL + S).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="9cf2e-147">Questo URL sarà necessario per configurare le app di autenticazione di Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="9cf2e-148">Aggiungere l'attributo [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al controller di `Home` per richiedere che tutte le richieste usino HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="9cf2e-149">Un approccio più sicuro consiste nell'aggiungere il filtro [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="9cf2e-150">Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo di autorizzazione&quot; nell'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla in app Azure Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="9cf2e-151">Di seguito è riportata una parte del controller Home.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="9cf2e-152">Premere CTRL+F5 per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="9cf2e-153">Se il certificato è stato installato in passato, è possibile ignorare il resto di questa sezione e passare alla [creazione di un'app Google per OAuth 2 e connettere l'app al progetto](#goog). in caso contrario, seguire le istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="9cf2e-154">Leggere la finestra di dialogo **avviso di sicurezza** e quindi fare clic su **Sì** se si vuole installare il certificato che rappresenta localhost.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="9cf2e-155">In Internet Explorer viene visualizzata la pagina *Home* e non viene visualizzato alcun avviso SSL.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="9cf2e-156">Google Chrome accetta anche il certificato e visualizza il contenuto HTTPS senza un avviso.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="9cf2e-157">Firefox usa il proprio archivio certificati, quindi verrà visualizzato un avviso.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="9cf2e-158">Per l'applicazione è possibile fare clic con il pulsante destro del mouse sul **rischio**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="9cf2e-159">Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto</span><span class="sxs-lookup"><span data-stu-id="9cf2e-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="9cf2e-160">Per le istruzioni di Google OAuth correnti, vedere la pagina relativa alla [configurazione dell'autenticazione di Google in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="9cf2e-161">Passare a [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="9cf2e-162">Se non è ancora stato creato un progetto, selezionare le **credenziali** nella scheda a sinistra e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="9cf2e-163">Nella scheda a sinistra fare clic su **credenziali**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="9cf2e-164">Fare clic su **create Credentials** Then **OAuth client ID**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="9cf2e-165">Nella finestra di dialogo **Crea ID client** , salvare l' **applicazione Web** predefinita per il tipo di applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="9cf2e-166">Impostare le origini **JavaScript autorizzate** sull'URL SSL usato sopra (`https://localhost:44300/`, a meno che non siano stati creati altri progetti SSL)</span><span class="sxs-lookup"><span data-stu-id="9cf2e-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="9cf2e-167">Impostare l' **URI di reindirizzamento autorizzato** su:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="9cf2e-168">Fare clic sulla voce di menu della schermata di consenso OAuth, quindi impostare l'indirizzo di posta elettronica e il nome del prodotto.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="9cf2e-169">Una volta completato il modulo, fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="9cf2e-170">Fare clic sulla voce di menu libreria, cercare **Google + API**, fare clic su di essa e quindi premere Abilita.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="9cf2e-171">L'immagine seguente mostra le API abilitate.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="9cf2e-172">Da gestione API Google API, visitare la scheda **credenziali** per ottenere l' **ID client**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="9cf2e-173">Scaricare per salvare un file JSON con i segreti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="9cf2e-174">Copiare e incollare i **ClientID** e **ClientSecret** nel metodo `UseGoogleAuthentication` trovato nel file *Startup.auth.cs* nella cartella *app_start* .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="9cf2e-175">I valori **ClientID** e **ClientSecret** riportati di seguito sono esempi e non funzionano.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="9cf2e-176">Sicurezza: non archiviare mai dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="9cf2e-177">L'account e le credenziali vengono aggiunti al codice riportato sopra per semplificare l'esempio.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="9cf2e-178">Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili a ASP.NET e al servizio app Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="9cf2e-179">Premere **CTRL+F5** per compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="9cf2e-180">Fare clic sul collegamento **Accedi** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="9cf2e-181">In **Usa un altro servizio per accedere**fare clic su **Google**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="9cf2e-182">Se non si verificano i passaggi precedenti, verrà ricevuto un errore HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="9cf2e-183">Controllare nuovamente i passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-183">Recheck your steps above.</span></span> <span data-ttu-id="9cf2e-184">Se non si dispone di un'impostazione obbligatoria (ad esempio **nome prodotto**), aggiungere l'elemento mancante e salvare; per il corretto funzionamento dell'autenticazione possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="9cf2e-185">Si verrà reindirizzati al sito Google in cui verranno immesse le credenziali.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="9cf2e-186">Dopo avere immesso le credenziali, verrà richiesto di concedere le autorizzazioni all'applicazione Web appena creata:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="9cf2e-187">Fare clic su **Accetta**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-187">Click **Accept**.</span></span> <span data-ttu-id="9cf2e-188">A questo punto si verrà reindirizzati alla pagina **Register** dell'applicazione MvcAuth in cui è possibile registrare il proprio account Google.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="9cf2e-189">Sarà possibile modificare il nome di registrazione dell'indirizzo di posta elettronica locale usato per l'account Gmail, anche se in generale è preferibile mantenere l'alias di posta elettronica predefinito, ovvero quello usato per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="9cf2e-190">Fare clic su **Registra**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="9cf2e-191">Creazione dell'app in Facebook e connessione dell'app al progetto</span><span class="sxs-lookup"><span data-stu-id="9cf2e-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="9cf2e-192">Per le attuali istruzioni di autenticazione di Facebook OAuth2, vedere [configurazione dell'autenticazione Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="9cf2e-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="9cf2e-193">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="9cf2e-193">Examine the Membership Data</span></span>

<span data-ttu-id="9cf2e-194">Scegliere **Esplora server**dal menu **Visualizza** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="9cf2e-195">Espandere **DefaultConnection (MvcAuth)** , espandere **tabelle**, fare clic con il pulsante destro del mouse su **AspNetUsers** e scegliere **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="9cf2e-197">Aggiunta di dati di profilo alla classe utente</span><span class="sxs-lookup"><span data-stu-id="9cf2e-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="9cf2e-198">In questa sezione si aggiungeranno la data di nascita e la Home page ai dati utente durante la registrazione, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg con Home Town e compleanno](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="9cf2e-200">Aprire il file *Models\IdentityModels.cs* e aggiungere le proprietà date di nascita e Home Town:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="9cf2e-201">Aprire il file *Models\AccountViewModels.cs* e le proprietà set birth date e Home town in `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="9cf2e-202">Aprire il file *Controllers\AccountController.cs* e aggiungere il codice per la data di nascita e la Home Page nel metodo di azione `ExternalLoginConfirmation` come illustrato:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="9cf2e-203">Aggiungere la data di nascita e la Home page al file *Views\Account\ExternalLoginConfirmation.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9cf2e-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="9cf2e-204">Eliminare il database delle appartenenze in modo che sia possibile registrare di nuovo l'account Facebook con l'applicazione e verificare che sia possibile aggiungere le nuove informazioni relative alla data di nascita e al profilo della città Home.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="9cf2e-205">Da **Esplora soluzioni**fare clic sull'icona **Mostra tutti i file** , quindi fare clic con il pulsante destro del mouse su *aggiungi\_Data\aspnet-MvcAuth-&lt;datestamp&gt;. MDF* e scegliere **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="9cf2e-206">Dal menu **strumenti** fare clic su gestione **pacchetti NuGet**, quindi su **console di gestione pacchetti** (PMC).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="9cf2e-207">Immettere i comandi seguenti nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="9cf2e-208">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="9cf2e-208">Enable-Migrations</span></span>
2. <span data-ttu-id="9cf2e-209">Aggiunta-migrazione init</span><span class="sxs-lookup"><span data-stu-id="9cf2e-209">Add-Migration Init</span></span>
3. <span data-ttu-id="9cf2e-210">Update-database</span><span class="sxs-lookup"><span data-stu-id="9cf2e-210">Update-Database</span></span>

<span data-ttu-id="9cf2e-211">Eseguire l'applicazione e usare FaceBook e Google per accedere e registrare alcuni utenti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="9cf2e-212">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="9cf2e-212">Examine the Membership Data</span></span>

<span data-ttu-id="9cf2e-213">Scegliere **Esplora server**dal menu **Visualizza** .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="9cf2e-214">Fare clic con il pulsante destro del mouse su **AspNetUsers** e scegliere **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="9cf2e-215">I campi `HomeTown` e `BirthDate` sono visualizzati di seguito.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="9cf2e-216">Disconnettere l'app ed effettuare l'accesso con un altro account</span><span class="sxs-lookup"><span data-stu-id="9cf2e-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="9cf2e-217">Se si accede all'app con Facebook, e si esegue la disconnessione e si tenta di accedere di nuovo con un account Facebook diverso (usando lo stesso browser), si accederà immediatamente all'account Facebook precedente usato.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="9cf2e-218">Per usare un altro account, è necessario passare a Facebook e disconnettersi da Facebook.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="9cf2e-219">La stessa regola si applica a qualsiasi altro provider di autenticazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="9cf2e-220">In alternativa, è possibile accedere con un altro account usando un browser diverso.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cf2e-221">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9cf2e-221">Next Steps</span></span>

<span data-ttu-id="9cf2e-222">Vedere [Introducing the Yahoo and LinkedIn OAuth Security Providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) di Jerrie Pelser for Yahoo and LinkedIn instructions.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="9cf2e-223">Vedere i pulsanti di accesso di social networking Jerrie per ASP.NET MVC 5 per ottenere i pulsanti Abilita accesso social.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="9cf2e-224">Seguire l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua questa esercitazione e Mostra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="9cf2e-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="9cf2e-225">Come distribuire l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="9cf2e-226">Come proteggere l'app con i ruoli.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="9cf2e-227">Come proteggere l'app con i filtri [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizza](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) .</span><span class="sxs-lookup"><span data-stu-id="9cf2e-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="9cf2e-228">Come usare l'API di appartenenza per aggiungere utenti e ruoli.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="9cf2e-229">Invia commenti e suggerimenti su come questa esercitazione è stata apprezzata e cosa possiamo migliorare.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="9cf2e-230">È anche possibile richiedere nuovi argomenti in [Mostra come usare il codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="9cf2e-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="9cf2e-231">È anche possibile richiedere e votare le nuove funzionalità da aggiungere a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="9cf2e-232">È possibile, ad esempio, votare uno strumento per la [creazione e la gestione di utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="9cf2e-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="9cf2e-233">Per una spiegazione corretta del funzionamento di ASP.NET External Authentication Services, vedere [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services)di Robert McMurray.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="9cf2e-234">Nell'articolo di Robert viene inoltre illustrato in dettaglio l'abilitazione dell'autenticazione Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="9cf2e-235">L'eccellente [esercitazione EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) di Tom Dykstra illustra come usare la Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cf2e-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
