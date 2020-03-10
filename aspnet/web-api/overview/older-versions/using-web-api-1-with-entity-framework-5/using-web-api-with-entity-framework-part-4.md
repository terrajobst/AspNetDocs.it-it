---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: aggiunta di una visualizzazione amministratore | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556014"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="5c5a2-102">Parte 4: aggiunta di una visualizzazione amministratore</span><span class="sxs-lookup"><span data-stu-id="5c5a2-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="5c5a2-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5c5a2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5c5a2-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="5c5a2-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="5c5a2-105">Aggiungere una visualizzazione amministratore</span><span class="sxs-lookup"><span data-stu-id="5c5a2-105">Add an Admin View</span></span>

<span data-ttu-id="5c5a2-106">A questo punto, si passerà al lato client e si aggiungerà una pagina che può utilizzare i dati del controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="5c5a2-107">La pagina consente agli utenti di creare, modificare o eliminare prodotti inviando richieste AJAX al controller.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="5c5a2-108">In Esplora soluzioni espandere la cartella controller e aprire il file denominato HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="5c5a2-109">Questo file contiene un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-109">This file contains an MVC controller.</span></span> <span data-ttu-id="5c5a2-110">Aggiungere un metodo denominato `Admin`:</span><span class="sxs-lookup"><span data-stu-id="5c5a2-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="5c5a2-111">Il metodo **HttpRouteUrl** crea l'URI per l'API Web e lo archivia nell'elenco delle visualizzazioni per un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="5c5a2-112">Posizionare quindi il cursore del testo all'interno del metodo di azione `Admin`, quindi fare clic con il pulsante destro del mouse e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="5c5a2-113">Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .</span><span class="sxs-lookup"><span data-stu-id="5c5a2-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="5c5a2-114">Nella finestra di dialogo **Aggiungi visualizzazione** assegnare un nome alla vista "admin".</span><span class="sxs-lookup"><span data-stu-id="5c5a2-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="5c5a2-115">Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="5c5a2-116">In **classe modello**selezionare "Product (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="5c5a2-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="5c5a2-117">Lasciare invariati i valori predefiniti per tutte le altre opzioni.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="5c5a2-118">Fare clic su **Aggiungi per aggiungere** un file denominato admin. cshtml in views/Home.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="5c5a2-119">Aprire il file e aggiungere il codice HTML seguente.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="5c5a2-120">Questo codice HTML definisce la struttura della pagina, ma nessuna funzionalità è ancora collegata.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="5c5a2-121">Creare un collegamento alla pagina di amministrazione</span><span class="sxs-lookup"><span data-stu-id="5c5a2-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="5c5a2-122">In Esplora soluzioni espandere la cartella Views, quindi espandere la cartella Shared.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="5c5a2-123">Aprire il file denominato \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="5c5a2-124">Individuare l'elemento **ul** con ID = "menu" e un collegamento all'azione per la visualizzazione amministratore:</span><span class="sxs-lookup"><span data-stu-id="5c5a2-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="5c5a2-125">Nel progetto di esempio sono state apportate alcune altre modifiche estetiche, ad esempio la sostituzione della stringa "your logo here".</span><span class="sxs-lookup"><span data-stu-id="5c5a2-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="5c5a2-126">Questi non influiscono sulla funzionalità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="5c5a2-127">È possibile scaricare il progetto e confrontare i file.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="5c5a2-128">Eseguire l'applicazione e fare clic sul collegamento "admin" visualizzato nella parte superiore del home page.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="5c5a2-129">La pagina di amministrazione dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="5c5a2-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="5c5a2-130">A questo punto, la pagina non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="5c5a2-131">Nella sezione successiva verrà usato knockout. js per creare un'interfaccia utente dinamica.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="5c5a2-132">Aggiungi autorizzazione</span><span class="sxs-lookup"><span data-stu-id="5c5a2-132">Add Authorization</span></span>

<span data-ttu-id="5c5a2-133">La pagina di amministrazione è attualmente accessibile a chiunque visiti il sito.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="5c5a2-134">Modificare questa opzione per limitare l'autorizzazione agli amministratori.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="5c5a2-135">Per iniziare, aggiungere un ruolo "Administrator" e un utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="5c5a2-136">In Esplora soluzioni espandere la cartella filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="5c5a2-137">Individuare il costruttore `SimpleMembershipInitializer`.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="5c5a2-138">Dopo la chiamata a **WebSecurity. InitializeDatabaseConnection**, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5c5a2-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="5c5a2-139">Si tratta di un modo rapido e rapido per aggiungere il ruolo "Administrator" e creare un utente per il ruolo.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="5c5a2-140">In Esplora soluzioni espandere la cartella controller e aprire il file HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="5c5a2-141">Aggiungere l'attributo **autorizzi** al metodo `Admin`.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="5c5a2-142">Aprire il file AdminController.cs e aggiungere l'attributo **autorizza** all'intera classe `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="5c5a2-143">MVC e l'API Web definiscono entrambi gli attributi di **autorizzazione** , in spazi dei nomi diversi.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="5c5a2-144">MVC usa **System. Web. Mvc. AuthorizeAttribute**, mentre l'API Web USA **System. Web. http. AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="5c5a2-145">Ora solo gli amministratori possono visualizzare la pagina di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="5c5a2-146">Se inoltre si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="5c5a2-147">In caso contrario, il server invia una risposta HTTP 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="5c5a2-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="5c5a2-148">È possibile visualizzarlo in Fiddler inviando una richiesta GET a `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="5c5a2-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5c5a2-149">[Precedente](using-web-api-with-entity-framework-part-3.md)
> [Successivo](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="5c5a2-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
