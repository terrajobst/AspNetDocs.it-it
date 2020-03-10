---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: visualizzazioni e ViewModel | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 3 vengono illustrate le viste e i ViewModel.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559808"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="9dc9d-104">Parte 3: visualizzazioni e ViewModel</span><span class="sxs-lookup"><span data-stu-id="9dc9d-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="9dc9d-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9dc9d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9dc9d-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9dc9d-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9dc9d-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9dc9d-109">Nella parte 3 vengono illustrate le viste e i ViewModel.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="9dc9d-110">Finora abbiamo appena restituito stringhe dalle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="9dc9d-111">Si tratta di un modo interessante per ottenere un'idea del funzionamento dei controller, ma non è il modo in cui si vuole creare un'applicazione Web reale.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="9dc9d-112">Si vuole ottenere un modo migliore per generare il codice HTML nei browser che visitano il sito, uno in cui è possibile usare i file di modello per personalizzare più facilmente il ritorno del contenuto HTML.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="9dc9d-113">Questo è esattamente ciò che accade per le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="9dc9d-114">Aggiunta di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="9dc9d-114">Adding a View template</span></span>

<span data-ttu-id="9dc9d-115">Per usare un modello di visualizzazione, modificare il metodo di indice HomeController per restituire un ActionResult e fare in modo che restituisca View (), come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="9dc9d-116">La modifica sopra indicata indica che invece di restituire una stringa, si vuole usare "View" per generare un risultato.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="9dc9d-117">A questo punto verrà aggiunto un modello di visualizzazione appropriato al progetto.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="9dc9d-118">A tale scopo, si posiziona il cursore del testo all'interno del metodo di azione dell'indice, quindi si fa clic con il pulsante destro del mouse e si seleziona "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="9dc9d-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="9dc9d-119">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="9dc9d-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9dc9d-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="9dc9d-121">La finestra di dialogo "Aggiungi visualizzazione" consente di generare in modo rapido e semplice i file del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="9dc9d-122">Per impostazione predefinita, nella finestra di dialogo "Aggiungi visualizzazione" viene prepopolato il nome del modello di vista da creare, in modo che corrisponda al metodo di azione che lo utilizzerà.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="9dc9d-123">Poiché è stato usato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione index () del HomeController, la finestra di dialogo "Aggiungi visualizzazione" precedente ha "index" come nome della visualizzazione pre-popolata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="9dc9d-124">Non è necessario modificare alcuna opzione in questa finestra di dialogo, quindi fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="9dc9d-125">Quando si fa clic sul pulsante Aggiungi, in Visual Web Developer viene creato un nuovo modello di vista index. cshtml nella directory \Views\Home, creando la cartella se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="9dc9d-126">Il nome e il percorso della cartella del file "index. cshtml" sono importanti e seguono le convenzioni di denominazione ASP.NET MVC predefinite.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="9dc9d-127">Il nome della directory, \Views\Home, corrisponde al controller, denominato HomeController.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="9dc9d-128">Il nome del modello di visualizzazione, index, corrisponde al metodo di azione del controller che visualizzerà la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="9dc9d-129">ASP.NET MVC consente di evitare di dover specificare in modo esplicito il nome o il percorso di un modello di vista quando si usa questa convenzione di denominazione per restituire una vista.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="9dc9d-130">Per impostazione predefinita, viene eseguito il rendering del modello di visualizzazione \Views\Home\Index.cshtml quando si scrive codice come riportato di seguito nel HomeController:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="9dc9d-131">Visual Web Developer ha creato e aperto il modello di visualizzazione "index. cshtml" dopo aver fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="9dc9d-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="9dc9d-132">Di seguito è riportato il contenuto di index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="9dc9d-133">Questa visualizzazione usa il sintassi Razor, che è più conciso del motore di visualizzazione Web Form usato nei Web Form ASP.NET e nelle versioni precedenti di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="9dc9d-134">Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori scoprono che il motore di visualizzazione Razor si adatta perfettamente allo sviluppo di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="9dc9d-135">Le prime tre righe impostano il titolo della pagina usando ViewBag. title.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="9dc9d-136">Si esaminerà il funzionamento più dettagliatamente, ma prima si aggiornerà il testo dell'intestazione del testo e si visualizza la pagina.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="9dc9d-137">Aggiornare il tag &lt;H2&gt; per indicare "Questa è la Home page", come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="9dc9d-138">L'esecuzione dell'applicazione mostra che il nuovo testo è visibile nel home page.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="9dc9d-139">Uso di un layout per gli elementi del sito comuni</span><span class="sxs-lookup"><span data-stu-id="9dc9d-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="9dc9d-140">La maggior parte dei siti Web include contenuto condiviso tra numerose pagine: navigazione, piè di pagina, immagini logo, riferimenti al foglio di stile e così via. Il motore di visualizzazione Razor semplifica la gestione usando una pagina denominata \_layout. cshtml, che è stata creata automaticamente nella cartella/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="9dc9d-141">Fare doppio clic su questa cartella per visualizzare il contenuto, illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="9dc9d-142">Il contenuto delle singole visualizzazioni verrà visualizzato tramite il comando @RenderBody() ed eventuali contenuti comuni che si desidera visualizzare all'esterno di che possono essere aggiunti al markup \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="9dc9d-143">Si vuole che l'archivio musicale MVC abbia un'intestazione comune con i collegamenti alla Home page e all'area di archiviazione in tutte le pagine del sito, quindi verrà aggiunto al modello direttamente sopra l'istruzione @RenderBody().</span><span class="sxs-lookup"><span data-stu-id="9dc9d-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="9dc9d-144">Aggiornamento del foglio di stile</span><span class="sxs-lookup"><span data-stu-id="9dc9d-144">Updating the StyleSheet</span></span>

<span data-ttu-id="9dc9d-145">Il modello di progetto vuoto include un file CSS molto semplificato che include solo gli stili utilizzati per visualizzare i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="9dc9d-146">La finestra di progettazione ha fornito alcuni CSS e immagini aggiuntive per definire l'aspetto del sito, quindi verranno aggiunti al momento.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="9dc9d-147">Il file CSS aggiornato e le immagini sono inclusi nella directory content di MvcMusicStore-Assets. zip, disponibile in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="9dc9d-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="9dc9d-148">Verranno selezionati entrambi in Esplora risorse e rilasciati nella cartella del contenuto della soluzione in Visual Web Developer, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="9dc9d-149">Verrà richiesto di confermare se si desidera sovrascrivere il file site. CSS esistente.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="9dc9d-150">Scegliere Sì.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="9dc9d-151">La cartella contenuto dell'applicazione verrà ora visualizzata come segue:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="9dc9d-152">A questo punto, eseguire l'applicazione e verificare l'aspetto delle modifiche nella Home page.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="9dc9d-153">Esaminiamo le modifiche apportate: il metodo di azione per l'indice di HomeController è stato trovato e visualizzato il modello \Views\Home\Index.cshtmlView, anche se il codice ha chiamato "Return View ()", perché il modello di visualizzazione ha seguito la convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="9dc9d-154">Nella Home page viene visualizzato un semplice messaggio di benvenuto definito nel modello di visualizzazione \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="9dc9d-155">La Home Page usa il modello \_layout. cshtml, quindi il messaggio di benvenuto è contenuto nel layout HTML del sito standard.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="9dc9d-156">Uso di un modello per passare informazioni alla visualizzazione</span><span class="sxs-lookup"><span data-stu-id="9dc9d-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="9dc9d-157">Un modello di vista che visualizza solo il codice HTML hardcoded non sta per creare un sito web molto interessante.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="9dc9d-158">Per creare un sito Web dinamico, è invece opportuno passare le informazioni dalle azioni del controller ai modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="9dc9d-159">Nel modello Model-View-Controller il termine modello fa riferimento a oggetti che rappresentano i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="9dc9d-160">Spesso gli oggetti modello corrispondono alle tabelle del database, ma non è necessario.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="9dc9d-161">I metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto modello alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="9dc9d-162">Questo consente a un controller di assemblare correttamente tutte le informazioni necessarie per generare una risposta e quindi di passare le informazioni a un modello di vista da usare per generare la risposta HTML appropriata.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="9dc9d-163">Questa operazione è più semplice da comprendere visualizzandola, quindi è possibile iniziare.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="9dc9d-164">Verranno innanzitutto create alcune classi di modelli per rappresentare i generi e gli album all'interno del negozio.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="9dc9d-165">Iniziamo creando una classe Genre.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="9dc9d-166">Fare clic con il pulsante destro del mouse sulla cartella "modelli" nel progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="9dc9d-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="9dc9d-167">Aggiungere quindi una proprietà nome stringa pubblica alla classe che è stata creata:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="9dc9d-168">*Nota: se ci si chiede, la notazione {get; set;} usa C#la funzionalità proprietà implementate automaticamente. Questo consente di ottenere i vantaggi di una proprietà senza richiedere la dichiarazione di un campo sottostante.*</span><span class="sxs-lookup"><span data-stu-id="9dc9d-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="9dc9d-169">Seguire quindi gli stessi passaggi per creare una classe album (denominata Album.cs) con un titolo e una proprietà Genre:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="9dc9d-170">A questo punto è possibile modificare il StoreController per usare le visualizzazioni che visualizzano le informazioni dinamiche del modello.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="9dc9d-171">Se-a scopo dimostrativo, abbiamo denominato gli album in base all'ID richiesta, possiamo visualizzare tali informazioni come nella visualizzazione seguente.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="9dc9d-172">Si inizierà modificando l'azione Archivia dettagli in modo da visualizzare le informazioni per un singolo album.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="9dc9d-173">Aggiungere un'istruzione "using" all'inizio della classe **StoreControllers** per includere lo spazio dei nomi MvcMusicStore. Models, quindi non è necessario digitare MvcMusicStore. Models. album ogni volta che si vuole usare la classe album.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="9dc9d-174">La sezione "using" di tale classe dovrebbe ora apparire come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="9dc9d-175">Successivamente, verrà aggiornata l'azione del controller Details in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice di HomeController.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="9dc9d-176">A questo punto è possibile modificare la logica per restituire un oggetto album alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="9dc9d-177">Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per il momento verrà usato "dati fittizi" per iniziare.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="9dc9d-178">*Nota: se non si ha familiarità C#con, è possibile presupporre che l'uso di var significhi che la variabile dell'album è ad associazione tardiva. Questa operazione non è corretta. C# il compilatore usa l'inferenza dei tipi in base a quanto assegnato alla variabile per determinare che l'album è di tipo album e la compilazione della variabile album locale come tipo di album, quindi si ottengono il controllo in fase di compilazione e il supporto di Visual Studio Code-Editor.*</span><span class="sxs-lookup"><span data-stu-id="9dc9d-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="9dc9d-179">A questo punto è possibile creare un modello di visualizzazione che usa l'album per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="9dc9d-180">Prima di eseguire questa operazione, è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione conosca la nuova classe album creata.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="9dc9d-181">È possibile compilare il progetto selezionando la voce di menu debug ⇨ build MvcMusicStore (per un credito aggiuntivo, è possibile usare il tasto di scelta rapida CTRL + MAIUSC + B per compilare il progetto).</span><span class="sxs-lookup"><span data-stu-id="9dc9d-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="9dc9d-182">Ora che sono state configurate le classi di supporto, è possibile creare il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="9dc9d-183">Fare clic con il pulsante destro del mouse all'interno del metodo Details e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="9dc9d-184">Verrà creato un nuovo modello di vista, come già fatto in precedenza con HomeController.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="9dc9d-185">Poiché viene creato dal StoreController, verrà generato per impostazione predefinita in un file \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="9dc9d-186">Diversamente da prima, verrà selezionata la casella di controllo "crea una visualizzazione fortemente tipizzata".</span><span class="sxs-lookup"><span data-stu-id="9dc9d-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="9dc9d-187">Verrà quindi selezionata la classe "album" nell'elenco a discesa "View Data-Class".</span><span class="sxs-lookup"><span data-stu-id="9dc9d-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="9dc9d-188">Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione" per creare un modello di visualizzazione che prevede che un oggetto album venga passato a tale modello per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="9dc9d-189">Quando si fa clic sul pulsante "Aggiungi", verrà creato il modello di visualizzazione \Views\Store\Details.cshtml contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="9dc9d-190">Si noti la prima riga, che indica che questa visualizzazione è fortemente tipizzata per la classe album.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="9dc9d-191">Il motore di visualizzazione Razor comprende che è stato passato un oggetto album, quindi è possibile accedere facilmente alle proprietà del modello e anche sfruttare i vantaggi di IntelliSense nell'editor di Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="9dc9d-192">Aggiornare il tag &lt;H2&gt; in modo che visualizzi la proprietà title dell'album modificando la riga in modo che appaia come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="9dc9d-193">Si noti che IntelliSense viene attivato quando si immette il punto dopo la parola chiave @Model, mostrando le proprietà e i metodi supportati dalla classe album.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="9dc9d-194">A questo punto, eseguire di nuovo il progetto e visitare l'URL/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="9dc9d-195">Verranno visualizzati i dettagli di un album come riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="9dc9d-196">Verrà ora creato un aggiornamento simile per il metodo di azione browse Store.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="9dc9d-197">Aggiornare il metodo in modo che restituisca un ActionResult e modifichi la logica del metodo in modo che crei un nuovo oggetto Genre e lo restituisca alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="9dc9d-198">Fare clic con il pulsante destro del mouse sul metodo browse e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida, aggiungere una visualizzazione fortemente tipizzata aggiungere un oggetto fortemente tipizzato alla classe Genre.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="9dc9d-199">Aggiornare l'elemento &lt;H2&gt; nel codice di visualizzazione (in/Views/Store/Browse.cshtml) per visualizzare le informazioni sul genere.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="9dc9d-200">A questo punto è possibile eseguire nuovamente il progetto e passare a/Store/Browse? Genere = URL disco.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="9dc9d-201">La pagina Browse verrà visualizzata come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="9dc9d-202">Infine, è possibile eseguire un aggiornamento leggermente più complesso al metodo e alla visualizzazione di azione dell' **indice di archiviazione** per visualizzare un elenco di tutti i generi nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="9dc9d-203">Questa operazione verrà eseguita usando un elenco di generi come oggetto modello, anziché un singolo genere.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="9dc9d-204">Fare clic con il pulsante destro del mouse sul metodo di azione Archivia indice e selezionare Aggiungi visualizzazione come in precedenza, selezionare genere come classe modello e premere il pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="9dc9d-205">Prima di tutto si modificherà la dichiarazione di @model per indicare che la visualizzazione prevede diversi oggetti di genere anziché uno solo.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="9dc9d-206">Modificare la prima riga di/Store/Index.cshtml in Read come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="9dc9d-207">Indica al motore di visualizzazione Razor che funzionerà con un oggetto modello che può contenere diversi oggetti Genre.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="9dc9d-208">Si sta usando IEnumerable&lt;genre&gt; anziché un elenco&lt;genere&gt; poiché è più generico, consentendo di modificare il tipo di modello in un secondo momento con qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="9dc9d-209">Successivamente, verrà eseguito il ciclo degli oggetti Genre nel modello, come illustrato nel codice di visualizzazione completato riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="9dc9d-210">Si noti che il supporto completo di IntelliSense viene immesso, in modo che quando si digita "@Model."</span><span class="sxs-lookup"><span data-stu-id="9dc9d-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="9dc9d-211">vengono visualizzati tutti i metodi e le proprietà supportati da un IEnumerable di tipo genere.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="9dc9d-212">All'interno del ciclo "foreach", Visual Web Developer sa che ogni elemento è di tipo genre, quindi viene visualizzato IntelliSense per ogni tipo di genere.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="9dc9d-213">Successivamente, la funzionalità di impalcatura ha esaminato l'oggetto Genre e ha determinato che ognuno avrà una proprietà Name, quindi esegue il ciclo e li scrive. Genera inoltre i collegamenti modifica, dettagli ed Elimina a ogni singolo elemento.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="9dc9d-214">Si utilizzerà questa funzionalità in un secondo momento nel gestore dello Store, ma per il momento si vuole usare un semplice elenco.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="9dc9d-215">Quando si esegue l'applicazione e si passa a/Store, si noterà che vengono visualizzati sia il conteggio che l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="9dc9d-216">Aggiunta di collegamenti tra pagine</span><span class="sxs-lookup"><span data-stu-id="9dc9d-216">Adding Links between pages</span></span>

<span data-ttu-id="9dc9d-217">L'URL di/Store che elenca i generi elenca attualmente i nomi di genere semplicemente come testo normale.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="9dc9d-218">Modifichiamo questa opzione in modo che, invece del testo normale, i nomi generici siano collegati all'URL/Store/Browse appropriato, in modo che facendo clic su un genere musicale come "discoteca" si passerà all'URL/Store/Browse? genre = discoteca.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="9dc9d-219">È possibile aggiornare il modello di visualizzazione \Views\Store\Index.cshtml per restituire questi collegamenti usando un codice simile al seguente **(non digitare questo in-verrà migliorato)** :</span><span class="sxs-lookup"><span data-stu-id="9dc9d-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="9dc9d-220">Funziona, ma potrebbe causare problemi in un secondo momento perché si basa su una stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="9dc9d-221">Se ad esempio si vuole rinominare il controller, è necessario eseguire una ricerca nel codice cercando i collegamenti che devono essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="9dc9d-222">Un approccio alternativo può essere usato per sfruttare i vantaggi di un metodo helper HTML.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="9dc9d-223">ASP.NET MVC include metodi helper HTML disponibili dal codice del modello di visualizzazione per eseguire una serie di attività comuni Analogamente a quanto segue.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="9dc9d-224">Il metodo helper HTML. ActionLink () è particolarmente utile e semplifica la creazione di HTML &lt;un&gt; collegamenti e si occupa di dettagli fastidiosi, ad esempio verificare che i percorsi URL siano codificati correttamente con URL.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="9dc9d-225">HTML. ActionLink () include diversi overload che consentono di specificare la quantità di informazioni necessaria per i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="9dc9d-226">Nel caso più semplice, si fornirà solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="9dc9d-227">Ad esempio, è possibile collegare il metodo "/Store/" index () nella pagina dei dettagli dell'archivio con il testo del collegamento "go to the store index" usando la chiamata seguente:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="9dc9d-228">*Nota: in questo caso, non è stato necessario specificare il nome del controller perché è in corso il collegamento a un'altra azione nello stesso controller che esegue il rendering della visualizzazione corrente.*</span><span class="sxs-lookup"><span data-stu-id="9dc9d-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="9dc9d-229">I collegamenti alla pagina Browse dovranno invece passare un parametro, quindi verrà usato un altro overload del metodo HTML. ActionLink che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="9dc9d-230">Testo del collegamento, che visualizzerà il nome del genere</span><span class="sxs-lookup"><span data-stu-id="9dc9d-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="9dc9d-231">Nome azione controller (Sfoglia)</span><span class="sxs-lookup"><span data-stu-id="9dc9d-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="9dc9d-232">Valori dei parametri di route, specificando il nome (genere) e il valore (nome del genere)</span><span class="sxs-lookup"><span data-stu-id="9dc9d-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="9dc9d-233">A questo punto, ecco come verranno scritti i collegamenti nella visualizzazione index Store:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="9dc9d-234">A questo punto, quando si esegue nuovamente il progetto e si accede all'URL/Store/, viene visualizzato un elenco di generi.</span><span class="sxs-lookup"><span data-stu-id="9dc9d-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="9dc9d-235">Ogni genere è un collegamento ipertestuale. quando si fa clic su di esso, si passerà all'URL/Store/Browse? genre = *[genre]* .</span><span class="sxs-lookup"><span data-stu-id="9dc9d-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="9dc9d-236">Il codice HTML per l'elenco genere è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="9dc9d-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="9dc9d-237">[Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="9dc9d-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
