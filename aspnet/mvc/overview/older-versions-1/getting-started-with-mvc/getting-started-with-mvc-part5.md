---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accesso ai dati del modello da un controller | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543750"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="85534-104">Accesso ai dati del modello da un controller</span><span class="sxs-lookup"><span data-stu-id="85534-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="85534-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="85534-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="85534-106">Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85534-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="85534-107">Verrà creata una semplice applicazione Web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="85534-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="85534-108">Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85534-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="85534-109">In questa sezione si creerà una nuova classe MoviesController e si scriverà il codice che recupera i dati dei film e li visualizza nuovamente nel browser usando un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="85534-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="85534-110">Fare clic con il pulsante destro del mouse sulla cartella Controllers e creare un nuovo MoviesController.</span><span class="sxs-lookup"><span data-stu-id="85534-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="85534-111">[![Aggiungi controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="85534-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="85534-112">Verrà creato un nuovo file "MoviesController.cs" sotto la cartella \Controllers all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="85534-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="85534-113">Aggiornare MovieController per recuperare l'elenco di filmati dal database appena popolato.</span><span class="sxs-lookup"><span data-stu-id="85534-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="85534-114">Viene eseguita una query LINQ per recuperare solo i film rilasciati dopo l'estate del 1984.</span><span class="sxs-lookup"><span data-stu-id="85534-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="85534-115">È necessario un modello di visualizzazione per eseguire il rendering di questo elenco di filmati, quindi fare clic con il pulsante destro del mouse sul metodo e scegliere Aggiungi visualizzazione per crearlo.</span><span class="sxs-lookup"><span data-stu-id="85534-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="85534-116">Nella finestra di dialogo Aggiungi visualizzazione si indicherà che verrà passato un elenco&lt;Movies. Models. Movie&gt; al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="85534-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="85534-117">Diversamente dalle volte precedenti è stata usata la finestra di dialogo Aggiungi visualizzazione e si è scelto di creare un modello "vuoto", questa volta indicheremo che Visual Studio dovrà eseguire automaticamente il "patibolo" di un modello di visualizzazione per Microsoft con un contenuto predefinito.</span><span class="sxs-lookup"><span data-stu-id="85534-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="85534-118">Questa operazione verrà eseguita selezionando l'elemento "list" nel menu a discesa Visualizza contenuto.</span><span class="sxs-lookup"><span data-stu-id="85534-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="85534-119">Tenere presente che, quando è stata creata una nuova classe, è necessario compilare l'applicazione affinché venga visualizzata nella finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="85534-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Aggiungi visualizzazione](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="85534-121">Fare clic su Aggiungi e il sistema genererà automaticamente il codice per una vista che Visualizza l'elenco dei filmati.</span><span class="sxs-lookup"><span data-stu-id="85534-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="85534-122">Questo è il momento giusto per modificare l'intestazione &lt;H2&gt; su un valore simile a "My Movie List", come in precedenza con la visualizzazione Hello World.</span><span class="sxs-lookup"><span data-stu-id="85534-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="85534-123">[![Movies-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="85534-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="85534-124">Eseguire l'applicazione e visitare/Movies nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="85534-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="85534-125">Ora che sono stati recuperati i dati dal database usando una query di base all'interno del controller e i dati sono stati restituiti a una vista che conosce i film.</span><span class="sxs-lookup"><span data-stu-id="85534-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="85534-126">Tale visualizzazione consente quindi di scorrere l'elenco dei film e di creare una tabella di dati per noi.</span><span class="sxs-lookup"><span data-stu-id="85534-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="85534-127">[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="85534-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="85534-128">Non verranno implementate le funzionalità di modifica, dettagli ed eliminazione con questa applicazione, quindi non sono necessari i collegamenti predefiniti creati dal modello di impalcatura.</span><span class="sxs-lookup"><span data-stu-id="85534-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="85534-129">Aprire il file/Movies/Index.aspx e rimuoverli.</span><span class="sxs-lookup"><span data-stu-id="85534-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="85534-130">Ecco il codice sorgente per il modello di vista aggiornato che dovrebbe essere simile dopo aver apportato queste modifiche:</span><span class="sxs-lookup"><span data-stu-id="85534-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="85534-131">Si tratta di creare collegamenti che non saranno necessari, quindi verranno eliminati per questo esempio.</span><span class="sxs-lookup"><span data-stu-id="85534-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="85534-132">Tuttavia, verrà mantenuto il collegamento Crea nuovo.</span><span class="sxs-lookup"><span data-stu-id="85534-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="85534-133">Di seguito è riportato l'aspetto dell'app con la colonna rimossa.</span><span class="sxs-lookup"><span data-stu-id="85534-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="85534-134">[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="85534-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="85534-135">È ora disponibile un semplice elenco di dati dei film.</span><span class="sxs-lookup"><span data-stu-id="85534-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="85534-136">Tuttavia, se si fa clic sul collegamento "Crea nuovo", viene ricevuto un errore perché non è collegato.</span><span class="sxs-lookup"><span data-stu-id="85534-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="85534-137">Viene ora implementato un metodo di azione create che consente a un utente di immettere nuovi film nel database.</span><span class="sxs-lookup"><span data-stu-id="85534-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85534-138">[Precedente](getting-started-with-mvc-part4.md)
> [Successivo](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="85534-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
