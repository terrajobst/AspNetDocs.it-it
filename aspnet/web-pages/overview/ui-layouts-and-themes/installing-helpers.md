---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installazione di un helper in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive come installare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un helper è un componente riutilizzabile che include codice e markup a per...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638586"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6af92-104">Installazione di un helper in un sito di Pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="6af92-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="6af92-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6af92-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6af92-106">Questo articolo descrive come installare un helper in un sito Web di Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="6af92-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="6af92-107">Un *Helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che può essere noiosa o complessa.</span><span class="sxs-lookup"><span data-stu-id="6af92-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="6af92-108">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="6af92-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6af92-109">Come installare un helper in un sito Web creato con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="6af92-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6af92-110">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6af92-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6af92-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6af92-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="6af92-112">Panoramica degli helper</span><span class="sxs-lookup"><span data-stu-id="6af92-112">Overview of Helpers</span></span>

<span data-ttu-id="6af92-113">Alcune attività che spesso gli utenti desiderano eseguire sulle pagine Web richiedono una grande quantità di codice o richiedono una conoscenza aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="6af92-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="6af92-114">Gli esempi includono la visualizzazione di un grafico per i dati; inserimento di un pulsante "follow" di Twitter in una pagina; invio di messaggi di posta elettronica dal sito Web; ritaglio o ridimensionamento delle immagini; utilizzando PayPal per il sito.</span><span class="sxs-lookup"><span data-stu-id="6af92-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="6af92-115">Per semplificare l'esecuzione di questi tipi di operazioni, Pagine Web ASP.NET consente di utilizzare gli *Helper*.</span><span class="sxs-lookup"><span data-stu-id="6af92-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="6af92-116">Gli helper sono componenti da installare per un sito e che consentono di eseguire attività tipiche usando solo una riga o due di codice Razor.</span><span class="sxs-lookup"><span data-stu-id="6af92-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="6af92-117">Pagine Web ASP.NET dispone di alcuni Helper incorporati.</span><span class="sxs-lookup"><span data-stu-id="6af92-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="6af92-118">Tuttavia, molti helper sono disponibili nei pacchetti (componenti aggiuntivi) forniti tramite Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="6af92-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="6af92-119">NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.</span><span class="sxs-lookup"><span data-stu-id="6af92-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="6af92-120">Installazione di un helper in WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6af92-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="6af92-121">In WebMatrix 3 fare clic sul pulsante **NuGet** .</span><span class="sxs-lookup"><span data-stu-id="6af92-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Finestra di dialogo raccolta NuGet in WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="6af92-123">Viene avviato Gestione pacchetti NuGet e vengono visualizzati i pacchetti disponibili.</span><span class="sxs-lookup"><span data-stu-id="6af92-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="6af92-124">Nella casella di ricerca immettere una parola chiave per l'helper che si vuole installare.</span><span class="sxs-lookup"><span data-stu-id="6af92-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Finestra di dialogo raccolta NuGet in WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="6af92-126">Selezionare il pacchetto e quindi fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="6af92-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="6af92-127">Fare clic su **Sì** quando viene chiesto se si desidera installare il pacchetto e indicare di accettare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="6af92-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="6af92-128">Se è la prima volta che si installa un helper, NuGet crea le cartelle nel sito Web per il codice che costituisce l'helper.</span><span class="sxs-lookup"><span data-stu-id="6af92-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="6af92-129">Per disinstallare un helper, fare clic sul pulsante **raccolta** , fare clic sulla scheda **installato** e selezionare il pacchetto che si desidera disinstallare.</span><span class="sxs-lookup"><span data-stu-id="6af92-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="6af92-130">Installazione dell'helper Twitter</span><span class="sxs-lookup"><span data-stu-id="6af92-130">Installing the Twitter helper</span></span>

<span data-ttu-id="6af92-131">La versione più recente dell'API Twitter non è compatibile con l'helper di Twitter che viene installato tramite NuGet.</span><span class="sxs-lookup"><span data-stu-id="6af92-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="6af92-132">Per informazioni su come configurare l'helper di Twitter nel progetto, vedere l'argomento [Helper di Twitter con WebMatrix](twitter-helper.md) .</span><span class="sxs-lookup"><span data-stu-id="6af92-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6af92-133">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6af92-133">Additional Resources</span></span>

[<span data-ttu-id="6af92-134">Introduzione a Pagine Web ASP.NET 2-Nozioni fondamentali sulla programmazione</span><span class="sxs-lookup"><span data-stu-id="6af92-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="6af92-135">Helper di Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6af92-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
