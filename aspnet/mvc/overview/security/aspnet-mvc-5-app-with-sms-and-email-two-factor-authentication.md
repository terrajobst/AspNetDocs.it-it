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
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="53661-104">App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica</span><span class="sxs-lookup"><span data-stu-id="53661-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="53661-105">di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53661-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="53661-106">Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 con l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="53661-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="53661-107">Prima di procedere, è necessario completare [la creazione di un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) .</span><span class="sxs-lookup"><span data-stu-id="53661-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="53661-108">È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="53661-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="53661-109">Il download contiene helper di debug che consentono di testare la conferma della posta elettronica e SMS senza configurare un messaggio di posta elettronica o un provider SMS.</span><span class="sxs-lookup"><span data-stu-id="53661-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="53661-110">Questa esercitazione è stata scritta da [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="53661-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="53661-111">Creare un'app MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53661-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="53661-112">Configurare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="53661-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="53661-113">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="53661-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="53661-114">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53661-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="53661-115">Creare un'app MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53661-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="53661-116">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="53661-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="53661-117">Installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="53661-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="53661-118">Avviso: è necessario completare [la creazione di un'app web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password prima di](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) procedere.</span><span class="sxs-lookup"><span data-stu-id="53661-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="53661-119">Per completare questa esercitazione, è necessario installare [Visual Studio 2013 aggiornamento 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="53661-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="53661-120">Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="53661-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="53661-121">Web Forms supporta anche ASP.NET Identity, quindi è possibile seguire una procedura simile in un'app Web Form.</span><span class="sxs-lookup"><span data-stu-id="53661-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="53661-122">Lasciare l'autenticazione predefinita come **account utente singoli**.</span><span class="sxs-lookup"><span data-stu-id="53661-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="53661-123">Se si vuole ospitare l'app in Azure, lasciare selezionata la casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="53661-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="53661-124">Più avanti nell'esercitazione si distribuirà in Azure.</span><span class="sxs-lookup"><span data-stu-id="53661-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="53661-125">È possibile [aprire un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)gratuitamente.</span><span class="sxs-lookup"><span data-stu-id="53661-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="53661-126">Impostare il [progetto per l'utilizzo di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="53661-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="53661-127">Configurare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="53661-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="53661-128">Questa esercitazione fornisce istruzioni per l'uso di Twilio o ASPSMS, ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="53661-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="53661-129">**Creazione di un account utente con un provider SMS**</span><span class="sxs-lookup"><span data-stu-id="53661-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="53661-130">Creare un account [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="53661-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="53661-131">**Installazione di pacchetti aggiuntivi o aggiunta di riferimenti al servizio**</span><span class="sxs-lookup"><span data-stu-id="53661-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="53661-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="53661-132">Twilio:</span></span>  
   <span data-ttu-id="53661-133">Nella Console di Gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="53661-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="53661-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="53661-134">ASPSMS:</span></span>  
   <span data-ttu-id="53661-135">È necessario aggiungere il seguente riferimento al servizio:</span><span class="sxs-lookup"><span data-stu-id="53661-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="53661-136">Indirizzo:</span><span class="sxs-lookup"><span data-stu-id="53661-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="53661-137">Spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="53661-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="53661-138">**Individuazione delle credenziali utente del provider SMS**</span><span class="sxs-lookup"><span data-stu-id="53661-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="53661-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="53661-139">Twilio:</span></span>  
   <span data-ttu-id="53661-140">Dalla scheda **Dashboard** dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="53661-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="53661-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="53661-141">ASPSMS:</span></span>  
   <span data-ttu-id="53661-142">Dalle impostazioni dell'account, passare a **userKey** e copiarlo insieme alla **password**definita autonomamente.</span><span class="sxs-lookup"><span data-stu-id="53661-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="53661-143">Questi valori vengono archiviati in un secondo momento nel file *Web. config* all'interno delle chiavi `"SMSAccountIdentification"` e `"SMSAccountPassword"`.</span><span class="sxs-lookup"><span data-stu-id="53661-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="53661-144">**Specifica di SenderID/originator**</span><span class="sxs-lookup"><span data-stu-id="53661-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="53661-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="53661-145">Twilio:</span></span>  
   <span data-ttu-id="53661-146">Dalla scheda **numeri** copiare il numero di telefono Twilio.</span><span class="sxs-lookup"><span data-stu-id="53661-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="53661-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="53661-147">ASPSMS:</span></span>  
   <span data-ttu-id="53661-148">Nel menu **Sblocca autori** sbloccare uno o più autori o scegliere un originatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="53661-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="53661-149">Il valore verrà archiviato in un secondo momento nel file *Web. config* all'interno della chiave `"SMSAccountFrom"`.</span><span class="sxs-lookup"><span data-stu-id="53661-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="53661-150">**Trasferimento delle credenziali del provider SMS nell'app**</span><span class="sxs-lookup"><span data-stu-id="53661-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="53661-151">Rendere disponibili le credenziali e il numero di telefono del mittente per l'app.</span><span class="sxs-lookup"><span data-stu-id="53661-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="53661-152">Per semplificare le operazioni, questi valori vengono archiviati nel file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="53661-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="53661-153">Quando si esegue la distribuzione in Azure, è possibile archiviare i valori in modo sicuro nella sezione **impostazioni app** della scheda Configura del sito Web.</span><span class="sxs-lookup"><span data-stu-id="53661-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="53661-154">Sicurezza: non archiviare mai dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="53661-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="53661-155">L'account e le credenziali vengono aggiunti al codice riportato sopra per semplificare l'esempio.</span><span class="sxs-lookup"><span data-stu-id="53661-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="53661-156">Vedere [le procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="53661-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="53661-157">**Implementazione del trasferimento dati al provider SMS**</span><span class="sxs-lookup"><span data-stu-id="53661-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="53661-158">Configurare la classe `SmsService` nel file *App\_Start\IdentityConfig.cs* .</span><span class="sxs-lookup"><span data-stu-id="53661-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="53661-159">A seconda del provider SMS utilizzato, attivare la sezione **Twilio** o **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="53661-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="53661-160">Aggiornare la visualizzazione Razor di *Views\Manage\Index.cshtml* : (Nota: non rimuovere solo i commenti nel codice di uscita, usare il codice seguente).</span><span class="sxs-lookup"><span data-stu-id="53661-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="53661-161">Verificare che i metodi di azione `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` nel `ManageController` dispongano dell'attributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :</span><span class="sxs-lookup"><span data-stu-id="53661-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="53661-162">Eseguire l'app e accedere con l'account registrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="53661-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="53661-163">Fare clic sull'ID utente, che attiva il metodo di azione `Index` in `Manage` controller.</span><span class="sxs-lookup"><span data-stu-id="53661-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="53661-164">Scegli Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="53661-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="53661-165">Il metodo di azione `AddPhoneNumber` Visualizza una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="53661-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="53661-166">In pochi secondi si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="53661-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="53661-167">Immetterla e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="53661-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="53661-168">La visualizzazione gestione mostra che è stato aggiunto il numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="53661-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="53661-169">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="53661-169">Enable two-factor authentication</span></span>

<span data-ttu-id="53661-170">Nell'app generata dal modello è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA).</span><span class="sxs-lookup"><span data-stu-id="53661-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="53661-171">Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="53661-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="53661-172">Fare clic su Abilita 2FA.</span><span class="sxs-lookup"><span data-stu-id="53661-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="53661-173">Disconnettersi, quindi accedere di nuovo.</span><span class="sxs-lookup"><span data-stu-id="53661-173">Logout, then log back in.</span></span> <span data-ttu-id="53661-174">Se è stata abilitata la posta elettronica (vedere l' [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare SMS o messaggio di posta elettronica per 2FA.</span><span class="sxs-lookup"><span data-stu-id="53661-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="53661-175">Viene visualizzata la pagina Verifica codice in cui è possibile immettere il codice (da SMS o posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="53661-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="53661-176">Se si fa clic sulla casella di controllo **memorizza questo browser** , non sarà necessario usare 2FA per accedere quando si usa il browser e il dispositivo in cui è stata selezionata la casella.</span><span class="sxs-lookup"><span data-stu-id="53661-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="53661-177">Se gli utenti malintenzionati non riescono ad accedere al dispositivo, abilitando 2FA e facendo clic sul pulsante **memorizza questo browser** fornirà un comodo accesso alla password, mantenendo al tempo stesso la protezione 2FA avanzata per tutti gli accessi da dispositivi non attendibili.</span><span class="sxs-lookup"><span data-stu-id="53661-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="53661-178">È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="53661-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="53661-179">Questa esercitazione offre una rapida introduzione all'abilitazione di 2FA in una nuova app ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="53661-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="53661-180">L' [autenticazione a due fattori dell'esercitazione con SMS e messaggi di posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) viene illustrata in dettaglio nel codice sottostante l'esempio.</span><span class="sxs-lookup"><span data-stu-id="53661-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="53661-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53661-181">Additional Resources</span></span>

- <span data-ttu-id="53661-182">[Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Viene illustrato in dettaglio l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="53661-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="53661-183">Collegamenti a ASP.NET Identity risorse consigliate</span><span class="sxs-lookup"><span data-stu-id="53661-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="53661-184">[Conferma dell'account e recupero della password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Esamina più dettagliatamente il recupero delle password e la conferma dell'account.</span><span class="sxs-lookup"><span data-stu-id="53661-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="53661-185">[App MVC 5 con accesso a Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con l'autorizzazione Facebook e Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="53661-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="53661-186">Viene inoltre illustrato come aggiungere altri dati al database di identità.</span><span class="sxs-lookup"><span data-stu-id="53661-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="53661-187">[Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="53661-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="53661-188">Questa esercitazione aggiunge la distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="53661-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="53661-189">Creazione di un'app Google per OAuth 2 e connessione dell'app al progetto</span><span class="sxs-lookup"><span data-stu-id="53661-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="53661-190">Creazione dell'app in Facebook e connessione dell'app al progetto</span><span class="sxs-lookup"><span data-stu-id="53661-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="53661-191">Configurazione di SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="53661-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="53661-192">Come configurare l' C# ambiente di sviluppo MVC e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53661-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
