---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendering dei siti Pagine Web ASP.NET (Razor) per i dispositivi mobili | Microsoft Docs
author: Rick-Anderson
description: 'Questo articolo descrive come creare pagine in un sito di Pagine Web ASP.NET (Razor) che eseguirà il rendering appropriato nei dispositivi mobili. Che cosa apprenderai: come te...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563567"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="46096-104">Rendering dei siti di Pagine Web ASP.NET (Razor) per i dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="46096-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="46096-105">di [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="46096-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="46096-106">Questo articolo descrive come creare pagine in un sito di Pagine Web ASP.NET (Razor) che eseguirà il rendering appropriato nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="46096-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="46096-107">Contenuto dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="46096-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="46096-108">Come usare una convenzione di denominazione per specificare che una pagina è progettata in modo specifico per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="46096-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="46096-109">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="46096-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="46096-110">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="46096-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="46096-111">Questa esercitazione funziona anche con Pagine Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="46096-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="46096-112">Pagine Web ASP.NET consente di creare visualizzazioni personalizzate per il rendering del contenuto in dispositivi mobili o in altri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="46096-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="46096-113">Il modo più semplice per creare una pagina specifica del dispositivo in un sito Pagine Web ASP.NET consiste nell'usare un modello di denominazione dei file simile al seguente: *filename. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46096-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="46096-114">È possibile creare due versioni di una pagina (ad esempio, una denominata *MyFile. cshtml* e una denominata *MyFile. mobile. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="46096-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="46096-115">In fase di esecuzione, quando un dispositivo mobile richiede *MyFile. cshtml*, ASP.NET esegue il rendering del contenuto da *MyFile. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46096-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="46096-116">In caso contrario, viene eseguito il rendering di *MyFile. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="46096-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="46096-117">Nell'esempio seguente viene illustrato come abilitare il rendering mobile aggiungendo una pagina di contenuto per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="46096-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="46096-118">*Pagina1. cshtml* contiene contenuto più una barra laterale di navigazione.</span><span class="sxs-lookup"><span data-stu-id="46096-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="46096-119">*Pagina1. mobile. cshtml* contiene lo stesso contenuto, ma omette l'intestazione laterale.</span><span class="sxs-lookup"><span data-stu-id="46096-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="46096-120">In un sito di Pagine Web ASP.NET creare un file denominato *Pagina1. cshtml* e sostituire il contenuto corrente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="46096-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="46096-121">Creare un file denominato *Pagina1. mobile. cshtml* e sostituire il contenuto esistente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="46096-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="46096-122">Si noti che la versione per dispositivi mobili della pagina omette la sezione di navigazione per un migliore rendering in una schermata più piccola.</span><span class="sxs-lookup"><span data-stu-id="46096-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="46096-123">Eseguire un browser desktop e passare a *Pagina1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46096-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="46096-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="46096-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="46096-125">Eseguire un browser per dispositivi mobili (o un emulatore di dispositivo mobile) e passare a *Pagina1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46096-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="46096-126">Si noti che non è incluso *. mobile.*</span><span class="sxs-lookup"><span data-stu-id="46096-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="46096-127">come parte dell'URL. Anche se la richiesta è a *Pagina1. cshtml*, ASP.NET esegue il rendering di *Pagina1. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46096-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="46096-129">Per testare le pagine per dispositivi mobili, è possibile usare un simulatore di dispositivi mobili che viene eseguito in un computer desktop.</span><span class="sxs-lookup"><span data-stu-id="46096-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="46096-130">Questo strumento consente di testare le pagine Web in modo analogo ai dispositivi mobili, ovvero in genere con un'area di visualizzazione molto più piccola.</span><span class="sxs-lookup"><span data-stu-id="46096-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="46096-131">Un esempio di simulatore è il [componente aggiuntivo User Agent Switcher](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare vari browser per dispositivi mobili da una versione desktop di Firefox.</span><span class="sxs-lookup"><span data-stu-id="46096-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="46096-132">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="46096-132">Additional Resources</span></span>

<span data-ttu-id="46096-133">[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="46096-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
