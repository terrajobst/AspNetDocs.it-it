---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata dell'API Web da un'applicazione Windows Phone 8C#()-ASP.NET 4. x
author: rmcmurray
description: "Esercitazione con codice: creare un'applicazione API Web ASP.NET in ASP.NET 4. x che fornisce un catalogo di libri a un'applicazione Windows Phone 8."
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614737"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="37fed-103">Chiamata dell'API Web da un'applicazione Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="37fed-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>

<span data-ttu-id="37fed-104">di [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="37fed-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="37fed-105">In questa esercitazione si apprenderà come creare uno scenario end-to-end completo costituito da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="37fed-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="37fed-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="37fed-106">Overview</span></span>

<span data-ttu-id="37fed-107">I servizi RESTful come API Web ASP.NET semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori astraendo l'architettura per le applicazioni lato server e lato client.</span><span class="sxs-lookup"><span data-stu-id="37fed-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="37fed-108">Anziché creare un protocollo proprietario basato su socket per la comunicazione, gli sviluppatori di API Web devono semplicemente pubblicare i metodi HTTP necessari per la propria applicazione, ad esempio GET, POST, PUT, DELETE, e gli sviluppatori di applicazioni client devono solo utilizzare metodi HTTP necessari per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="37fed-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="37fed-109">In questa esercitazione end-to-end si apprenderà come usare l'API Web per creare i progetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="37fed-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="37fed-110">Nella [prima parte di questa esercitazione](#STEP1)si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per la gestione di un catalogo di libri.</span><span class="sxs-lookup"><span data-stu-id="37fed-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="37fed-111">Questa applicazione utilizzerà il [file XML di esempio (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) di MSDN.</span><span class="sxs-lookup"><span data-stu-id="37fed-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="37fed-112">Nella [seconda parte di questa esercitazione](#STEP2)si creerà un'applicazione interactive Windows Phone 8 che recupera i dati dall'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="37fed-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="37fed-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="37fed-113">Prerequisites</span></span>

- <span data-ttu-id="37fed-114">Visual Studio 2013 con Windows Phone 8 SDK installato</span><span class="sxs-lookup"><span data-stu-id="37fed-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="37fed-115">Windows 8 o versione successiva in un sistema a 64 bit con Hyper-V installato</span><span class="sxs-lookup"><span data-stu-id="37fed-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="37fed-116">Per un elenco di requisiti aggiuntivi, vedere la sezione *requisiti di sistema* nella pagina di download di [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .</span><span class="sxs-lookup"><span data-stu-id="37fed-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="37fed-117">Per eseguire il test della connettività tra l'API Web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni riportate nell'articolo *[connettere l'emulatore Windows Phone 8 all'API Web in un computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* per configurare l'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="37fed-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="37fed-118">Passaggio 1: creazione del progetto Bookstore dell'API Web</span><span class="sxs-lookup"><span data-stu-id="37fed-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="37fed-119">Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto API Web che supporta tutte le operazioni CRUD. Si noti che nel [passaggio 2](#STEP2) di questa esercitazione verrà aggiunto il progetto di applicazione Windows Phone alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="37fed-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="37fed-120">Aprire **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="37fed-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="37fed-121">Fare clic su **file**, quindi su **nuovo**e infine su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="37fed-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="37fed-122">Quando viene visualizzata la finestra di dialogo **nuovo progetto** , espandere **installato**, **modelli**, quindi **oggetto C#visivo** , quindi **Web**.</span><span class="sxs-lookup"><span data-stu-id="37fed-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="37fed-123">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-123">Click image to expand</span></span>                                                                |

4. <span data-ttu-id="37fed-124">Evidenziare **ASP.NET Web Application**, immettere **Bookstore** per il nome del progetto e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="37fed-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="37fed-125">Quando viene visualizzata la finestra di dialogo **nuovo progetto ASP.NET** , selezionare il modello **API Web** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="37fed-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="37fed-126">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-126">Click image to expand</span></span>                                                                |

6. <span data-ttu-id="37fed-127">Quando si apre il progetto API Web, rimuovere il controller di esempio dal progetto:</span><span class="sxs-lookup"><span data-stu-id="37fed-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="37fed-128">Espandere la cartella **controller** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="37fed-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="37fed-129">Fare clic con il pulsante destro del mouse sul file **ValuesController.cs** e scegliere **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="37fed-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="37fed-130">Quando viene richiesto di confermare l'eliminazione, fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="37fed-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="37fed-131">Aggiungere un file di dati XML al progetto API Web; Questo file contiene il contenuto del catalogo della libreria:</span><span class="sxs-lookup"><span data-stu-id="37fed-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="37fed-132">Fare clic con il pulsante destro del mouse sulla cartella **App\_data** in Esplora soluzioni, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="37fed-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="37fed-133">Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , evidenziare il modello di **file XML** .</span><span class="sxs-lookup"><span data-stu-id="37fed-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="37fed-134">Denominare il file **books. XML**e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="37fed-135">Quando si apre il file **books. XML** , sostituire il codice nel file con il codice XML del file **books. XML** di esempio su MSDN:</span><span class="sxs-lookup"><span data-stu-id="37fed-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="37fed-136">Salvare e chiudere il file XML.</span><span class="sxs-lookup"><span data-stu-id="37fed-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="37fed-137">Aggiungere il modello Bookstore al progetto API Web; Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione Bookstore:</span><span class="sxs-lookup"><span data-stu-id="37fed-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="37fed-138">Fare clic con il pulsante destro del mouse sulla cartella **Models** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="37fed-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="37fed-139">Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare il file di classe **bookdetails.cs**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="37fed-140">Quando viene aperto il file **bookdetails.cs** , sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="37fed-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="37fed-141">Salvare e chiudere il file **bookdetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="37fed-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="37fed-142">Aggiungere il controller Bookstore al progetto API Web:</span><span class="sxs-lookup"><span data-stu-id="37fed-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="37fed-143">Fare clic con il pulsante destro del mouse sulla cartella **controller** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **controller**.</span><span class="sxs-lookup"><span data-stu-id="37fed-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="37fed-144">Quando viene visualizzata la finestra di dialogo **Aggiungi impalcatura** , evidenziare **Controller Web API 2-vuoto**e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="37fed-145">Quando viene visualizzata la finestra di dialogo **Aggiungi controller** , denominare il controller **BooksController**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="37fed-146">Quando viene aperto il file **BooksController.cs** , sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="37fed-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="37fed-147">Salvare e chiudere il file **BooksController.cs** .</span><span class="sxs-lookup"><span data-stu-id="37fed-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="37fed-148">Compilare l'applicazione API Web per verificare la presenza di errori.</span><span class="sxs-lookup"><span data-stu-id="37fed-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="37fed-149">Passaggio 2: aggiunta del progetto di catalogo della libreria Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="37fed-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="37fed-150">Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="37fed-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="37fed-151">Questa applicazione userà il modello di *app di Windows Phone Data Binding* per l'interfaccia utente predefinita e userà l'applicazione API Web creata nel [passaggio 1](#STEP1) di questa esercitazione come origine dati.</span><span class="sxs-lookup"><span data-stu-id="37fed-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="37fed-152">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione **Bookstore** , quindi scegliere **Aggiungi**, quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="37fed-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="37fed-153">Quando viene visualizzata la finestra di dialogo **nuovo progetto** , espandere **installato**, quindi oggetto **visivo C#** , quindi **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="37fed-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="37fed-154">Evidenziare **Windows Phone app con associazione**a file, immettere **BookCatalog** per nome e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="37fed-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="37fed-155">Aggiungere il pacchetto NuGet Json.NET al progetto **BookCatalog** :</span><span class="sxs-lookup"><span data-stu-id="37fed-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="37fed-156">Fare clic con il pulsante destro del mouse su **riferimenti** per il progetto **BookCatalog** in Esplora soluzioni e quindi scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="37fed-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="37fed-157">Quando viene visualizzata la finestra di dialogo **Gestisci pacchetti NuGet** , espandere la sezione **Online** ed evidenziare **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="37fed-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="37fed-158">Immettere **JSON.NET** nel campo di ricerca e fare clic sull'icona di ricerca.</span><span class="sxs-lookup"><span data-stu-id="37fed-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="37fed-159">Evidenziare **JSON.NET** nei risultati della ricerca e quindi fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="37fed-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="37fed-160">Al termine dell'installazione, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="37fed-161">Aggiungere il modello **BookDetails** al progetto **BookCatalog** . contiene un modello generico della classe Bookstore:</span><span class="sxs-lookup"><span data-stu-id="37fed-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="37fed-162">Fare clic con il pulsante destro del mouse sul progetto **BookCatalog** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="37fed-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="37fed-163">Assegnare un nome ai nuovi **modelli**di cartella.</span><span class="sxs-lookup"><span data-stu-id="37fed-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="37fed-164">Fare clic con il pulsante destro del mouse sulla cartella **Models** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="37fed-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="37fed-165">Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare il file di classe **bookdetails.cs**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="37fed-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="37fed-166">Quando viene aperto il file **bookdetails.cs** , sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="37fed-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="37fed-167">Salvare e chiudere il file **bookdetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="37fed-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="37fed-168">Aggiornare la classe **MainViewModel.cs** per includere la funzionalità per comunicare con l'applicazione API Web della libreria:</span><span class="sxs-lookup"><span data-stu-id="37fed-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="37fed-169">Espandere la cartella **ViewModels** in Esplora soluzioni, quindi fare doppio clic sul file **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="37fed-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="37fed-170">Quando viene aperto il file **MainViewModel.cs** , sostituire il codice nel file con il codice seguente: Si noti che sarà necessario aggiornare il valore della costante `apiUrl` con l'URL effettivo dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="37fed-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="37fed-171">Salvare e chiudere il file **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="37fed-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="37fed-172">Aggiornare il file **MainPage. XAML** per personalizzare il nome dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="37fed-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="37fed-173">Fare doppio clic sul file **MainPage. XAML** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="37fed-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="37fed-174">Quando si apre il file **MainPage. XAML** , individuare le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="37fed-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="37fed-175">Sostituire le righe con le seguenti:</span><span class="sxs-lookup"><span data-stu-id="37fed-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="37fed-176">Salvare e chiudere il file **MainPage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="37fed-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="37fed-177">Aggiornare il file **DetailsPage. XAML** per personalizzare gli elementi visualizzati:</span><span class="sxs-lookup"><span data-stu-id="37fed-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="37fed-178">Fare doppio clic sul file **DetailsPage. XAML** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="37fed-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="37fed-179">Quando viene aperto il file **DetailsPage. XAML** , individuare le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="37fed-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="37fed-180">Sostituire le righe con le seguenti:</span><span class="sxs-lookup"><span data-stu-id="37fed-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="37fed-181">Salvare e chiudere il file **DetailsPage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="37fed-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="37fed-182">Compilare l'applicazione Windows Phone per verificare la presenza di errori.</span><span class="sxs-lookup"><span data-stu-id="37fed-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="37fed-183">Passaggio 3: test della soluzione end-to-end</span><span class="sxs-lookup"><span data-stu-id="37fed-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="37fed-184">Come indicato nella sezione *prerequisiti* di questa esercitazione, quando si esegue il test della connettività tra l'API web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni riportate nell'articolo *[connettere l'emulatore Windows Phone 8 all'API Web in un computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* per configurare l'ambiente di testing.</span><span class="sxs-lookup"><span data-stu-id="37fed-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="37fed-185">Una volta configurato l'ambiente di testing, sarà necessario impostare l'applicazione Windows Phone come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="37fed-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="37fed-186">A tale scopo, evidenziare l'applicazione **BookCatalog** in Esplora soluzioni e quindi fare clic su **Imposta come progetto di avvio**:</span><span class="sxs-lookup"><span data-stu-id="37fed-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="37fed-187">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-187">Click image to expand</span></span> |

<span data-ttu-id="37fed-188">Quando si preme F5, Visual Studio avvierà sia l'emulatore di Windows Phone, che visualizzerà una &quot;attendere&quot; messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:</span><span class="sxs-lookup"><span data-stu-id="37fed-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="37fed-189">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-189">Click image to expand</span></span> |

<span data-ttu-id="37fed-190">Se tutti gli elementi hanno esito positivo, verrà visualizzato il catalogo:</span><span class="sxs-lookup"><span data-stu-id="37fed-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="37fed-191">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-191">Click image to expand</span></span> |

<span data-ttu-id="37fed-192">Se si tocca un titolo di libro, l'applicazione visualizzerà la descrizione del libro:</span><span class="sxs-lookup"><span data-stu-id="37fed-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="37fed-193">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-193">Click image to expand</span></span> |

<span data-ttu-id="37fed-194">Se l'applicazione non è in grado di comunicare con l'API Web, verrà visualizzato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="37fed-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="37fed-195">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-195">Click image to expand</span></span> |

<span data-ttu-id="37fed-196">Se si tocca il messaggio di errore, verranno visualizzati altri dettagli sull'errore:</span><span class="sxs-lookup"><span data-stu-id="37fed-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="37fed-197">Fare clic sull'immagine per espanderla</span><span class="sxs-lookup"><span data-stu-id="37fed-197">Click image to expand</span></span>                                                                 |
