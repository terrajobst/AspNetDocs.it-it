---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introduzione a AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Scopri tutte le informazioni necessarie per iniziare a usare AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621604"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="34f6a-103">Introduzione ad AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="34f6a-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="34f6a-104">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="34f6a-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="34f6a-105">Scopri tutte le informazioni necessarie per iniziare a usare AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="34f6a-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="34f6a-106">AJAX Control Toolkit contiene più di 30 controlli gratuiti che è possibile usare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34f6a-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="34f6a-107">In questa esercitazione si apprenderà come scaricare AJAX Control Toolkit e aggiungere i controlli del Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="34f6a-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="34f6a-108">Download di AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="34f6a-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="34f6a-109">[AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della community di ASP.NET e dal team di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34f6a-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="34f6a-110">[![download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="34f6a-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="34f6a-111">**Figura 01**: download di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="34f6a-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="34f6a-112">Dopo aver scaricato il file, è necessario sbloccare il file.</span><span class="sxs-lookup"><span data-stu-id="34f6a-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="34f6a-113">Fare clic con il pulsante destro del mouse sul file, scegliere Proprietà e fare clic sul pulsante **Sblocca** (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="34f6a-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="34f6a-114">[![sbloccare il file ZIP AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="34f6a-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="34f6a-115">**Figura 02**: sblocco del file zip di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="34f6a-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="34f6a-116">Dopo aver sbloccato il file, è possibile decomprimere il file: fare clic con il pulsante destro del mouse sul file e selezionare l'opzione di menu **Estrai tutto** .</span><span class="sxs-lookup"><span data-stu-id="34f6a-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="34f6a-117">A questo punto, è possibile aggiungere il Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="34f6a-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="34f6a-118">Aggiunta di AJAX Control Toolkit alla casella degli strumenti</span><span class="sxs-lookup"><span data-stu-id="34f6a-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="34f6a-119">Il modo più semplice per utilizzare AJAX Control Toolkit consiste nell'aggiungere il Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="34f6a-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="34f6a-120">In questo modo, è possibile trascinare semplicemente un controllo Toolkit in una pagina quando si desidera utilizzarlo.</span><span class="sxs-lookup"><span data-stu-id="34f6a-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="34f6a-121">[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="34f6a-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="34f6a-122">**Figura 03**: AJAX Control Toolkit viene visualizzato nella casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="34f6a-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="34f6a-123">Per prima cosa, è necessario aggiungere una scheda AJAX Control Toolkit alla casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="34f6a-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="34f6a-124">Seguire questa procedura.</span><span class="sxs-lookup"><span data-stu-id="34f6a-124">Follow these steps.</span></span>

1. <span data-ttu-id="34f6a-125">Creare un nuovo sito Web ASP.NET selezionando l'opzione di menu file, nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="34f6a-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="34f6a-126">Fare doppio clic su default. aspx nella finestra Esplora soluzioni per aprire il file nell'editor.</span><span class="sxs-lookup"><span data-stu-id="34f6a-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="34f6a-127">Fare clic con il pulsante destro del mouse sulla casella degli strumenti sotto la scheda generale e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="34f6a-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="34f6a-128">Immettere una nuova scheda denominata AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="34f6a-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="34f6a-129">[![l'aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="34f6a-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="34f6a-130">**Figura 04**: aggiunta di una nuova scheda ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="34f6a-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="34f6a-131">Successivamente, è necessario aggiungere i controlli AJAX Control Toolkit alla nuova scheda. attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="34f6a-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="34f6a-132">Fare clic con il pulsante destro del mouse sotto la scheda AJAX Control Toolkit e selezionare l'opzione di menu **Choose Items (vedere la figura 5)** .</span><span class="sxs-lookup"><span data-stu-id="34f6a-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="34f6a-133">Passare al percorso in cui è stato decompresso AJAX Control Toolkit e selezionare l'assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="34f6a-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="34f6a-134">[![scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="34f6a-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="34f6a-135">**Figura 05**: scegliere gli elementi da aggiungere alla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="34f6a-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="34f6a-136">Dopo aver completato questi passaggi, tutti i controlli del Toolkit verranno visualizzati nella casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="34f6a-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="34f6a-137">Aggiornamento a una nuova versione del Toolkit</span><span class="sxs-lookup"><span data-stu-id="34f6a-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="34f6a-138">Se si usa una versione precedente del Toolkit e ora è necessario passare a una versione successiva, di seguito sono riportati i passaggi consigliati:</span><span class="sxs-lookup"><span data-stu-id="34f6a-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="34f6a-139">Binari: eliminare la versione precedente dell'assembly AjaxControlToolkit. dll dalla cartella bin del sito Web.</span><span class="sxs-lookup"><span data-stu-id="34f6a-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="34f6a-140">Elementi della casella degli strumenti: eliminare la scheda AJAX Control Toolkit e seguire i passaggi precedenti per ricreare la scheda con la nuova versione dell'assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="34f6a-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="34f6a-141">avanti</span><span class="sxs-lookup"><span data-stu-id="34f6a-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
