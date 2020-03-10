---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Creazione di un controller (VB) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther illustra come è possibile aggiungere un controller a un'applicazione MVC ASP.NET.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601458"
---
# <a name="creating-a-controller-vb"></a><span data-ttu-id="2ef78-103">Creazione di un controller (VB)</span><span class="sxs-lookup"><span data-stu-id="2ef78-103">Creating a Controller (VB)</span></span>

<span data-ttu-id="2ef78-104">di [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2ef78-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2ef78-105">In questa esercitazione, Stephen Walther illustra come è possibile aggiungere un controller a un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ef78-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="2ef78-106">L'obiettivo di questa esercitazione è spiegare come è possibile creare nuovi controller MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ef78-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="2ef78-107">Si apprenderà come creare i controller usando l'opzione di menu Aggiungi controller di Visual Studio e creando manualmente un file di classe.</span><span class="sxs-lookup"><span data-stu-id="2ef78-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="2ef78-108">Utilizzo dell'opzione di menu Aggiungi controller</span><span class="sxs-lookup"><span data-stu-id="2ef78-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="2ef78-109">Il modo più semplice per creare un nuovo controller è fare clic con il pulsante destro del mouse sulla cartella controller nella finestra di Esplora soluzioni di Visual Studio e selezionare l'opzione di menu **Aggiungi, controller** (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="2ef78-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="2ef78-110">Selezionando questa opzione di menu viene visualizzata la finestra di dialogo **Aggiungi controller** (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="2ef78-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="2ef78-111">[![finestra di dialogo nuovo progetto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ef78-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="2ef78-112">**Figura 01**: aggiunta di un nuovo controller ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2ef78-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>

<span data-ttu-id="2ef78-113">[![finestra di dialogo nuovo progetto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2ef78-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="2ef78-114">**Figura 02**: finestra di dialogo Aggiungi controller ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="2ef78-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>

<span data-ttu-id="2ef78-115">Si noti che la prima parte del nome del controller viene evidenziata nella finestra di dialogo **Aggiungi controller** .</span><span class="sxs-lookup"><span data-stu-id="2ef78-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="2ef78-116">Ogni nome di controller deve terminare con il *controller*del suffisso.</span><span class="sxs-lookup"><span data-stu-id="2ef78-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="2ef78-117">Ad esempio, è possibile creare un controller denominato *ProductController* , ma non un controller denominato *Product*.</span><span class="sxs-lookup"><span data-stu-id="2ef78-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="2ef78-118">Se si crea un controller in cui manca il suffisso del *controller* , non sarà possibile richiamare il controller.</span><span class="sxs-lookup"><span data-stu-id="2ef78-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="2ef78-119">Non eseguire questa operazione: ho perso innumerevoli ore di vita dopo aver commesso questo errore.</span><span class="sxs-lookup"><span data-stu-id="2ef78-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="2ef78-120">**Listato 1-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="2ef78-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="2ef78-121">È sempre necessario creare i controller nella cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="2ef78-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="2ef78-122">In caso contrario, si violeranno le convenzioni di ASP.NET MVC e gli altri sviluppatori avranno un tempo più difficile per comprendere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2ef78-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="2ef78-123">Metodi di azione di impalcatura</span><span class="sxs-lookup"><span data-stu-id="2ef78-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="2ef78-124">Quando si crea un controller, è possibile scegliere di generare automaticamente metodi di azione di creazione, aggiornamento e dettagli (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="2ef78-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="2ef78-125">Se si seleziona questa opzione, viene generata la classe controller nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="2ef78-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="2ef78-126">[![la creazione automatica di metodi di azione](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2ef78-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="2ef78-127">**Figura 03**: creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2ef78-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>

<span data-ttu-id="2ef78-128">**Listato 2-Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="2ef78-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="2ef78-129">Questi metodi generati sono metodi stub.</span><span class="sxs-lookup"><span data-stu-id="2ef78-129">These generated methods are stub methods.</span></span> <span data-ttu-id="2ef78-130">È necessario aggiungere la logica effettiva per la creazione, l'aggiornamento e la visualizzazione dei dettagli per un cliente.</span><span class="sxs-lookup"><span data-stu-id="2ef78-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="2ef78-131">Tuttavia, i metodi dello stub forniscono un ottimo punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="2ef78-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="2ef78-132">Creazione di una classe controller</span><span class="sxs-lookup"><span data-stu-id="2ef78-132">Creating a Controller Class</span></span>

<span data-ttu-id="2ef78-133">Il controller MVC ASP.NET è semplicemente una classe.</span><span class="sxs-lookup"><span data-stu-id="2ef78-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="2ef78-134">Se si preferisce, è possibile ignorare la pratica impalcatura del controller di Visual Studio e creare manualmente una classe controller.</span><span class="sxs-lookup"><span data-stu-id="2ef78-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="2ef78-135">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="2ef78-135">Follow these steps:</span></span>

1. <span data-ttu-id="2ef78-136">Fare clic con il pulsante destro del mouse sulla cartella controller e selezionare l'opzione di menu **Aggiungi, nuovo elemento** e selezionare il modello di **classe** (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="2ef78-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="2ef78-137">Assegnare alla nuova classe il nome PersonController. vb, quindi fare clic sul pulsante **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="2ef78-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="2ef78-138">Modificare il file di classe risultante in modo che la classe erediti dalla classe System. Web. Mvc. controller di base (vedere l'elenco 3).</span><span class="sxs-lookup"><span data-stu-id="2ef78-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="2ef78-139">[![la creazione di una nuova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2ef78-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="2ef78-140">**Figura 04**: creazione di una nuova classe ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="2ef78-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>

<span data-ttu-id="2ef78-141">**Listato 3-Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="2ef78-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="2ef78-142">Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="2ef78-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="2ef78-143">È possibile richiamare questa azione del controller eseguendo l'applicazione e richiedendo un URL simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2ef78-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="2ef78-144">Il Server di sviluppo ASP.NET utilizza un numero di porta casuale, ad esempio 40071.</span><span class="sxs-lookup"><span data-stu-id="2ef78-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="2ef78-145">Quando si immette un URL per richiamare un controller, è necessario specificare il numero di porta corretto.</span><span class="sxs-lookup"><span data-stu-id="2ef78-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="2ef78-146">È possibile determinare il numero di porta posizionando il puntatore del mouse sull'icona del Server di sviluppo ASP.NET nell'area di notifica di Windows (in basso a destra dello schermo).</span><span class="sxs-lookup"><span data-stu-id="2ef78-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="2ef78-147">[Precedente](adding-dynamic-content-to-a-cached-page-vb.md)
> [Successivo](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2ef78-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
