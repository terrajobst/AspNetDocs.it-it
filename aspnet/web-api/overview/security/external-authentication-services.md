---
uid: web-api/overview/security/external-authentication-services
title: Servizi di autenticazione esterni con API Web ASP.NETC#() | Microsoft Docs
author: rmcmurray
description: Viene descritto l'utilizzo di servizi di autenticazione esterni in API Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555475"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="1d174-103">Servizi di autenticazione esterni con API Web ASP.NETC#()</span><span class="sxs-lookup"><span data-stu-id="1d174-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="1d174-104">Visual Studio 2017 e ASP.NET 4.7.2 espandere le opzioni di sicurezza per [le applicazioni a pagina singola](../../../single-page-application/index.md) (Spa) e i servizi [API Web](../../index.md) per l'integrazione con i servizi di autenticazione esterni, che includono diversi servizi di autenticazione OAuth/OpenID e social media: account Microsoft, Twitter, Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="1d174-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="1d174-105">In questa procedura dettagliata</span><span class="sxs-lookup"><span data-stu-id="1d174-105">In this Walkthrough</span></span>

- [<span data-ttu-id="1d174-106">Uso di servizi di autenticazione esterni</span><span class="sxs-lookup"><span data-stu-id="1d174-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="1d174-107">Creazione dell'applicazione Web di esempio</span><span class="sxs-lookup"><span data-stu-id="1d174-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="1d174-108">Abilitazione dell'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="1d174-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="1d174-109">Abilitazione dell'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="1d174-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="1d174-110">Abilitazione dell'autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="1d174-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="1d174-111">Abilitazione dell'autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="1d174-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="1d174-112">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1d174-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="1d174-113">Combinazione di servizi di autenticazione esterni</span><span class="sxs-lookup"><span data-stu-id="1d174-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="1d174-114">Configurazione di IIS Express per l'utilizzo di un nome di dominio completo</span><span class="sxs-lookup"><span data-stu-id="1d174-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="1d174-115">Come ottenere le impostazioni dell'applicazione per l'autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="1d174-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="1d174-116">Facoltativo: disabilitare la registrazione locale</span><span class="sxs-lookup"><span data-stu-id="1d174-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="1d174-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1d174-117">Prerequisites</span></span>

<span data-ttu-id="1d174-118">Per seguire gli esempi di questa procedura dettagliata, è necessario disporre di quanto segue:</span><span class="sxs-lookup"><span data-stu-id="1d174-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="1d174-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1d174-119">Visual Studio 2017</span></span>
- <span data-ttu-id="1d174-120">Un account sviluppatore con l'identificatore dell'applicazione e la chiave privata per uno dei servizi di autenticazione dei social media seguenti:</span><span class="sxs-lookup"><span data-stu-id="1d174-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="1d174-121">Account Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="1d174-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="1d174-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="1d174-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="1d174-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="1d174-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="1d174-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="1d174-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="1d174-125">Uso di servizi di autenticazione esterni</span><span class="sxs-lookup"><span data-stu-id="1d174-125">Using External Authentication Services</span></span>

<span data-ttu-id="1d174-126">L'abbondanza di servizi di autenticazione esterni attualmente disponibili per gli sviluppatori Web contribuisce a ridurre i tempi di sviluppo quando si creano nuove applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="1d174-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="1d174-127">Gli utenti Web hanno in genere diversi account esistenti per i servizi Web più diffusi e i siti Web di social media, pertanto quando un'applicazione Web implementa i servizi di autenticazione da un servizio Web esterno o da un sito Web di social media, Salva il tempo di sviluppo che sarebbe stata impiegata la creazione di un'implementazione di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1d174-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="1d174-128">L'utilizzo di un servizio di autenticazione esterno consente agli utenti finali di creare un altro account per l'applicazione Web e di dover ricordare un altro nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="1d174-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="1d174-129">In passato, gli sviluppatori hanno avuto due opzioni: creare una propria implementazione di autenticazione o apprendere come integrare un servizio di autenticazione esterno nelle proprie applicazioni.</span><span class="sxs-lookup"><span data-stu-id="1d174-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="1d174-130">Al livello più elementare, il diagramma seguente illustra un semplice flusso di richieste per un agente utente (Web browser) che richiede informazioni da un'applicazione Web configurata per l'utilizzo di un servizio di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="1d174-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="1d174-131">Nel diagramma precedente, l'agente utente (o il Web browser in questo esempio) Invia una richiesta a un'applicazione Web, che reindirizza il Web browser a un servizio di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="1d174-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="1d174-132">L'agente utente invia le proprie credenziali al servizio di autenticazione esterno e, se l'agente utente è stato autenticato correttamente, il servizio di autenticazione esterno reindirizza l'agente utente all'applicazione Web originale con una forma di token che l'agente utente invierà all'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="1d174-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="1d174-133">L'applicazione Web utilizzerà il token per verificare che l'agente utente sia stato autenticato correttamente dal servizio di autenticazione esterno e che l'applicazione Web possa usare il token per raccogliere altre informazioni sull'agente utente.</span><span class="sxs-lookup"><span data-stu-id="1d174-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="1d174-134">Una volta che l'applicazione ha completato l'elaborazione delle informazioni dell'agente utente, l'applicazione Web restituirà la risposta appropriata all'agente utente in base alle impostazioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="1d174-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="1d174-135">In questo secondo esempio, l'agente utente negozia con l'applicazione Web e il server di autorizzazione esterno e l'applicazione Web esegue comunicazioni aggiuntive con il server di autorizzazione esterno per recuperare informazioni aggiuntive sull'utente agente</span><span class="sxs-lookup"><span data-stu-id="1d174-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="1d174-136">Visual Studio 2017 e ASP.NET 4.7.2 semplificano l'integrazione con i servizi di autenticazione esterni per gli sviluppatori grazie all'integrazione predefinita per i servizi di autenticazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="1d174-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="1d174-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="1d174-137">Facebook</span></span>
- <span data-ttu-id="1d174-138">Google</span><span class="sxs-lookup"><span data-stu-id="1d174-138">Google</span></span>
- <span data-ttu-id="1d174-139">Account Microsoft (account Windows Live ID)</span><span class="sxs-lookup"><span data-stu-id="1d174-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="1d174-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="1d174-140">Twitter</span></span>

<span data-ttu-id="1d174-141">Gli esempi in questa procedura dettagliata illustrano come configurare ognuno dei servizi di autenticazione esterni supportati usando il nuovo modello di applicazione Web ASP.NET fornito con Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1d174-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="1d174-142">Se necessario, potrebbe essere necessario aggiungere il nome di dominio completo alle impostazioni per il servizio di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="1d174-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="1d174-143">Questo requisito si basa sui vincoli di sicurezza per alcuni servizi di autenticazione esterni che richiedono che il nome di dominio completo nelle impostazioni dell'applicazione corrisponda al nome di dominio completo utilizzato dai client.</span><span class="sxs-lookup"><span data-stu-id="1d174-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="1d174-144">I passaggi per questa operazione variano significativamente per ogni servizio di autenticazione esterno. per verificare se è necessario e come configurare queste impostazioni, è necessario consultare la documentazione di ogni servizio di autenticazione esterno. Se è necessario configurare IIS Express per l'uso di un FQDN per il test di questo ambiente, vedere la sezione [configurazione IIS Express per l'uso di un nome di dominio](#FQDN) completo più avanti in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="1d174-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="1d174-145">Creare un'applicazione Web di esempio</span><span class="sxs-lookup"><span data-stu-id="1d174-145">Create a Sample Web Application</span></span>

<span data-ttu-id="1d174-146">La procedura seguente consente di creare un'applicazione di esempio usando il modello di applicazione Web ASP.NET. questa applicazione di esempio verrà usata per ognuno dei servizi di autenticazione esterni, più avanti in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="1d174-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="1d174-147">Avviare Visual Studio 2017 e selezionare **nuovo progetto** nella pagina iniziale.</span><span class="sxs-lookup"><span data-stu-id="1d174-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="1d174-148">In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="1d174-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="1d174-149">Quando viene visualizzata la finestra di dialogo **nuovo progetto** , selezionare **installato** ed espandere oggetto **visivo C#** .</span><span class="sxs-lookup"><span data-stu-id="1d174-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="1d174-150">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="1d174-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1d174-151">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="1d174-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="1d174-152">Immettere un nome per il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d174-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="1d174-153">Quando viene visualizzato il **nuovo progetto ASP.NET** , selezionare il modello **applicazione a pagina singola** e fare clic su **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="1d174-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="1d174-154">Attendere che Visual Studio 2017 crei il progetto.</span><span class="sxs-lookup"><span data-stu-id="1d174-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="1d174-155">Al termine della creazione del progetto in Visual Studio 2017, aprire il file *Startup.auth.cs* che si trova nella cartella **app\_Start** .</span><span class="sxs-lookup"><span data-stu-id="1d174-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="1d174-156">Quando si crea il progetto per la prima volta, nessuno dei servizi di autenticazione esterni viene abilitato nel file *Startup.auth.cs* ; di seguito viene illustrata la somiglianza del codice, con le sezioni evidenziate per la posizione in cui si Abilita un servizio di autenticazione esterno e qualsiasi impostazione pertinente per l'uso di account Microsoft, Twitter, Facebook o Google Authentication con l'applicazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="1d174-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="1d174-157">Quando si preme F5 per compilare ed eseguire il debug dell'applicazione Web, viene visualizzata una schermata di accesso in cui si noterà che non sono stati definiti servizi di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="1d174-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="1d174-158">Nelle sezioni seguenti si apprenderà come abilitare ogni servizio di autenticazione esterno fornito con ASP.NET in Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1d174-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="1d174-159">Abilitazione dell'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="1d174-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="1d174-160">Con l'autenticazione di Facebook è necessario creare un account Facebook Developer e il progetto richiederà un ID applicazione e una chiave privata da Facebook per funzionare.</span><span class="sxs-lookup"><span data-stu-id="1d174-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="1d174-161">Per informazioni sulla creazione di un account Facebook Developer e sull'acquisizione dell'ID applicazione e della chiave privata, vedere [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="1d174-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="1d174-162">Una volta ottenuti l'ID applicazione e la chiave privata, attenersi alla procedura seguente per abilitare l'autenticazione di Facebook per l'applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="1d174-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="1d174-163">Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="1d174-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="1d174-164">Individuare la sezione relativa all'autenticazione di Facebook del codice:</span><span class="sxs-lookup"><span data-stu-id="1d174-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="1d174-165">Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID applicazione e la chiave privata.</span><span class="sxs-lookup"><span data-stu-id="1d174-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="1d174-166">Una volta aggiunti tali parametri, è possibile ricompilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="1d174-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="1d174-167">Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Facebook è stato definito come servizio di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="1d174-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="1d174-168">Quando si fa clic sul pulsante **Facebook** , il browser verrà reindirizzato alla pagina di accesso di Facebook:</span><span class="sxs-lookup"><span data-stu-id="1d174-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="1d174-169">Dopo aver immesso le credenziali di Facebook e fatto clic su **Accedi**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si vuole associare all'account Facebook:</span><span class="sxs-lookup"><span data-stu-id="1d174-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="1d174-170">Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Facebook:</span><span class="sxs-lookup"><span data-stu-id="1d174-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="1d174-171">Abilitazione dell'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="1d174-171">Enabling Google Authentication</span></span>

<span data-ttu-id="1d174-172">Con l'autenticazione di Google è necessario creare un account Google Developer e il progetto richiederà un ID applicazione e una chiave privata da Google per funzionare.</span><span class="sxs-lookup"><span data-stu-id="1d174-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="1d174-173">Per informazioni sulla creazione di un account Google Developer e sull'ottenimento dell'ID applicazione e della chiave privata, vedere [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="1d174-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="1d174-174">Per abilitare l'autenticazione di Google per l'applicazione Web, attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="1d174-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="1d174-175">Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="1d174-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="1d174-176">Individuare la sezione di autenticazione di Google di codice:</span><span class="sxs-lookup"><span data-stu-id="1d174-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="1d174-177">Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID applicazione e la chiave privata.</span><span class="sxs-lookup"><span data-stu-id="1d174-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="1d174-178">Una volta aggiunti tali parametri, è possibile ricompilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="1d174-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="1d174-179">Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Google è stato definito come servizio di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="1d174-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="1d174-180">Quando si fa clic sul pulsante **Google** , il browser verrà reindirizzato alla pagina di accesso di Google:</span><span class="sxs-lookup"><span data-stu-id="1d174-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="1d174-181">Dopo aver immesso le credenziali di Google e aver fatto clic su **Accedi**, Google chiederà di verificare che l'applicazione Web disponga delle autorizzazioni per l'accesso all'account Google:</span><span class="sxs-lookup"><span data-stu-id="1d174-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="1d174-182">Quando si fa clic su **Accetto**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si desidera associare al proprio account Google:</span><span class="sxs-lookup"><span data-stu-id="1d174-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="1d174-183">Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Google:</span><span class="sxs-lookup"><span data-stu-id="1d174-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="1d174-184">Abilitazione dell'autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="1d174-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="1d174-185">L'autenticazione Microsoft richiede la creazione di un account sviluppatore e richiede un ID client e un segreto client per funzionare.</span><span class="sxs-lookup"><span data-stu-id="1d174-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="1d174-186">Per informazioni sulla creazione di un account Microsoft Developer e sull'ottenimento dell'ID client e del segreto client, vedere [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="1d174-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="1d174-187">Una volta ottenuta la chiave utente e il segreto utente, attenersi alla procedura seguente per abilitare l'autenticazione Microsoft per l'applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="1d174-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="1d174-188">Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="1d174-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="1d174-189">Individuare la sezione Microsoft Authentication del codice:</span><span class="sxs-lookup"><span data-stu-id="1d174-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="1d174-190">Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID client e il segreto client.</span><span class="sxs-lookup"><span data-stu-id="1d174-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="1d174-191">Una volta aggiunti tali parametri, è possibile ricompilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="1d174-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="1d174-192">Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Microsoft è stato definito come servizio di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="1d174-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="1d174-193">Quando si fa clic sul pulsante **Microsoft** , il browser verrà reindirizzato alla pagina di accesso di Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1d174-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="1d174-194">Dopo aver immesso le credenziali Microsoft e fatto clic su **Accedi**, verrà richiesto di verificare che l'applicazione Web disponga delle autorizzazioni per accedere ai account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1d174-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="1d174-195">Quando si fa clic su **Sì**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si desidera associare al account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1d174-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="1d174-196">Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per la account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1d174-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="1d174-197">Abilitazione dell'autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="1d174-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="1d174-198">L'autenticazione di Twitter richiede la creazione di un account sviluppatore e richiede una chiave utente e un segreto utente per funzionare.</span><span class="sxs-lookup"><span data-stu-id="1d174-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="1d174-199">Per informazioni sulla creazione di un account sviluppatore di Twitter e sul recupero della chiave utente e del segreto utente, vedere [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="1d174-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="1d174-200">Una volta ottenuta la chiave utente e il segreto utente, attenersi alla procedura seguente per abilitare l'autenticazione di Twitter per l'applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="1d174-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="1d174-201">Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="1d174-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="1d174-202">Individuare la sezione relativa all'autenticazione di Twitter del codice:</span><span class="sxs-lookup"><span data-stu-id="1d174-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="1d174-203">Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere la chiave utente e il segreto utente.</span><span class="sxs-lookup"><span data-stu-id="1d174-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="1d174-204">Una volta aggiunti tali parametri, è possibile ricompilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="1d174-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="1d174-205">Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Twitter è stato definito come servizio di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="1d174-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="1d174-206">Quando si fa clic sul pulsante **Twitter** , il browser verrà reindirizzato alla pagina di accesso di Twitter:</span><span class="sxs-lookup"><span data-stu-id="1d174-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="1d174-207">Dopo aver immesso le credenziali di Twitter e aver fatto clic su **autorizza app**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si vuole associare all'account Twitter:</span><span class="sxs-lookup"><span data-stu-id="1d174-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="1d174-208">Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Twitter:</span><span class="sxs-lookup"><span data-stu-id="1d174-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="1d174-209">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1d174-209">Additional Information</span></span>

<span data-ttu-id="1d174-210">Per ulteriori informazioni sulla creazione di applicazioni che utilizzano OAuth e OpenID, vedere gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="1d174-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="1d174-211">Combinazione di servizi di autenticazione esterni</span><span class="sxs-lookup"><span data-stu-id="1d174-211">Combining External Authentication Services</span></span>

<span data-ttu-id="1d174-212">Per una maggiore flessibilità, è possibile definire contemporaneamente più servizi di autenticazione esterni, in modo da consentire agli utenti dell'applicazione Web di usare un account di uno dei servizi di autenticazione esterni abilitati:</span><span class="sxs-lookup"><span data-stu-id="1d174-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="1d174-213">Configurare IIS Express per l'uso di un nome di dominio completo</span><span class="sxs-lookup"><span data-stu-id="1d174-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="1d174-214">Alcuni provider di autenticazione esterni non supportano il test dell'applicazione utilizzando un indirizzo HTTP come `http://localhost:port/`.</span><span class="sxs-lookup"><span data-stu-id="1d174-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="1d174-215">Per risolvere questo problema, è possibile aggiungere un mapping del nome di dominio completo (FQDN) statico al file degli host e configurare le opzioni del progetto in Visual Studio 2017 per usare l'FQDN per il testing/debug.</span><span class="sxs-lookup"><span data-stu-id="1d174-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="1d174-216">A tale scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="1d174-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="1d174-217">Aggiungere un FQDN statico eseguendo il mapping del file HOSTs:</span><span class="sxs-lookup"><span data-stu-id="1d174-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="1d174-218">Aprire un prompt dei comandi con privilegi elevati in Windows.</span><span class="sxs-lookup"><span data-stu-id="1d174-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="1d174-219">Digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1d174-219">Type the following command:</span></span>

      <span data-ttu-id="1d174-220"><kbd>blocco note%WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="1d174-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="1d174-221">Aggiungere una voce simile alla seguente al file HOSTs:</span><span class="sxs-lookup"><span data-stu-id="1d174-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="1d174-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="1d174-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="1d174-223">Salvare e chiudere il file degli host.</span><span class="sxs-lookup"><span data-stu-id="1d174-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="1d174-224">Configurare il progetto di Visual Studio in modo che usi il nome di dominio completo:</span><span class="sxs-lookup"><span data-stu-id="1d174-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="1d174-225">Quando il progetto è aperto in Visual Studio 2017, fare clic sul menu **progetto** , quindi selezionare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="1d174-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="1d174-226">Ad esempio, è possibile selezionare le **proprietà di WebApplication1**.</span><span class="sxs-lookup"><span data-stu-id="1d174-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="1d174-227">Selezionare la scheda **Web** .</span><span class="sxs-lookup"><span data-stu-id="1d174-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="1d174-228">Immettere il nome di dominio completo per l' <strong>URL del progetto</strong>.</span><span class="sxs-lookup"><span data-stu-id="1d174-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="1d174-229">Ad esempio, immettere <kbd><http://www.wingtiptoys.com></kbd> se si tratta del mapping FQDN aggiunto al file degli host.</span><span class="sxs-lookup"><span data-stu-id="1d174-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="1d174-230">Configurare IIS Express per usare il nome di dominio completo per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="1d174-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="1d174-231">Aprire un prompt dei comandi con privilegi elevati in Windows.</span><span class="sxs-lookup"><span data-stu-id="1d174-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="1d174-232">Digitare il comando seguente per passare alla cartella IIS Express:</span><span class="sxs-lookup"><span data-stu-id="1d174-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="1d174-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="1d174-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="1d174-234">Digitare il comando seguente per aggiungere il nome di dominio completo all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="1d174-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="1d174-235"><kbd>appcmd. exe set config-section: System. applicationHost/sites/+&quot;[Name =' WebApplication1']. Bindings. [Protocol =' http ', bindingInformation =' \*: 80: www. wingtiptoys. com ']&quot;/commit: apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="1d174-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="1d174-236">Dove **WebApplication1** è il nome del progetto e **BindingInformation** contiene il numero di porta e il nome di dominio completo che si desidera utilizzare per il test.</span><span class="sxs-lookup"><span data-stu-id="1d174-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="1d174-237">Come ottenere le impostazioni dell'applicazione per l'autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="1d174-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="1d174-238">Il collegamento di un'applicazione a Windows Live per l'autenticazione Microsoft è un processo semplice.</span><span class="sxs-lookup"><span data-stu-id="1d174-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="1d174-239">Se un'applicazione non è ancora stata collegata a Windows Live, è possibile eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1d174-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="1d174-240">Passare a [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) e immettere il nome e la password del account Microsoft quando richiesto, quindi fare clic su **Accedi**:</span><span class="sxs-lookup"><span data-stu-id="1d174-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="1d174-241">Selezionare **Aggiungi un'app** e immettere il nome dell'applicazione quando richiesto, quindi fare clic su **Crea**:</span><span class="sxs-lookup"><span data-stu-id="1d174-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="1d174-242">Selezionare l'app in **nome** e viene visualizzata la pagina delle proprietà dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1d174-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="1d174-243">Immettere il dominio di reindirizzamento per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1d174-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="1d174-244">Copiare l' **ID applicazione** e, in **segreti applicazione**, selezionare **genera password**.</span><span class="sxs-lookup"><span data-stu-id="1d174-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="1d174-245">Copiare la password visualizzata.</span><span class="sxs-lookup"><span data-stu-id="1d174-245">Copy the password that appears.</span></span> <span data-ttu-id="1d174-246">L'ID e la password dell'applicazione sono l'ID client e il segreto client.</span><span class="sxs-lookup"><span data-stu-id="1d174-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="1d174-247">Selezionare **OK** e quindi **Salva**.</span><span class="sxs-lookup"><span data-stu-id="1d174-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="1d174-248">Facoltativo: disabilitare la registrazione locale</span><span class="sxs-lookup"><span data-stu-id="1d174-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="1d174-249">La funzionalità di registrazione locale di ASP.NET corrente non impedisce la creazione di account membri da parte di programmi automatizzati. ad esempio, usando una tecnologia di prevenzione e convalida bot come [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="1d174-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="1d174-250">Per questo motivo, è necessario rimuovere il modulo di accesso locale e il collegamento di registrazione nella pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="1d174-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="1d174-251">A tale scopo, aprire la pagina *\_login. cshtml* nel progetto, quindi impostare come commento le righe per il pannello di accesso locale e il collegamento di registrazione.</span><span class="sxs-lookup"><span data-stu-id="1d174-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="1d174-252">La pagina risultante dovrebbe essere simile all'esempio di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1d174-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="1d174-253">Una volta disabilitato il pannello di accesso locale e il collegamento di registrazione, nella pagina di accesso vengono visualizzati solo i provider di autenticazione esterni abilitati:</span><span class="sxs-lookup"><span data-stu-id="1d174-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
