---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione all'esercitazione su NerdDinner | Microsoft Docs
author: shanselman
description: Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso. Questa esercitazione illustra come creare un'applicazione ridotta, ma completa, usando ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580577"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="ba879-104">Introduzione all'esercitazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ba879-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="ba879-105">di [Scott hanseln](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ba879-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="ba879-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="ba879-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="ba879-107">Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso.</span><span class="sxs-lookup"><span data-stu-id="ba879-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="ba879-108">Questa esercitazione illustra come creare un'applicazione di piccole dimensioni, ma completa, usando ASP.NET MVC 1 e introduce alcuni concetti di base.</span><span class="sxs-lookup"><span data-stu-id="ba879-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="ba879-109">Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="ba879-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="ba879-110">Esercitazione su NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ba879-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="ba879-111">Il modo migliore per apprendere un nuovo Framework consiste nel creare qualcosa con esso.</span><span class="sxs-lookup"><span data-stu-id="ba879-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="ba879-112">Questa esercitazione illustra come creare un'applicazione di piccole dimensioni, ma completa, usando ASP.NET MVC e introduce alcuni concetti di base.</span><span class="sxs-lookup"><span data-stu-id="ba879-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="ba879-113">L'applicazione che verrà compilata è denominata "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="ba879-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="ba879-114">NerdDinner consente agli utenti di trovare e organizzare in modo semplice le cene online:</span><span class="sxs-lookup"><span data-stu-id="ba879-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="ba879-115">NerdDinner consente agli utenti registrati di creare, modificare ed eliminare cene.</span><span class="sxs-lookup"><span data-stu-id="ba879-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="ba879-116">Viene applicato un set coerente di regole business e di convalida nell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="ba879-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="ba879-117">I visitatori possono usare una mappa basata su AJAX per cercare le cene imminenti che vengono mantenute nelle vicinanze:</span><span class="sxs-lookup"><span data-stu-id="ba879-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="ba879-118">Quando si fa clic su una cena, viene visualizzata una pagina dei dettagli in cui sono disponibili altre informazioni:</span><span class="sxs-lookup"><span data-stu-id="ba879-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="ba879-119">Se sono interessati a partecipare alla cena, possono eseguire l'accesso o la registrazione al sito:</span><span class="sxs-lookup"><span data-stu-id="ba879-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="ba879-120">Possono quindi fare clic su un collegamento RSVP basato su AJAX per partecipare all'evento:</span><span class="sxs-lookup"><span data-stu-id="ba879-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="ba879-121">Implementazione di NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ba879-121">Implementing NerdDinner</span></span>

<span data-ttu-id="ba879-122">Verrà avviata l'applicazione NerdDinner usando il comando file-&gt;nuovo progetto in Visual Studio per creare un nuovo progetto MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ba879-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="ba879-123">Si aggiungeranno quindi le funzionalità e le funzionalità in modo incrementale.</span><span class="sxs-lookup"><span data-stu-id="ba879-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="ba879-124">Lungo il percorso, verranno illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ba879-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="ba879-125">Come creare un nuovo progetto MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba879-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="ba879-126">Come creare un database</span><span class="sxs-lookup"><span data-stu-id="ba879-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="ba879-127">Come compilare un modello con le convalide delle regole business</span><span class="sxs-lookup"><span data-stu-id="ba879-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="ba879-128">Come usare i controller e le visualizzazioni per implementare un'interfaccia utente di elenco/dettagli</span><span class="sxs-lookup"><span data-stu-id="ba879-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="ba879-129">Come fornire supporto per l'immissione dei moduli dati CRUD (creazione, lettura, aggiornamento, eliminazione)</span><span class="sxs-lookup"><span data-stu-id="ba879-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="ba879-130">Come usare ViewData e implementare classi ViewModel</span><span class="sxs-lookup"><span data-stu-id="ba879-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="ba879-131">Come riutilizzare l'interfaccia utente utilizzando le pagine master e i parziali</span><span class="sxs-lookup"><span data-stu-id="ba879-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="ba879-132">Come implementare il paging dei dati efficiente</span><span class="sxs-lookup"><span data-stu-id="ba879-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="ba879-133">Come proteggere le applicazioni usando l'autenticazione e l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ba879-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="ba879-134">Come utilizzare AJAX per distribuire aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="ba879-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="ba879-135">Come utilizzare AJAX per implementare scenari di mapping</span><span class="sxs-lookup"><span data-stu-id="ba879-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="ba879-136">Come abilitare il testing unità automatizzato</span><span class="sxs-lookup"><span data-stu-id="ba879-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="ba879-137">È possibile creare una copia personalizzata di NerdDinner da zero completando ogni passaggio illustrato in questo capitolo.</span><span class="sxs-lookup"><span data-stu-id="ba879-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="ba879-138">In alternativa, è possibile scaricare una versione completa del codice sorgente qui: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="ba879-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="ba879-139">Facoltativamente, è anche possibile [scaricare una versione in formato PDF gratuita di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione offline.</span><span class="sxs-lookup"><span data-stu-id="ba879-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="ba879-140">Per compilare l'applicazione, è possibile usare Visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="ba879-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="ba879-141">È possibile utilizzare SQL Server o la SQL Server Express gratuita per il database.</span><span class="sxs-lookup"><span data-stu-id="ba879-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="ba879-142">È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (tutti gratuiti) usando V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="ba879-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="ba879-143">Ora è possibile iniziare....</span><span class="sxs-lookup"><span data-stu-id="ba879-143">Now let's get started....</span></span>

<span data-ttu-id="ba879-144">Ora che è stato analizzato il NerdDinner, è possibile eseguire il rollup delle maniche e scrivere del codice.</span><span class="sxs-lookup"><span data-stu-id="ba879-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="ba879-145">Per creare l'applicazione NerdDinner, si inizierà a usare il nuovo progetto&gt;file in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba879-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ba879-146">avanti</span><span class="sxs-lookup"><span data-stu-id="ba879-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
