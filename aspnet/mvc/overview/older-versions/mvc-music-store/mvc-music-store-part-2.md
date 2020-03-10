---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: controller | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 2 illustra i controller.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559878"
---
# <a name="part-2-controllers"></a><span data-ttu-id="b1485-104">Parte 2: controller</span><span class="sxs-lookup"><span data-stu-id="b1485-104">Part 2: Controllers</span></span>

<span data-ttu-id="b1485-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b1485-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b1485-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="b1485-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b1485-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="b1485-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b1485-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b1485-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b1485-109">La parte 2 illustra i controller.</span><span class="sxs-lookup"><span data-stu-id="b1485-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="b1485-110">Con i framework Web tradizionali, gli URL in ingresso vengono in genere mappati a file su disco.</span><span class="sxs-lookup"><span data-stu-id="b1485-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="b1485-111">Ad esempio, una richiesta per un URL come "/Products.aspx" o "/Products.php" potrebbe essere elaborata da un file "Products. aspx" o "Products. php".</span><span class="sxs-lookup"><span data-stu-id="b1485-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="b1485-112">I framework MVC basati sul Web mappano gli URL al codice server in modo leggermente diverso.</span><span class="sxs-lookup"><span data-stu-id="b1485-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="b1485-113">Anziché eseguire il mapping degli URL in ingresso ai file, eseguono invece il mapping degli URL ai metodi nelle classi.</span><span class="sxs-lookup"><span data-stu-id="b1485-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="b1485-114">Queste classi sono denominate "controller" e sono responsabili dell'elaborazione delle richieste HTTP in ingresso, della gestione dell'input dell'utente, del recupero e del salvataggio dei dati e della determinazione della risposta da inviare al client (visualizzazione HTML, download di un file, Reindirizzamento a un altro URL e così via).</span><span class="sxs-lookup"><span data-stu-id="b1485-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="b1485-115">Aggiunta di un HomeController</span><span class="sxs-lookup"><span data-stu-id="b1485-115">Adding a HomeController</span></span>

<span data-ttu-id="b1485-116">Si inizierà l'applicazione MVC Music Store aggiungendo una classe controller che gestirà gli URL alla Home page del sito.</span><span class="sxs-lookup"><span data-stu-id="b1485-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="b1485-117">Si seguiranno le convenzioni di denominazione predefinite di ASP.NET MVC e lo si chiamerà HomeController.</span><span class="sxs-lookup"><span data-stu-id="b1485-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="b1485-118">Fare clic con il pulsante destro del mouse sulla cartella "controller" all'interno del Esplora soluzioni e selezionare "Aggiungi", quindi il "controller". comando</span><span class="sxs-lookup"><span data-stu-id="b1485-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="b1485-119">Verrà visualizzata la finestra di dialogo "Aggiungi controller".</span><span class="sxs-lookup"><span data-stu-id="b1485-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="b1485-120">Assegnare al controller il nome "HomeController" e premere il pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="b1485-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="b1485-121">Verrà creato un nuovo file, HomeController.cs, con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b1485-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="b1485-122">Per iniziare nel modo più semplice possibile, sostituire il metodo index con un metodo semplice che restituisce solo una stringa.</span><span class="sxs-lookup"><span data-stu-id="b1485-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="b1485-123">Verranno apportate due modifiche:</span><span class="sxs-lookup"><span data-stu-id="b1485-123">We'll make two changes:</span></span>

- <span data-ttu-id="b1485-124">Modificare il metodo in modo che restituisca una stringa anziché un ActionResult</span><span class="sxs-lookup"><span data-stu-id="b1485-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="b1485-125">Modificare l'istruzione return per restituire "Hello from Home"</span><span class="sxs-lookup"><span data-stu-id="b1485-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="b1485-126">Il metodo dovrebbe ora essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b1485-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="b1485-127">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="b1485-127">Running the Application</span></span>

<span data-ttu-id="b1485-128">A questo punto, eseguire il sito.</span><span class="sxs-lookup"><span data-stu-id="b1485-128">Now let's run the site.</span></span> <span data-ttu-id="b1485-129">È possibile avviare il server Web e provare il sito usando uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="b1485-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="b1485-130">Scegliere la voce di menu debug ⇨ Avvia debug</span><span class="sxs-lookup"><span data-stu-id="b1485-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="b1485-131">Fare clic sul pulsante freccia verde sulla barra degli strumenti ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="b1485-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="b1485-132">Usare il tasto di scelta rapida F5.</span><span class="sxs-lookup"><span data-stu-id="b1485-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="b1485-133">L'uso di uno dei passaggi precedenti compilerà il progetto, quindi comporterà l'avvio del Server di sviluppo ASP.NET incorporato in Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="b1485-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="b1485-134">Verrà visualizzata una notifica nell'angolo inferiore dello schermo per indicare che l'Server di sviluppo ASP.NET è stata avviata e visualizzerà il numero di porta in cui è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b1485-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="b1485-135">In Visual Web Developer verrà aperta automaticamente una finestra del browser il cui URL punta al server Web.</span><span class="sxs-lookup"><span data-stu-id="b1485-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="b1485-136">Questo consentirà di provare rapidamente l'applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="b1485-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="b1485-137">Ok, era piuttosto rapido: abbiamo creato un nuovo sito Web, abbiamo aggiunto una funzione a tre righe e abbiamo ottenuto testo in un browser.</span><span class="sxs-lookup"><span data-stu-id="b1485-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="b1485-138">Non è una scienza missilistica, ma si tratta di un inizio.</span><span class="sxs-lookup"><span data-stu-id="b1485-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="b1485-139">*Nota: Visual Web Developer include il Server di sviluppo ASP.NET, che eseguirà il sito Web in un numero "porta" libero casuale. Nella schermata precedente il sito è in esecuzione in `http://localhost:26641/`, quindi usa la porta 26641. Il numero di porta sarà diverso. Quando si parla di URL come/Store/Browse in questa esercitazione, questa operazione verrà completata dopo il numero di porta. Supponendo un numero di porta 26641, l'esplorazione di/Store/Browse significherà l'esplorazione `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="b1485-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="b1485-140">Aggiunta di un StoreController</span><span class="sxs-lookup"><span data-stu-id="b1485-140">Adding a StoreController</span></span>

<span data-ttu-id="b1485-141">È stato aggiunto un semplice HomeController che implementa la Home page del sito.</span><span class="sxs-lookup"><span data-stu-id="b1485-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="b1485-142">A questo punto, aggiungere un altro controller che verrà usato per implementare la funzionalità di esplorazione di Music Store.</span><span class="sxs-lookup"><span data-stu-id="b1485-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="b1485-143">Il controller dello Store supporterà tre scenari:</span><span class="sxs-lookup"><span data-stu-id="b1485-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="b1485-144">Pagina di presentazione dei generi musicali nel nostro archivio musica</span><span class="sxs-lookup"><span data-stu-id="b1485-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="b1485-145">Pagina Sfoglia che elenca tutti gli album musicali di un determinato genere</span><span class="sxs-lookup"><span data-stu-id="b1485-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="b1485-146">Pagina dei dettagli che mostra le informazioni su uno specifico album musicale</span><span class="sxs-lookup"><span data-stu-id="b1485-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="b1485-147">Si inizierà aggiungendo una nuova classe StoreController.</span><span class="sxs-lookup"><span data-stu-id="b1485-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="b1485-148">Se non è già stato fatto, arrestare l'esecuzione dell'applicazione chiudendo il browser o selezionando la voce di menu debug ⇨ Interrompi debug.</span><span class="sxs-lookup"><span data-stu-id="b1485-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="b1485-149">Aggiungere ora un nuovo StoreController.</span><span class="sxs-lookup"><span data-stu-id="b1485-149">Now add a new StoreController.</span></span> <span data-ttu-id="b1485-150">Analogamente a quanto avviene con HomeController, questa operazione viene eseguita facendo clic con il pulsante destro del mouse sulla cartella "Controllers" all'interno del Esplora soluzioni e scegliendo la voce di menu Aggiungi&gt;controller</span><span class="sxs-lookup"><span data-stu-id="b1485-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="b1485-151">Il nuovo StoreController dispone già di un metodo "index".</span><span class="sxs-lookup"><span data-stu-id="b1485-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="b1485-152">Questo metodo "index" verrà usato per implementare la pagina di presentazione in cui sono elencati tutti i generi presenti nel Music Store.</span><span class="sxs-lookup"><span data-stu-id="b1485-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="b1485-153">Verranno anche aggiunti altri due metodi per implementare gli altri due scenari che si vuole gestire con il StoreController: browse and details.</span><span class="sxs-lookup"><span data-stu-id="b1485-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="b1485-154">Questi metodi (index, browse e Details) all'interno del controller sono denominati "azioni del controller" e, come già visto con il metodo di azione HomeController. index (), il loro lavoro consiste nel rispondere alle richieste URL e, in generale, determinare il contenuto deve essere restituito al browser o all'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="b1485-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="b1485-155">Verrà avviata l'implementazione di StoreController modificando il metodo theIndex () per restituire la stringa "Hello from Store. index ()" e si aggiungeranno metodi simili per Browse () e Details ():</span><span class="sxs-lookup"><span data-stu-id="b1485-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="b1485-156">Eseguire nuovamente il progetto ed esplorare gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1485-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="b1485-157">/Store</span><span class="sxs-lookup"><span data-stu-id="b1485-157">/Store</span></span>
- <span data-ttu-id="b1485-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="b1485-158">/Store/Browse</span></span>
- <span data-ttu-id="b1485-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="b1485-159">/Store/Details</span></span>

<span data-ttu-id="b1485-160">L'accesso a questi URL richiama i metodi di azione all'interno del controller e restituiscono risposte stringa:</span><span class="sxs-lookup"><span data-stu-id="b1485-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="b1485-161">Questo è un ottimo, ma si tratta solo di stringhe costanti.</span><span class="sxs-lookup"><span data-stu-id="b1485-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="b1485-162">È quindi opportuno renderli dinamici, in modo da ottenere informazioni dall'URL e visualizzarle nell'output della pagina.</span><span class="sxs-lookup"><span data-stu-id="b1485-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="b1485-163">Prima di tutto verrà modificato il metodo di azione browse per recuperare un valore QueryString dall'URL.</span><span class="sxs-lookup"><span data-stu-id="b1485-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="b1485-164">È possibile eseguire questa operazione aggiungendo un parametro "genre" al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="b1485-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="b1485-165">Quando si esegue questa operazione, ASP.NET MVC passa automaticamente i parametri QueryString o post del form denominati "genre" al metodo di azione quando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="b1485-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="b1485-166">*Nota: il metodo di utilità HttpUtility. HtmlEncode viene usato per purificare l'input dell'utente. In questo modo si impedisce agli utenti di inserire JavaScript nella visualizzazione con un collegamento come/Store/Browse? Genre =&lt;script&gt;finestra. location ='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="b1485-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="b1485-167">Passiamo ora a/Store/Browse? Genere = discoteca</span><span class="sxs-lookup"><span data-stu-id="b1485-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="b1485-168">Modificare quindi l'azione dettagli per leggere e visualizzare un parametro di input denominato ID.</span><span class="sxs-lookup"><span data-stu-id="b1485-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="b1485-169">A differenza del metodo precedente, il valore ID non verrà incorporato come parametro QueryString.</span><span class="sxs-lookup"><span data-stu-id="b1485-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="b1485-170">Verrà invece incorporata direttamente all'interno dell'URL.</span><span class="sxs-lookup"><span data-stu-id="b1485-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="b1485-171">Ad esempio:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="b1485-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="b1485-172">ASP.NET MVC consente di eseguire facilmente questa operazione senza dover configurare alcun elemento.</span><span class="sxs-lookup"><span data-stu-id="b1485-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="b1485-173">La convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come parametro denominato "ID".</span><span class="sxs-lookup"><span data-stu-id="b1485-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="b1485-174">Se il metodo di azione include un parametro denominato ID, ASP.NET MVC passa automaticamente il segmento dell'URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="b1485-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="b1485-175">Eseguire l'applicazione e passare a/Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="b1485-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="b1485-176">Ricapitoliamo ora quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b1485-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="b1485-177">È stato creato un nuovo progetto MVC ASP.NET in Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="b1485-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="b1485-178">È stata illustrata la struttura di cartelle di base di un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1485-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="b1485-179">Abbiamo appreso come eseguire il nostro sito Web usando il Server di sviluppo ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1485-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="b1485-180">Sono state create due classi controller: HomeController e StoreController</span><span class="sxs-lookup"><span data-stu-id="b1485-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="b1485-181">Sono stati aggiunti metodi di azione ai controller che rispondono alle richieste URL e restituiscono testo al browser</span><span class="sxs-lookup"><span data-stu-id="b1485-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b1485-182">[Precedente](mvc-music-store-part-1.md)
> [Successivo](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="b1485-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
