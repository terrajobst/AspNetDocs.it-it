---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Creazione del capitolo Download per le esercitazioni di EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 0e720b3e4c5d3b8f779afe3a6e2b47baa86eec4c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592712"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="2e052-103">Creazione del capitolo Download per le esercitazioni di EF 5 MVC 4</span><span class="sxs-lookup"><span data-stu-id="2e052-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="2e052-104">di [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="2e052-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="2e052-105">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="2e052-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="2e052-106">L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2e052-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="2e052-107">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2e052-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="2e052-108">Creazione dei download dei capitoli</span><span class="sxs-lookup"><span data-stu-id="2e052-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="2e052-109">Scaricare e decomprimere il file zip di esempio del progetto.</span><span class="sxs-lookup"><span data-stu-id="2e052-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="2e052-110">Nel pacchetto di download decompresso sono disponibili file zip aggiuntivi, uno per il completamento di ogni capitolo.</span><span class="sxs-lookup"><span data-stu-id="2e052-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="2e052-111">Fare clic con il pulsante destro del mouse sul file zip desiderato, scegliere **Proprietà**e fare clic sul pulsante **Sblocca** .</span><span class="sxs-lookup"><span data-stu-id="2e052-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="2e052-112">Decomprimere il file.</span><span class="sxs-lookup"><span data-stu-id="2e052-112">Unzip the file.</span></span>
4. <span data-ttu-id="2e052-113">Fare doppio clic sul file *CUx. sln* per avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e052-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="2e052-114">Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="2e052-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="2e052-115">Nella console di gestione pacchetti (PMC) fare clic su **Ripristina**.</span><span class="sxs-lookup"><span data-stu-id="2e052-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="2e052-116">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e052-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="2e052-117">Riavviare Visual Studio, aprendo il file della soluzione chiuso nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="2e052-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="2e052-118">Nella console di gestione pacchetti (PMC) immettere il comando `Update-Database`:</span><span class="sxs-lookup"><span data-stu-id="2e052-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="2e052-119">Se viene ricevuto il seguente errore:</span><span class="sxs-lookup"><span data-stu-id="2e052-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="2e052-120">*Il termine "Update-database" non è riconosciuto come nome di un cmdlet, di una funzione, di un file di script o di un programma eseguibile. Controllare l'ortografia del nome o, se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.*</span><span class="sxs-lookup"><span data-stu-id="2e052-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="2e052-121">Chiudere e riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e052-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="2e052-122">Ogni migrazione verrà eseguita, quindi verrà eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="2e052-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="2e052-123">È ora possibile eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2e052-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="2e052-124">Precedente</span><span class="sxs-lookup"><span data-stu-id="2e052-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
