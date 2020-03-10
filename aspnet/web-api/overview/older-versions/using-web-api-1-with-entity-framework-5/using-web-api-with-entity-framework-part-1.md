---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: panoramica e creazione del progetto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556077"
---
# <a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="85ee9-102">Parte 1: panoramica e creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="85ee9-102">Part 1: Overview and Creating the Project</span></span>

<span data-ttu-id="85ee9-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="85ee9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="85ee9-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="85ee9-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="85ee9-105">Entity Framework è un Framework di mapping relazionale a oggetti.</span><span class="sxs-lookup"><span data-stu-id="85ee9-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="85ee9-106">Esegue il mapping degli oggetti di dominio nel codice alle entità in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="85ee9-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="85ee9-107">Nella maggior parte dei casi, non è necessario preoccuparsi del livello di database, perché Entity Framework lo gestisce automaticamente.</span><span class="sxs-lookup"><span data-stu-id="85ee9-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="85ee9-108">Il codice manipola gli oggetti e le modifiche vengono salvate in modo permanente in un database.</span><span class="sxs-lookup"><span data-stu-id="85ee9-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="85ee9-109">Informazioni sull'esercitazione</span><span class="sxs-lookup"><span data-stu-id="85ee9-109">About the Tutorial</span></span>

<span data-ttu-id="85ee9-110">In questa esercitazione verrà creata una semplice applicazione di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="85ee9-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="85ee9-111">Sono presenti due parti principali per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85ee9-111">There are two main parts to the application.</span></span> <span data-ttu-id="85ee9-112">Gli utenti normali possono visualizzare i prodotti e creare gli ordini:</span><span class="sxs-lookup"><span data-stu-id="85ee9-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="85ee9-113">Gli amministratori possono creare, eliminare o modificare i prodotti:</span><span class="sxs-lookup"><span data-stu-id="85ee9-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="85ee9-114">Acquisizione di competenze</span><span class="sxs-lookup"><span data-stu-id="85ee9-114">Skills You'll Learn</span></span>

<span data-ttu-id="85ee9-115">In questa esercitazione si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="85ee9-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="85ee9-116">Come usare Entity Framework con API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="85ee9-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="85ee9-117">Come usare knockout. js per creare un'interfaccia utente client dinamica.</span><span class="sxs-lookup"><span data-stu-id="85ee9-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="85ee9-118">Come usare l'autenticazione basata su form con l'API Web per autenticare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="85ee9-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="85ee9-119">Sebbene questa esercitazione sia autonoma, è consigliabile leggere prima le esercitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="85ee9-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="85ee9-120">Creare la prima API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="85ee9-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="85ee9-121">Creazione di un'API Web che supporta operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="85ee9-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="85ee9-122">È inoltre utile una certa conoscenza di [ASP.NET MVC](../../../../mvc/index.md) .</span><span class="sxs-lookup"><span data-stu-id="85ee9-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="85ee9-123">Panoramica</span><span class="sxs-lookup"><span data-stu-id="85ee9-123">Overview</span></span>

<span data-ttu-id="85ee9-124">A livello generale, di seguito è illustrata l'architettura dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="85ee9-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="85ee9-125">ASP.NET MVC genera le pagine HTML per il client.</span><span class="sxs-lookup"><span data-stu-id="85ee9-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="85ee9-126">API Web ASP.NET espone le operazioni CRUD sui dati (prodotti e ordini).</span><span class="sxs-lookup"><span data-stu-id="85ee9-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="85ee9-127">Entity Framework converte i C# modelli utilizzati dall'API Web in entità di database.</span><span class="sxs-lookup"><span data-stu-id="85ee9-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="85ee9-128">Il diagramma seguente illustra il modo in cui gli oggetti di dominio sono rappresentati a diversi livelli dell'applicazione, ovvero il livello del database, il modello a oggetti e infine il formato wire, usato per trasmettere i dati al client tramite HTTP.</span><span class="sxs-lookup"><span data-stu-id="85ee9-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="85ee9-129">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85ee9-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="85ee9-130">È possibile creare il progetto Tutorial utilizzando Visual Web Developer Express o la versione completa di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85ee9-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="85ee9-131">Nella pagina **iniziale** fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="85ee9-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="85ee9-132">Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** .</span><span class="sxs-lookup"><span data-stu-id="85ee9-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="85ee9-133">In **Visual C#** Selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="85ee9-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="85ee9-134">Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="85ee9-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="85ee9-135">Denominare il progetto "ProductStore" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="85ee9-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="85ee9-136">Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare **applicazione Internet** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="85ee9-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="85ee9-137">Il modello "applicazione Internet" crea un'applicazione MVC ASP.NET che supporta l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="85ee9-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="85ee9-138">Se l'applicazione viene eseguita adesso, dispone già di alcune funzionalità:</span><span class="sxs-lookup"><span data-stu-id="85ee9-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="85ee9-139">I nuovi utenti possono registrarsi facendo clic sul collegamento "Register" nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="85ee9-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="85ee9-140">Gli utenti registrati possono eseguire l'accesso facendo clic sul collegamento "Accedi".</span><span class="sxs-lookup"><span data-stu-id="85ee9-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="85ee9-141">Le informazioni di appartenenza vengono rese permanente in un database che viene creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="85ee9-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="85ee9-142">Per ulteriori informazioni sull'autenticazione basata su form in ASP.NET MVC, vedere [procedura dettagliata: uso dell'autenticazione basata su form in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="85ee9-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="85ee9-143">Aggiornare il file CSS</span><span class="sxs-lookup"><span data-stu-id="85ee9-143">Update the CSS File</span></span>

<span data-ttu-id="85ee9-144">Questo passaggio è cosmetico, ma renderà il rendering delle pagine come le schermate precedenti.</span><span class="sxs-lookup"><span data-stu-id="85ee9-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="85ee9-145">In Esplora soluzioni espandere la cartella contenuto e aprire il file denominato site. CSS.</span><span class="sxs-lookup"><span data-stu-id="85ee9-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="85ee9-146">Aggiungere gli stili CSS seguenti:</span><span class="sxs-lookup"><span data-stu-id="85ee9-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="85ee9-147">avanti</span><span class="sxs-lookup"><span data-stu-id="85ee9-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
