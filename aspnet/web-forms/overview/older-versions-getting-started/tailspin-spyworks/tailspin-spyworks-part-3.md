---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menu layout e categoria | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639118"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="e4846-104">Parte 3: menu layout e categoria</span><span class="sxs-lookup"><span data-stu-id="e4846-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="e4846-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e4846-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e4846-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="e4846-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e4846-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="e4846-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e4846-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="e4846-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e4846-109">Nella parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.</span><span class="sxs-lookup"><span data-stu-id="e4846-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="e4846-110">Aggiunta di un layout e di un menu di categoria</span><span class="sxs-lookup"><span data-stu-id="e4846-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="e4846-111">Nella pagina master del sito verrà aggiunto un div per la colonna sul lato sinistro che conterrà il menu Product Category.</span><span class="sxs-lookup"><span data-stu-id="e4846-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="e4846-112">Si noti che l'allineamento e la formattazione desiderate verranno fornite dalla classe CSS aggiunta al file Style. CSS.</span><span class="sxs-lookup"><span data-stu-id="e4846-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="e4846-113">Il menu Categoria prodotto verrà creato dinamicamente in fase di esecuzione eseguendo una query sul database Commerce per le categorie di prodotti esistenti e creando le voci di menu e i collegamenti corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="e4846-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="e4846-114">A tale scopo, si utilizzeranno due di ASP. Controlli dati potenti di NET.</span><span class="sxs-lookup"><span data-stu-id="e4846-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="e4846-115">Il controllo "Entity Data Source" e il controllo "ListView".</span><span class="sxs-lookup"><span data-stu-id="e4846-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="e4846-116">Passiamo a "visualizzazione progettazione" e utilizzeremo gli helper per configurare i controlli.</span><span class="sxs-lookup"><span data-stu-id="e4846-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="e4846-117">Impostare la proprietà ID EntityDataSource sul menu EDS\_categoria\_e fare clic su "Configura origine dati".</span><span class="sxs-lookup"><span data-stu-id="e4846-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="e4846-118">Selezionare la connessione CommerceEntities creata per l'utente quando è stato creato il modello di origine dati dell'entità per il database Commerce e fare clic su "Avanti".</span><span class="sxs-lookup"><span data-stu-id="e4846-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="e4846-119">Selezionare il nome del set di entità "Categories" e lasciare le altre opzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="e4846-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="e4846-120">Fare clic su "fine".</span><span class="sxs-lookup"><span data-stu-id="e4846-120">Click "Finish".</span></span>

<span data-ttu-id="e4846-121">A questo punto è possibile impostare la proprietà ID dell'istanza del controllo ListView che è stata inserita nella pagina su ListView\_ProductsMenu e attivare il relativo helper.</span><span class="sxs-lookup"><span data-stu-id="e4846-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="e4846-122">Sebbene sia possibile utilizzare le opzioni di controllo per formattare la visualizzazione e la formattazione degli elementi di dati, la creazione del menu richiederà solo markup semplice, quindi il codice sarà immesso nella visualizzazione origine.</span><span class="sxs-lookup"><span data-stu-id="e4846-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="e4846-123">Si noti l'istruzione "eval": &lt;% # Eval ("CategoryName")%&gt;</span><span class="sxs-lookup"><span data-stu-id="e4846-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="e4846-124">La sintassi ASP.NET &lt;% #%&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire qualsiasi contenuto all'interno di e di restituire i risultati "nella riga".</span><span class="sxs-lookup"><span data-stu-id="e4846-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="e4846-125">L'istruzione eval ("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi degli elementi del modello di entità "CategoryName".</span><span class="sxs-lookup"><span data-stu-id="e4846-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="e4846-126">Si tratta di una sintassi concisa per una funzionalità molto potente.</span><span class="sxs-lookup"><span data-stu-id="e4846-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="e4846-127">Consente ora di eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4846-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="e4846-128">Si noti che il menu Product Category è ora visualizzato e quando si passa il puntatore del mouse su una delle voci di menu Category è possibile vedere che il collegamento voce di menu fa riferimento a una pagina che è ancora necessario implementare denominato Products. aspx e che è stato compilato un argomento della stringa di query dinamica che contiene  ID categoria.</span><span class="sxs-lookup"><span data-stu-id="e4846-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4846-129">[Precedente](tailspin-spyworks-part-2.md)
> [Successivo](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e4846-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
