---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: appartenenza e autorizzazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 7 riguarda l'appartenenza e l'autorizzazione.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539200"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="94e90-104">Parte 7: appartenenza e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="94e90-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="94e90-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="94e90-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="94e90-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="94e90-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="94e90-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="94e90-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="94e90-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="94e90-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="94e90-109">La parte 7 riguarda l'appartenenza e l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="94e90-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="94e90-110">Il controller di Store Manager è attualmente accessibile a chiunque visiti il sito.</span><span class="sxs-lookup"><span data-stu-id="94e90-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="94e90-111">Modificare questa opzione per limitare l'autorizzazione agli amministratori del sito.</span><span class="sxs-lookup"><span data-stu-id="94e90-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="94e90-112">Aggiunta di AccountController e views</span><span class="sxs-lookup"><span data-stu-id="94e90-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="94e90-113">Una differenza tra il modello di applicazione Web ASP.NET MVC 3 completo e il modello di applicazione Web vuota ASP.NET MVC 3 è che il modello vuoto non include un controller di account.</span><span class="sxs-lookup"><span data-stu-id="94e90-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="94e90-114">Si aggiungerà un controller di account copiando alcuni file da una nuova applicazione MVC ASP.NET creata dal modello di applicazione Web ASP.NET MVC 3 completo.</span><span class="sxs-lookup"><span data-stu-id="94e90-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="94e90-115">Creare una nuova applicazione MVC ASP.NET usando il modello di applicazione Web ASP.NET MVC 3 completo e copiare i file seguenti nelle stesse directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="94e90-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="94e90-116">Copiare AccountController.cs nella directory Controllers</span><span class="sxs-lookup"><span data-stu-id="94e90-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="94e90-117">Copiare AccountModels nella directory Models</span><span class="sxs-lookup"><span data-stu-id="94e90-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="94e90-118">Creare una directory dell'account all'interno della directory views e copiare tutte e quattro le visualizzazioni in</span><span class="sxs-lookup"><span data-stu-id="94e90-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="94e90-119">Modificare lo spazio dei nomi per le classi controller e modello in modo che inizino con MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="94e90-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="94e90-120">La classe AccountController deve usare lo spazio dei nomi MvcMusicStore. Controllers e la classe AccountModels deve usare lo spazio dei nomi MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="94e90-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="94e90-121">*Nota: questi file sono disponibili anche nel download MvcMusicStore-Assets. zip dal quale sono stati copiati i file di progettazione del sito all'inizio dell'esercitazione. I file di appartenenza si trovano nella directory del codice.*</span><span class="sxs-lookup"><span data-stu-id="94e90-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="94e90-122">La soluzione aggiornata dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="94e90-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="94e90-123">Aggiunta di un utente amministratore con il sito di configurazione di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="94e90-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="94e90-124">Prima di richiedere l'autorizzazione nel sito Web, è necessario creare un utente con accesso.</span><span class="sxs-lookup"><span data-stu-id="94e90-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="94e90-125">Il modo più semplice per creare un utente consiste nell'usare il sito Web di configurazione di ASP.NET incorporato.</span><span class="sxs-lookup"><span data-stu-id="94e90-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="94e90-126">Avviare il sito Web di configurazione di ASP.NET facendo clic sull'icona seguente nell'Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="94e90-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="94e90-127">Verrà avviato un sito Web di configurazione.</span><span class="sxs-lookup"><span data-stu-id="94e90-127">This launches a configuration website.</span></span> <span data-ttu-id="94e90-128">Fare clic sulla scheda sicurezza nella schermata iniziale, quindi fare clic sul collegamento "Abilita ruoli" al centro della schermata.</span><span class="sxs-lookup"><span data-stu-id="94e90-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="94e90-129">Fare clic sul collegamento "crea o Gestisci ruoli".</span><span class="sxs-lookup"><span data-stu-id="94e90-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="94e90-130">Immettere "Administrator" come nome del ruolo e premere il pulsante Aggiungi ruolo.</span><span class="sxs-lookup"><span data-stu-id="94e90-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="94e90-131">Fai clic sul pulsante indietro, quindi fai clic sul collegamento Crea utente sul lato sinistro.</span><span class="sxs-lookup"><span data-stu-id="94e90-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="94e90-132">Inserire i campi delle informazioni utente a sinistra usando le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="94e90-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="94e90-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="94e90-133">**Field**</span></span> | <span data-ttu-id="94e90-134">**Valore**</span><span class="sxs-lookup"><span data-stu-id="94e90-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="94e90-135">**Nome utente**</span><span class="sxs-lookup"><span data-stu-id="94e90-135">**User Name**</span></span> | <span data-ttu-id="94e90-136">Amministratore</span><span class="sxs-lookup"><span data-stu-id="94e90-136">Administrator</span></span> |
| <span data-ttu-id="94e90-137">**Password**</span><span class="sxs-lookup"><span data-stu-id="94e90-137">**Password**</span></span> | <span data-ttu-id="94e90-138">password123!</span><span class="sxs-lookup"><span data-stu-id="94e90-138">password123!</span></span> |
| <span data-ttu-id="94e90-139">**Conferma password**</span><span class="sxs-lookup"><span data-stu-id="94e90-139">**Confirm Password**</span></span> | <span data-ttu-id="94e90-140">password123!</span><span class="sxs-lookup"><span data-stu-id="94e90-140">password123!</span></span> |
| <span data-ttu-id="94e90-141">**Posta elettronica**</span><span class="sxs-lookup"><span data-stu-id="94e90-141">**E-mail**</span></span> | <span data-ttu-id="94e90-142">(qualsiasi indirizzo di posta elettronica funzionerà)</span><span class="sxs-lookup"><span data-stu-id="94e90-142">(any email address will work)</span></span> |
| <span data-ttu-id="94e90-143">**Domanda segreta**</span><span class="sxs-lookup"><span data-stu-id="94e90-143">**Security Question**</span></span> | <span data-ttu-id="94e90-144">(qualsiasi cosa piaccia)</span><span class="sxs-lookup"><span data-stu-id="94e90-144">(whatever you like)</span></span> |
| <span data-ttu-id="94e90-145">**Risposta segreta**</span><span class="sxs-lookup"><span data-stu-id="94e90-145">**Security Answer**</span></span> | <span data-ttu-id="94e90-146">(qualsiasi cosa piaccia)</span><span class="sxs-lookup"><span data-stu-id="94e90-146">(whatever you like)</span></span> |

<span data-ttu-id="94e90-147">*Nota: naturalmente, è possibile usare qualsiasi password. Le impostazioni di sicurezza predefinite per la password richiedono una password con una lunghezza di 7 caratteri e contengono un carattere non alfanumerico.*</span><span class="sxs-lookup"><span data-stu-id="94e90-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="94e90-148">Selezionare il ruolo di amministratore per l'utente e fare clic sul pulsante Crea utente.</span><span class="sxs-lookup"><span data-stu-id="94e90-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="94e90-149">A questo punto, verrà visualizzato un messaggio che indica che l'utente è stato creato correttamente.</span><span class="sxs-lookup"><span data-stu-id="94e90-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="94e90-150">È ora possibile chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="94e90-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="94e90-151">Autorizzazione basata sui ruoli</span><span class="sxs-lookup"><span data-stu-id="94e90-151">Role-based Authorization</span></span>

<span data-ttu-id="94e90-152">È ora possibile limitare l'accesso a StoreManagerController usando l'attributo [autorizzate], specificando che l'utente deve avere il ruolo di amministratore per accedere a qualsiasi azione del controller nella classe.</span><span class="sxs-lookup"><span data-stu-id="94e90-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="94e90-153">*Nota: l'attributo [autorizzate] può essere posizionato su metodi di azione specifici, oltre che a livello di classe del controller.*</span><span class="sxs-lookup"><span data-stu-id="94e90-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="94e90-154">Ora passare a/StoreManager Visualizza una finestra di dialogo di accesso:</span><span class="sxs-lookup"><span data-stu-id="94e90-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="94e90-155">Dopo aver eseguito l'accesso con il nuovo account amministratore, è possibile passare alla schermata di modifica dell'album come prima.</span><span class="sxs-lookup"><span data-stu-id="94e90-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="94e90-156">[Precedente](mvc-music-store-part-6.md)
> [Successivo](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="94e90-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
