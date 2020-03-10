---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'app nel servizio app Azure di Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622388"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="bbadd-102">Pubblicare l'app nel servizio app Azure di Azure</span><span class="sxs-lookup"><span data-stu-id="bbadd-102">Publish the App to Azure Azure App Service</span></span>

<span data-ttu-id="bbadd-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bbadd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bbadd-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="bbadd-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="bbadd-105">Come ultimo passaggio, l'applicazione viene pubblicata in Azure.</span><span class="sxs-lookup"><span data-stu-id="bbadd-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="bbadd-106">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="bbadd-107">Fare clic su **pubblica** per richiamare la finestra di dialogo **pubblica sul Web** .</span><span class="sxs-lookup"><span data-stu-id="bbadd-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="bbadd-108">Se è stata selezionata l'opzione **host nel cloud** quando è stato creato per la prima volta il progetto, la connessione e le impostazioni sono già configurate.</span><span class="sxs-lookup"><span data-stu-id="bbadd-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="bbadd-109">In tal caso, è sufficiente fare clic sulla scheda **Impostazioni** e selezionare &quot;esegui migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="bbadd-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="bbadd-110">Se all'inizio non è stata selezionata l'opzione **host nel cloud** , attenersi alla procedura descritta nella [sezione successiva](#new-website).</span><span class="sxs-lookup"><span data-stu-id="bbadd-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="bbadd-111">Per distribuire l'app, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="bbadd-112">È possibile visualizzare lo stato di avanzamento della pubblicazione nella finestra **attività di pubblicazione Web** .</span><span class="sxs-lookup"><span data-stu-id="bbadd-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="bbadd-113">Scegliere **altre finestre**dal menu **Visualizza** , quindi selezionare **attività pubblicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="bbadd-114">Quando Visual Studio completa la distribuzione dell'app, il browser predefinito si apre automaticamente all'URL del sito Web distribuito e l'applicazione creata viene ora eseguita nel cloud.</span><span class="sxs-lookup"><span data-stu-id="bbadd-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="bbadd-115">L'URL nella barra degli indirizzi del browser indica che il sito è in fase di caricamento da Internet.</span><span class="sxs-lookup"><span data-stu-id="bbadd-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="bbadd-116">Distribuzione in un nuovo sito Web</span><span class="sxs-lookup"><span data-stu-id="bbadd-116">Deploying to a New Website</span></span>

<span data-ttu-id="bbadd-117">Se non è stata selezionata l'opzione **host nel cloud** al momento della creazione del progetto, è possibile configurare una nuova app Web.</span><span class="sxs-lookup"><span data-stu-id="bbadd-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="bbadd-118">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="bbadd-119">Selezionare la scheda **profilo** e fare clic su **Microsoft Azure siti Web**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="bbadd-120">Se non è attualmente stato effettuato l'accesso ad Azure, verrà richiesto di eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="bbadd-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="bbadd-121">Nella finestra di dialogo **siti Web esistenti** fare clic su **nuovo**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="bbadd-122">Immettere un nome di sito.</span><span class="sxs-lookup"><span data-stu-id="bbadd-122">Enter a site name.</span></span> <span data-ttu-id="bbadd-123">Selezionare la sottoscrizione di Azure e l'area.</span><span class="sxs-lookup"><span data-stu-id="bbadd-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="bbadd-124">In **server database**selezionare **Crea nuovo server**o selezionare un server esistente.</span><span class="sxs-lookup"><span data-stu-id="bbadd-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="bbadd-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="bbadd-126">Fare clic sulla scheda **Impostazioni** e selezionare &quot;esegui migrazioni Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="bbadd-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="bbadd-127">Quindi viene selezionato **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bbadd-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bbadd-128">Precedente</span><span class="sxs-lookup"><span data-stu-id="bbadd-128">Previous</span></span>](part-9.md)
