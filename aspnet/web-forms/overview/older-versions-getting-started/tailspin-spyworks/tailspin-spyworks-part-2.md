---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: livello di accesso ai dati | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella seconda parte viene illustrata l'aggiunta del livello di accesso ai dati.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573248"
---
# <a name="part-2-data-access-layer"></a><span data-ttu-id="df765-104">Parte 2: livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="df765-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="df765-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="df765-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="df765-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="df765-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="df765-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="df765-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="df765-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="df765-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="df765-109">Nella seconda parte viene illustrata l'aggiunta del livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="df765-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a><span data-ttu-id="df765-110">Aggiunta del livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="df765-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="df765-111">L'applicazione di eCommerce dipenderà da due database.</span><span class="sxs-lookup"><span data-stu-id="df765-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="df765-112">Per informazioni sul cliente verrà usato il database di appartenenza ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="df765-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="df765-113">Per il carrello acquisti e il catalogo dei prodotti verrà implementato un database di SQL Express come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="df765-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="df765-114">Dopo aver creato il database (Commerce. MDF) nella cartella app\_data dell'applicazione, è possibile procedere con la creazione del livello di accesso ai dati usando il Entity Framework .NET.</span><span class="sxs-lookup"><span data-stu-id="df765-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="df765-115">Verrà creata una cartella denominata "data\_Access", facendo clic con il pulsante destro del mouse sulla cartella e scegliendo "Aggiungi nuovo elemento".</span><span class="sxs-lookup"><span data-stu-id="df765-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="df765-116">Nell'elemento "modelli installati", quindi selezionare "ADO.NET Entity Data Model" immettere EDM\_Commerce. edmx come nome e fare clic sul pulsante "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="df765-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="df765-117">Scegliere "genera da database".</span><span class="sxs-lookup"><span data-stu-id="df765-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="df765-118">Salvare e compilare.</span><span class="sxs-lookup"><span data-stu-id="df765-118">Save and build.</span></span>

<span data-ttu-id="df765-119">A questo punto è possibile aggiungere la prima funzionalità, ovvero un menu di categoria di prodotti.</span><span class="sxs-lookup"><span data-stu-id="df765-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df765-120">[Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="df765-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
