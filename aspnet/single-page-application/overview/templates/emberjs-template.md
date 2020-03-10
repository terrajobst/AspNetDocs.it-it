---
uid: single-page-application/overview/templates/emberjs-template
title: Modello EmberJS | Microsoft Docs
author: xqiu
description: Modello EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578505"
---
# <a name="emberjs-template"></a><span data-ttu-id="043ea-103">Modello EmberJS</span><span class="sxs-lookup"><span data-stu-id="043ea-103">EmberJS template</span></span>

<span data-ttu-id="043ea-104">di [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="043ea-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="043ea-105">Il modello MVC EmberJS è scritto da Nathan Lucchesi, Thiago Santos e Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="043ea-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="043ea-106">Scaricare il modello MVC EmberJS</span><span class="sxs-lookup"><span data-stu-id="043ea-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="043ea-107">Il modello EmberJS SPA è progettato per iniziare rapidamente a creare app Web interattive sul lato client usando EmberJS.</span><span class="sxs-lookup"><span data-stu-id="043ea-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="043ea-108">"Applicazione a pagina singola" (SPA) è il termine generale per un'applicazione Web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, anziché caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="043ea-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="043ea-109">Al termine del caricamento iniziale della pagina, la SPA comunica con il server tramite richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="043ea-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="043ea-110">AJAX non è una novità, ma oggi sono disponibili Framework JavaScript che semplificano la creazione e la gestione di un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="043ea-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="043ea-111">Inoltre, HTML 5 e CSS3 semplificano la creazione di interfacce utente avanzate.</span><span class="sxs-lookup"><span data-stu-id="043ea-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="043ea-112">Il modello di EmberJS SPA usa la libreria [per la gestione di pagine JavaScript per](http://emberjs.com/) gestire gli aggiornamenti delle pagine dalle richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="043ea-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="043ea-113">Brac. js USA data binding per sincronizzare la pagina con i dati più recenti.</span><span class="sxs-lookup"><span data-stu-id="043ea-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="043ea-114">In questo modo, non è necessario scrivere codice che analizza i dati JSON e aggiorna il DOM.</span><span class="sxs-lookup"><span data-stu-id="043ea-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="043ea-115">Al contrario, si inseriscono gli attributi dichiarativi nel codice HTML che indicano a Brac. js come presentare i dati.</span><span class="sxs-lookup"><span data-stu-id="043ea-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="043ea-116">Sul lato server, il modello EmberJS è quasi identico al [modello di Knockout Spa](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="043ea-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="043ea-117">USA ASP.NET MVC per fornire documenti HTML e API Web ASP.NET per gestire le richieste AJAX dal client.</span><span class="sxs-lookup"><span data-stu-id="043ea-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="043ea-118">Per ulteriori informazioni su questi aspetti del modello, vedere la documentazione del [modello knockout](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="043ea-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="043ea-119">Questo argomento è incentrato sulle differenze tra il modello knockout e il modello EmberJS.</span><span class="sxs-lookup"><span data-stu-id="043ea-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="043ea-120">Creare un progetto di modello EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="043ea-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="043ea-121">Scaricare e installare il modello facendo clic sul pulsante di download riportato sopra.</span><span class="sxs-lookup"><span data-stu-id="043ea-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="043ea-122">Potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="043ea-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="043ea-123">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="043ea-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="043ea-124">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="043ea-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="043ea-125">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="043ea-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="043ea-126">Assegnare un nome al progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="043ea-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="043ea-127">Nella creazione guidata **nuovo progetto** , selezionare **progetto Spa Brac. js**.</span><span class="sxs-lookup"><span data-stu-id="043ea-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="043ea-128">Panoramica del modello di EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="043ea-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="043ea-129">Il modello EmberJS usa una combinazione di jQuery, Brac. js, manopole. js per creare un'interfaccia utente interattiva e uniforme.</span><span class="sxs-lookup"><span data-stu-id="043ea-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="043ea-130">Brac. js è una libreria JavaScript che usa uno schema MVC sul lato client.</span><span class="sxs-lookup"><span data-stu-id="043ea-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="043ea-131">Un *modello*, scritto nel linguaggio del modello manubrio, descrive l'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="043ea-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="043ea-132">In modalità di rilascio, il [compilatore manubri](https://github.com/Myslik/csharp-ember-handlebars) viene usato per aggregare e compilare il modello manubrio.</span><span class="sxs-lookup"><span data-stu-id="043ea-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="043ea-133">Un *modello* archivia i dati dell'applicazione ottenuti dal server (elenchi todo e elementi todo).</span><span class="sxs-lookup"><span data-stu-id="043ea-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="043ea-134">Un *controller* archivia lo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="043ea-134">A *controller* stores application state.</span></span> <span data-ttu-id="043ea-135">I controller spesso presentano dati del modello ai modelli corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="043ea-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="043ea-136">Una *visualizzazione* converte gli eventi primitivi dall'applicazione e li passa al controller.</span><span class="sxs-lookup"><span data-stu-id="043ea-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="043ea-137">Un *router* gestisce lo stato dell'applicazione, mantenendo sincronizzati gli URL e i modelli.</span><span class="sxs-lookup"><span data-stu-id="043ea-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="043ea-138">Inoltre, la libreria di dati di Brac può essere usata per sincronizzare gli oggetti JSON (ottenuti dal server tramite un'API RESTful) e i modelli client.</span><span class="sxs-lookup"><span data-stu-id="043ea-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="043ea-139">Il modello EmberJS SPA organizza gli script in otto livelli:</span><span class="sxs-lookup"><span data-stu-id="043ea-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="043ea-140">WebAPI\_adapter. js, WebAPI\_serializer. js: estende la libreria di dati Brac per lavorare con API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="043ea-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="043ea-141">Scripts/helper. js: definisce nuovi Helper del manubrio Brac.</span><span class="sxs-lookup"><span data-stu-id="043ea-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="043ea-142">Scripts/app. js: crea l'app e configura l'adapter e il serializzatore.</span><span class="sxs-lookup"><span data-stu-id="043ea-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="043ea-143">Scripts/app/models/\*. js: definisce i modelli.</span><span class="sxs-lookup"><span data-stu-id="043ea-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="043ea-144">Scripts/app/views/\*. js: definisce le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="043ea-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="043ea-145">Scripts/app/controllers/\*. js: definisce i controller.</span><span class="sxs-lookup"><span data-stu-id="043ea-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="043ea-146">Scripts/app/routes, scripts/app/router. js: definisce le route.</span><span class="sxs-lookup"><span data-stu-id="043ea-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="043ea-147">Templates/\*. HBS: definisce i modelli dei manubri.</span><span class="sxs-lookup"><span data-stu-id="043ea-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="043ea-148">Esaminiamo alcuni di questi script in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="043ea-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="043ea-149">Modelli</span><span class="sxs-lookup"><span data-stu-id="043ea-149">Models</span></span>

<span data-ttu-id="043ea-150">I modelli sono definiti nella cartella Scripts/app/models.</span><span class="sxs-lookup"><span data-stu-id="043ea-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="043ea-151">Sono disponibili due file di modello: todoItem. js e todo. js.</span><span class="sxs-lookup"><span data-stu-id="043ea-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="043ea-152">**todo. Model. js** definisce i modelli lato client (browser) per gli elenchi di attività.</span><span class="sxs-lookup"><span data-stu-id="043ea-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="043ea-153">Sono disponibili due classi di modello: todoItem e todo.</span><span class="sxs-lookup"><span data-stu-id="043ea-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="043ea-154">In Brac, i modelli sono sottoclassi di servizi di dominio Active Directory. Modello.</span><span class="sxs-lookup"><span data-stu-id="043ea-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="043ea-155">Un modello può avere proprietà con attributi:</span><span class="sxs-lookup"><span data-stu-id="043ea-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="043ea-156">I modelli possono definire relazioni con altri modelli:</span><span class="sxs-lookup"><span data-stu-id="043ea-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="043ea-157">I modelli possono avere proprietà calcolate associate ad altre proprietà:</span><span class="sxs-lookup"><span data-stu-id="043ea-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="043ea-158">I modelli possono avere funzioni Observer, che vengono richiamate quando una proprietà osservata cambia:</span><span class="sxs-lookup"><span data-stu-id="043ea-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="043ea-159">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="043ea-159">Views</span></span>

<span data-ttu-id="043ea-160">Le visualizzazioni sono definite nella cartella Scripts/app/views.</span><span class="sxs-lookup"><span data-stu-id="043ea-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="043ea-161">Una vista converte gli eventi dall'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="043ea-161">A view translates events from the application UI.</span></span> <span data-ttu-id="043ea-162">Un gestore eventi può richiamare le funzioni del controller o semplicemente chiamare direttamente il contesto dati.</span><span class="sxs-lookup"><span data-stu-id="043ea-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="043ea-163">Il codice seguente, ad esempio, è da views/TodoItemEditView. js.</span><span class="sxs-lookup"><span data-stu-id="043ea-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="043ea-164">Definisce la gestione degli eventi per un campo di testo di input.</span><span class="sxs-lookup"><span data-stu-id="043ea-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="043ea-165">Controller</span><span class="sxs-lookup"><span data-stu-id="043ea-165">Controller</span></span>

<span data-ttu-id="043ea-166">I controller sono definiti nella cartella Scripts/app/controllers.</span><span class="sxs-lookup"><span data-stu-id="043ea-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="043ea-167">Per rappresentare un singolo modello, estendere `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="043ea-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="043ea-168">Un controller può anche rappresentare una raccolta di modelli estendendo `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="043ea-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="043ea-169">Ad esempio, TodoListController rappresenta una matrice di oggetti `todoList`.</span><span class="sxs-lookup"><span data-stu-id="043ea-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="043ea-170">Il controller Ordina in base all'ID dell'oggetto todo, in ordine decrescente:</span><span class="sxs-lookup"><span data-stu-id="043ea-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="043ea-171">Il controller definisce una funzione denominata `addTodoList`, che crea un nuovo elenco attività e lo aggiunge alla matrice.</span><span class="sxs-lookup"><span data-stu-id="043ea-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="043ea-172">Per vedere come viene chiamata questa funzione, aprire il file modello denominato todoListTemplate. html nella cartella Templates.</span><span class="sxs-lookup"><span data-stu-id="043ea-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="043ea-173">Il codice di modello seguente associa un pulsante alla funzione `addTodoList`:</span><span class="sxs-lookup"><span data-stu-id="043ea-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="043ea-174">Il controller contiene anche una proprietà `error`, che contiene un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="043ea-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="043ea-175">Ecco il codice del modello per visualizzare il messaggio di errore (anche in todoListTemplate. html):</span><span class="sxs-lookup"><span data-stu-id="043ea-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="043ea-176">Route</span><span class="sxs-lookup"><span data-stu-id="043ea-176">Routes</span></span>

<span data-ttu-id="043ea-177">Router. js definisce le route e il modello predefinito da visualizzare, imposta lo stato dell'applicazione e corrisponde agli URL delle route:</span><span class="sxs-lookup"><span data-stu-id="043ea-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="043ea-178">TodoListRoute. js carica i dati per il TodoListRoute eseguendo l'override della funzione setupController:</span><span class="sxs-lookup"><span data-stu-id="043ea-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="043ea-179">Brac usa le convenzioni di denominazione per associare URL, nomi di route, controller e modelli.</span><span class="sxs-lookup"><span data-stu-id="043ea-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="043ea-180">Per ulteriori informazioni, vedere [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) nella documentazione di EmberJS.</span><span class="sxs-lookup"><span data-stu-id="043ea-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="043ea-181">Modelli</span><span class="sxs-lookup"><span data-stu-id="043ea-181">Templates</span></span>

<span data-ttu-id="043ea-182">La cartella Templates contiene quattro modelli:</span><span class="sxs-lookup"><span data-stu-id="043ea-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="043ea-183">Application. HBS: modello predefinito di cui viene eseguito il rendering all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="043ea-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="043ea-184">about. HBS: modello per la route "/about".</span><span class="sxs-lookup"><span data-stu-id="043ea-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="043ea-185">index. HBS: modello per la route radice "/".</span><span class="sxs-lookup"><span data-stu-id="043ea-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="043ea-186">todo. HBS: modello per la route "/TODO".</span><span class="sxs-lookup"><span data-stu-id="043ea-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="043ea-187">\_barra di spostamento. HBS: il modello definisce il menu di navigazione.</span><span class="sxs-lookup"><span data-stu-id="043ea-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="043ea-188">Il modello di applicazione funge da pagina master.</span><span class="sxs-lookup"><span data-stu-id="043ea-188">The application template acts like a master page.</span></span> <span data-ttu-id="043ea-189">Contiene un'intestazione, un piè di pagina e un "{{Outlet}}" per inserire altri modelli in a seconda della route.</span><span class="sxs-lookup"><span data-stu-id="043ea-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="043ea-190">Per ulteriori informazioni sui modelli di applicazione in Brac, vedere [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="043ea-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="043ea-191">Il modello "/todoList" contiene due espressioni di ciclo.</span><span class="sxs-lookup"><span data-stu-id="043ea-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="043ea-192">Il ciclo esterno è `{{#each controller}}`e il ciclo interno è `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="043ea-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="043ea-193">Il codice seguente illustra una visualizzazione `Ember.Checkbox` incorporata, una `App.TodoItemEditView`personalizzata e un collegamento con un'azione di `deleteTodo`.</span><span class="sxs-lookup"><span data-stu-id="043ea-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="043ea-194">La classe `HtmlHelperExtensions`, definita in Controllers/HtmlHelperExtensions. cs, definisce una funzione helper per memorizzare nella cache e inserire i file di modello quando il **debug** è impostato su **true** nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="043ea-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="043ea-195">Questa funzione viene chiamata dal file di visualizzazione MVC ASP.NET definito in views/Home/app. cshtml:</span><span class="sxs-lookup"><span data-stu-id="043ea-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="043ea-196">Chiamato senza argomenti, la funzione esegue il rendering di tutti i file di modello nella cartella Templates.</span><span class="sxs-lookup"><span data-stu-id="043ea-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="043ea-197">È anche possibile specificare una sottocartella o un file di modello specifico.</span><span class="sxs-lookup"><span data-stu-id="043ea-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="043ea-198">Quando il **debug** è **false** in Web. config, l'applicazione include l'elemento del bundle "~/Bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="043ea-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="043ea-199">Questo elemento del bundle viene aggiunto in BundleConfig.cs, usando la libreria del compilatore dei manubri:</span><span class="sxs-lookup"><span data-stu-id="043ea-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
