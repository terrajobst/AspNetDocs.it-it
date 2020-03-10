---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: modelli e accesso ai dati | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 4 illustra i modelli e l'accesso ai dati.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559675"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="0858d-104">Parte 4: modelli e accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="0858d-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="0858d-105">di [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0858d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0858d-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="0858d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0858d-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0858d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="0858d-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="0858d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0858d-109">La parte 4 illustra i modelli e l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="0858d-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="0858d-110">Fino ad ora, abbiamo appena passato "dati fittizi" dai nostri controller ai nostri modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0858d-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="0858d-111">A questo punto è possibile collegare un database reale.</span><span class="sxs-lookup"><span data-stu-id="0858d-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="0858d-112">In questa esercitazione verrà illustrato come usare SQL Server Compact Edition (spesso denominate SQL CE) come motore di database.</span><span class="sxs-lookup"><span data-stu-id="0858d-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="0858d-113">SQL CE è un database gratuito, incorporato e basato su file che non richiede alcuna installazione o configurazione, il che lo rende molto utile per lo sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="0858d-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="0858d-114">Accesso al database con Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="0858d-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="0858d-115">Verrà usato il supporto di Entity Framework (EF) incluso nei progetti ASP.NET MVC 3 per eseguire query e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="0858d-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="0858d-116">EF è un'API dati ORM (Flexible Object Relational Mapping) che consente agli sviluppatori di eseguire query e aggiornare i dati archiviati in un database in modo orientato agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="0858d-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="0858d-117">Entity Framework versione 4 supporta un paradigma di sviluppo denominato Code-First.</span><span class="sxs-lookup"><span data-stu-id="0858d-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="0858d-118">Code-First consente di creare un oggetto modello scrivendo classi semplici, note anche come oggetti POCO da oggetti CLR "Plain-Old", ed è anche in grado di creare il database in tempo reale dalle classi.</span><span class="sxs-lookup"><span data-stu-id="0858d-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="0858d-119">Modifiche alle classi del modello</span><span class="sxs-lookup"><span data-stu-id="0858d-119">Changes to our Model Classes</span></span>

<span data-ttu-id="0858d-120">In questa esercitazione verrà sfruttata la funzionalità di creazione del database in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0858d-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="0858d-121">Prima di eseguire questa operazione, tuttavia, verranno apportate alcune modifiche minime alle classi del modello da aggiungere ad alcuni elementi che verranno usati in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="0858d-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="0858d-122">Aggiunta delle classi del modello di artista</span><span class="sxs-lookup"><span data-stu-id="0858d-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="0858d-123">Gli album verranno associati agli artisti, quindi verrà aggiunta una semplice classe modello per descrivere un artista.</span><span class="sxs-lookup"><span data-stu-id="0858d-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="0858d-124">Aggiungere una nuova classe alla cartella Models denominata Artist.cs usando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="0858d-125">Aggiornamento delle classi di modelli</span><span class="sxs-lookup"><span data-stu-id="0858d-125">Updating our Model Classes</span></span>

<span data-ttu-id="0858d-126">Aggiornare la classe album come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="0858d-127">Quindi, effettuare gli aggiornamenti seguenti alla classe Genre.</span><span class="sxs-lookup"><span data-stu-id="0858d-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="0858d-128">Aggiunta della cartella di dati dell'app\_</span><span class="sxs-lookup"><span data-stu-id="0858d-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="0858d-129">Si aggiungerà un'app\_directory dati al progetto per conservare i file di SQL Server Express database.</span><span class="sxs-lookup"><span data-stu-id="0858d-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="0858d-130">App\_data è una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="0858d-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="0858d-131">Dal menu progetto selezionare Aggiungi cartella ASP.NET, quindi dati app\_.</span><span class="sxs-lookup"><span data-stu-id="0858d-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="0858d-132">Creazione di una stringa di connessione nel file Web. config</span><span class="sxs-lookup"><span data-stu-id="0858d-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="0858d-133">Si aggiungeranno alcune righe al file di configurazione del sito Web in modo che Entity Framework sappia come connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="0858d-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="0858d-134">Fare doppio clic sul file Web. config che si trova nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="0858d-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="0858d-135">Scorrere fino alla fine del file e aggiungere una &lt;connectionStrings&gt; sezione immediatamente sopra l'ultima riga, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="0858d-136">Aggiunta di una classe Context</span><span class="sxs-lookup"><span data-stu-id="0858d-136">Adding a Context Class</span></span>

<span data-ttu-id="0858d-137">Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una nuova classe denominata MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="0858d-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="0858d-138">Questa classe rappresenterà il contesto Entity Framework database e gestirà le operazioni di creazione, lettura, aggiornamento ed eliminazione per Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0858d-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="0858d-139">Il codice per questa classe è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="0858d-140">Non sono disponibili altre configurazioni, interfacce speciali e così via. Estendendo la classe di base DbContext, la classe MusicStoreEntities è in grado di gestire le operazioni di database.</span><span class="sxs-lookup"><span data-stu-id="0858d-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="0858d-141">Ora che il collegamento è stato aggiunto, è possibile aggiungere altre proprietà alle classi del modello per sfruttare alcune delle informazioni aggiuntive del database.</span><span class="sxs-lookup"><span data-stu-id="0858d-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="0858d-142">Aggiunta dei dati del catalogo dei negozi</span><span class="sxs-lookup"><span data-stu-id="0858d-142">Adding our store catalog data</span></span>

<span data-ttu-id="0858d-143">Si approfitterà di una funzionalità di Entity Framework che aggiunge dati di "seeding" a un nuovo database creato.</span><span class="sxs-lookup"><span data-stu-id="0858d-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="0858d-144">Il catalogo dei negozi verrà pre-popolato con un elenco di generi, artisti e album.</span><span class="sxs-lookup"><span data-stu-id="0858d-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="0858d-145">Il download di MvcMusicStore-Assets. zip, che include i file di progettazione del sito usati in precedenza in questa esercitazione, include un file di classe con questi dati di inizializzazione, che si trovano in una cartella denominata code.</span><span class="sxs-lookup"><span data-stu-id="0858d-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="0858d-146">Nella cartella Code/Models individuare il file SampleData.cs e rilasciarlo nella cartella Models del progetto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="0858d-147">A questo punto è necessario aggiungere una riga di codice per indicare Entity Framework sulla classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="0858d-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="0858d-148">Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente alla parte superiore del metodo di avvio dell'applicazione\_.</span><span class="sxs-lookup"><span data-stu-id="0858d-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="0858d-149">A questo punto, abbiamo completato il lavoro necessario per configurare Entity Framework per il progetto.</span><span class="sxs-lookup"><span data-stu-id="0858d-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="0858d-150">Esecuzione di query sul database</span><span class="sxs-lookup"><span data-stu-id="0858d-150">Querying the Database</span></span>

<span data-ttu-id="0858d-151">A questo punto è possibile aggiornare il StoreController in modo che, invece di usare "dati fittizi", chiami il database per eseguire una query su tutte le informazioni.</span><span class="sxs-lookup"><span data-stu-id="0858d-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="0858d-152">Si inizierà dichiarando un campo in **StoreController** per mantenere un'istanza della classe MusicStoreEntities, denominata storeDB:</span><span class="sxs-lookup"><span data-stu-id="0858d-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="0858d-153">Aggiornamento dell'indice dell'archivio per eseguire una query sul database</span><span class="sxs-lookup"><span data-stu-id="0858d-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="0858d-154">La classe MusicStoreEntities viene gestita dal Entity Framework ed espone una proprietà di raccolta per ogni tabella del database.</span><span class="sxs-lookup"><span data-stu-id="0858d-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="0858d-155">Aggiornare l'azione dell'indice di StoreController per recuperare tutti i generi nel database.</span><span class="sxs-lookup"><span data-stu-id="0858d-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="0858d-156">In precedenza è stato fatto con i dati di stringa a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="0858d-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="0858d-157">A questo punto è possibile usare la raccolta Entity Framework context Genres:</span><span class="sxs-lookup"><span data-stu-id="0858d-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="0858d-158">Non è necessario apportare modifiche al modello di vista perché stiamo ancora restituendo lo stesso StoreIndexViewModel che abbiamo restituito prima. stiamo semplicemente restituendo i dati in tempo reale dal database.</span><span class="sxs-lookup"><span data-stu-id="0858d-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="0858d-159">Quando si esegue nuovamente il progetto e si visita l'URL "/Store", verrà ora visualizzato un elenco di tutti i generi nel database:</span><span class="sxs-lookup"><span data-stu-id="0858d-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="0858d-160">Aggiornamento di Esplora archivi e dettagli per l'uso di dati dinamici</span><span class="sxs-lookup"><span data-stu-id="0858d-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="0858d-161">Con il metodo di azione/Store/Browse? genre = *[some-genre]* si sta cercando un genere in base al nome.</span><span class="sxs-lookup"><span data-stu-id="0858d-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="0858d-162">Si prevede solo un risultato, poiché non dovrebbero essere presenti due voci per lo stesso nome del genere, quindi è possibile usare. Estensione Single () in LINQ to query per l'oggetto Genre appropriato, ad esempio (non digitare ancora):</span><span class="sxs-lookup"><span data-stu-id="0858d-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="0858d-163">Il metodo singolo accetta un'espressione lambda come parametro, che specifica che è necessario un singolo oggetto Genre in modo tale che il nome corrisponda al valore definito.</span><span class="sxs-lookup"><span data-stu-id="0858d-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="0858d-164">Nel caso precedente, è in corso il caricamento di un singolo oggetto Genre con un valore name corrispondente al disco.</span><span class="sxs-lookup"><span data-stu-id="0858d-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="0858d-165">Si utilizzerà un Entity Framework funzionalità che consente di indicare anche altre entità correlate che si desidera caricare quando viene recuperato l'oggetto Genre.</span><span class="sxs-lookup"><span data-stu-id="0858d-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="0858d-166">Questa funzionalità è denominata shaping dei risultati della query e consente di ridurre il numero di volte in cui è necessario accedere al database per recuperare tutte le informazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="0858d-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="0858d-167">Si vuole eseguire il recupero preliminare degli album per il genere recuperato, quindi la query verrà aggiornata in modo da includere i generi. includere ("album") per indicare che si desiderano anche gli album correlati.</span><span class="sxs-lookup"><span data-stu-id="0858d-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="0858d-168">Questa operazione è più efficiente, perché recupererà i dati di genere e di album in una singola richiesta di database.</span><span class="sxs-lookup"><span data-stu-id="0858d-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="0858d-169">Con le spiegazioni, ecco come appare l'azione aggiornata del controller di esplorazione:</span><span class="sxs-lookup"><span data-stu-id="0858d-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="0858d-170">È ora possibile aggiornare la visualizzazione Store Browse per visualizzare gli album disponibili in ogni genere.</span><span class="sxs-lookup"><span data-stu-id="0858d-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="0858d-171">Aprire il modello di visualizzazione (disponibile in/Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0858d-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="0858d-172">Eseguire l'applicazione e passare a/Store/Browse? genre = jazz Mostra che i risultati sono ora estratti dal database, visualizzando tutti gli album del genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="0858d-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="0858d-173">Verrà apportata la stessa modifica all'URL/Store/Details/[ID] e verranno sostituiti i dati fittizi con una query di database che carica un album il cui ID corrisponde al valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="0858d-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="0858d-174">L'esecuzione dell'applicazione e l'esplorazione di/Store/Details/1 mostrano che i risultati vengono ora estratti dal database.</span><span class="sxs-lookup"><span data-stu-id="0858d-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="0858d-175">Ora che la pagina dei dettagli del negozio è configurata in modo da visualizzare un album con l'ID album, verrà aggiornata la visualizzazione **Sfoglia** per il collegamento alla visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="0858d-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="0858d-176">Si userà HTML. ActionLink, esattamente come è stato fatto per il collegamento da store index all'archivio Browse alla fine della sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="0858d-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="0858d-177">Di seguito è riportata l'origine completa per la visualizzazione Browse.</span><span class="sxs-lookup"><span data-stu-id="0858d-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="0858d-178">A questo punto è possibile passare dalla pagina del negozio a una pagina del genere, in cui sono elencati gli album disponibili, facendo clic su un album per visualizzare i dettagli dell'album.</span><span class="sxs-lookup"><span data-stu-id="0858d-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="0858d-179">[Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="0858d-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
