---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aggiornamento di un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive come eseguire l'aggiornamento manualmente e con una procedura guidata un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2. Questo documento è disponibile anche per d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637018"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="ef443-104">Aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="ef443-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>

> <span data-ttu-id="ef443-105">Questo documento descrive come eseguire l'aggiornamento manualmente e con una procedura guidata un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ef443-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="ef443-106">Questo documento è disponibile anche per il [download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="ef443-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>

## <a name="introduction"></a><span data-ttu-id="ef443-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="ef443-107">Introduction</span></span>

<span data-ttu-id="ef443-108">ASP.NET MVC 2 può essere installato side-by-side con ASP.NET MVC 1,0 nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="ef443-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="ef443-109">Questo consente agli sviluppatori di applicazioni di scegliere quando eseguire l'aggiornamento di un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ef443-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="ef443-110">Visual Studio 2010 include una procedura guidata che consente di aggiornare i progetti ASP.NET MVC 1,0 esistenti compilati con Visual Studio 2008 a ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ef443-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="ef443-111">L'aggiornamento guidato viene avviato aprendo un progetto ASP.NET MVC 1,0 in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ef443-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="ef443-112">Aggiornamento guidato per ASP.NET MVC 1,0 in Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ef443-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="ef443-113">Per aggiornare un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2 in Visual Studio 2008 SP1, usare l'applicazione MvcAppConverter (non supportata).</span><span class="sxs-lookup"><span data-stu-id="ef443-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="ef443-114">È possibile scaricare l'applicazione dall'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="ef443-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="ef443-115">Aggiornamento manuale di un progetto MVC 1,0 di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef443-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="ef443-116">Per aggiornare manualmente un'applicazione ASP.NET MVC 1,0 esistente alla versione 2, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ef443-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="ef443-117">Eseguire un backup del progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="ef443-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="ef443-118">In un editor di testo aprire il file di progetto (il file con estensione csproj o vbproj) e trovare l'elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="ef443-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="ef443-119">Come valore di tale elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="ef443-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="ef443-120">Al termine, il valore dell'elemento deve essere il seguente:</span><span class="sxs-lookup"><span data-stu-id="ef443-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="ef443-121">Nella cartella radice dell'applicazione Web modificare il file Web. config.</span><span class="sxs-lookup"><span data-stu-id="ef443-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="ef443-122">Cercare System. Web. MVC, Version = 1.0.0.0 e sostituire tutte le istanze con System. Web. MVC, Version = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="ef443-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="ef443-123">Ripetere il passaggio precedente per il file Web. config che si trova nella cartella views.</span><span class="sxs-lookup"><span data-stu-id="ef443-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="ef443-124">Aprire il progetto con Visual Studio e in **Esplora soluzioni**espandere il nodo **riferimenti** .</span><span class="sxs-lookup"><span data-stu-id="ef443-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="ef443-125">Eliminare il riferimento a System. Web. MVC, che fa riferimento all'assembly della versione 1,0.</span><span class="sxs-lookup"><span data-stu-id="ef443-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="ef443-126">Aggiungere un riferimento a System. Web. Mvc (v 2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="ef443-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="ef443-127">Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione nella sezione configuraton:</span><span class="sxs-lookup"><span data-stu-id="ef443-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="ef443-128">Creare una nuova applicazione MVC 2 ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="ef443-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="ef443-129">Copiare i file dalla cartella Scripts della nuova applicazione nella cartella Scripts dell'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="ef443-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="ef443-130">Aggiornare il file CSS ApplicationA™ s esistente con le definizioni di stile CSS nel file site. CSS.</span><span class="sxs-lookup"><span data-stu-id="ef443-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="ef443-131">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="ef443-131">Compile the application and run it.</span></span> <span data-ttu-id="ef443-132">Se si verificano errori, vedere la sezione relativa alle modifiche di rilievo nella pagina Novità di [ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .</span><span class="sxs-lookup"><span data-stu-id="ef443-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
