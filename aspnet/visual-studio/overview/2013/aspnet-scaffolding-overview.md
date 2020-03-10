---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Impalcature ASP.NET in Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: L'impalcatura ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557981"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="1991e-103">Scaffolding di ASP.NET in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1991e-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>

<span data-ttu-id="1991e-104">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1991e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1991e-105">L'impalcatura ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1991e-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>

## <a name="overview"></a><span data-ttu-id="1991e-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="1991e-106">Overview</span></span>

<span data-ttu-id="1991e-107">L'impalcatura ASP.NET è un Framework per la generazione di codice per le applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1991e-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="1991e-108">Visual Studio 2013 include i generatori di codice preinstallati per i progetti MVC e API Web.</span><span class="sxs-lookup"><span data-stu-id="1991e-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="1991e-109">Quando si vuole aggiungere rapidamente codice che interagisce con i modelli di dati, si aggiungono le impalcature al progetto.</span><span class="sxs-lookup"><span data-stu-id="1991e-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="1991e-110">L'uso dell'impalcatura può ridurre la quantità di tempo necessario per sviluppare operazioni dati standard nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1991e-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="1991e-111">Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto Web Form, ma è possibile usare l'impalcatura con i Web form aggiungendo dipendenze MVC al progetto o installando un'estensione.</span><span class="sxs-lookup"><span data-stu-id="1991e-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="1991e-112">Di seguito sono illustrati entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="1991e-112">Both approaches are shown below.</span></span>

<span data-ttu-id="1991e-113">Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere l'impalcatura ASP.NET per soddisfare i requisiti dello scenario.</span><span class="sxs-lookup"><span data-stu-id="1991e-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="1991e-114">Con questa funzionalità è possibile creare un modello di impalcatura personalizzato e aggiungerlo alla finestra di dialogo Aggiungi nuova impalcatura.</span><span class="sxs-lookup"><span data-stu-id="1991e-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="1991e-115">All'interno del modello personalizzato, si specifica il codice generato quando si aggiunge un elemento con impalcatura.</span><span class="sxs-lookup"><span data-stu-id="1991e-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="1991e-116">Per ulteriori informazioni, vedere la pagina relativa alla [creazione di un'impalcatura personalizzata per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="1991e-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1991e-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1991e-117">Prerequisites</span></span>

<span data-ttu-id="1991e-118">Per usare l'impalcatura ASP.NET, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="1991e-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="1991e-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1991e-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="1991e-120">Strumenti di sviluppo Web (parte dell'installazione Visual Studio 2013 predefinita)</span><span class="sxs-lookup"><span data-stu-id="1991e-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="1991e-121">Framework e strumenti Web di ASP.NET 2013 (parte dell'installazione predefinita di Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="1991e-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="1991e-122">Aggiungere un elemento con impalcatura a MVC o API Web</span><span class="sxs-lookup"><span data-stu-id="1991e-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="1991e-123">Per aggiungere un patibolo, fare clic con il pulsante destro del mouse sul progetto o su una cartella all'interno del progetto e selezionare **Aggiungi** , **nuovo elemento con impalcatura**, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="1991e-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Aggiungi elemento impalcatura](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="1991e-125">Dalla finestra **Aggiungi impalcatura** selezionare il tipo di impalcatura da aggiungere.</span><span class="sxs-lookup"><span data-stu-id="1991e-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Selezionare il tipo di impalcatura](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="1991e-127">La finestra **Aggiungi controller** consente di selezionare le opzioni per la generazione del controller, ad esempio se si vogliono usare le nuove funzionalità asincrone di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1991e-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="1991e-129">Le classi e le pagine pertinenti vengono create per lo scenario in uso.</span><span class="sxs-lookup"><span data-stu-id="1991e-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="1991e-130">Ad esempio, l'immagine seguente mostra il controller MVC e le visualizzazioni create tramite l'impalcatura per una classe modello denominata Movies.</span><span class="sxs-lookup"><span data-stu-id="1991e-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![File creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="1991e-132">Aggiungere un elemento con impalcatura a Web Form</span><span class="sxs-lookup"><span data-stu-id="1991e-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="1991e-133">Per aggiungere l'impalcatura che genera codice Web Form, è necessario installare un'estensione in Visual Studio o aggiungere dipendenze MVC.</span><span class="sxs-lookup"><span data-stu-id="1991e-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="1991e-134">Entrambi gli approcci sono illustrati di seguito, ma è sufficiente eseguire uno di questi approcci.</span><span class="sxs-lookup"><span data-stu-id="1991e-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="1991e-135">Estensione di ponteggi Web Form</span><span class="sxs-lookup"><span data-stu-id="1991e-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="1991e-136">È possibile installare un'estensione di Visual Studio che consente di usare l'impalcatura con un progetto Web Form.</span><span class="sxs-lookup"><span data-stu-id="1991e-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="1991e-137">In Visual Studio selezionare **strumenti** , quindi **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="1991e-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="1991e-138">Da questa finestra di dialogo cercare in Visual Studio Gallery per l' **impalcatura di Web Form**.</span><span class="sxs-lookup"><span data-stu-id="1991e-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![installare l'impalcatura di Web Form](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="1991e-140">Per ulteriori informazioni, vedere [ponteggi Web Form](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="1991e-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="1991e-141">Dipendenze MVC</span><span class="sxs-lookup"><span data-stu-id="1991e-141">MVC Dependencies</span></span>

<span data-ttu-id="1991e-142">Per aggiungere le dipendenze MVC, selezionare **aggiungi** - **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="1991e-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="1991e-143">Nella finestra Aggiungi impalcatura selezionare **dipendenze MVC**, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1991e-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Aggiungi dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="1991e-145">Sono disponibili due opzioni per l'impalcatura MVC; Minimo e completo.</span><span class="sxs-lookup"><span data-stu-id="1991e-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="1991e-146">Se si seleziona minimal, al progetto vengono aggiunti solo i pacchetti NuGet e i riferimenti per ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1991e-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="1991e-147">Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="1991e-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="1991e-148">Per usare facilmente l'impalcatura, selezionare dipendenze complete.</span><span class="sxs-lookup"><span data-stu-id="1991e-148">To easily use scaffolding, select Full dependencies.</span></span>

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="1991e-150">Dopo aver aggiunto le dipendenze, viene visualizzato un file **Readme. txt** .</span><span class="sxs-lookup"><span data-stu-id="1991e-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="1991e-151">Seguire attentamente le istruzioni riportate in questo file per assicurarsi che il progetto funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="1991e-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="1991e-152">Una volta completati i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento con impalcatura, come illustrato nella sezione precedente su MVC e l'API Web.</span><span class="sxs-lookup"><span data-stu-id="1991e-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="1991e-153">Le visualizzazioni e il controller generati automaticamente funzioneranno correttamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1991e-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="1991e-154">Esercitazioni</span><span class="sxs-lookup"><span data-stu-id="1991e-154">Tutorials</span></span>

<span data-ttu-id="1991e-155">Per creare un'impalcatura personalizzata, vedere [la pagina relativa alla creazione di un'impalcatura personalizzata per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="1991e-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="1991e-156">Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo nuovo elemento con impalcature](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="1991e-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="1991e-157">Per un esempio di come usare l'impalcatura con **database First sviluppo**, vedere [EF database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="1991e-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="1991e-158">Per un esempio di utilizzo dell'impalcatura in un progetto **MVC** , vedere [Introduzione con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="1991e-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="1991e-159">Per un esempio di come usare l'impalcatura in un progetto **API Web** , vedere [creare un'API REST con routing degli attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="1991e-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
