---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Associazione dati a una fisarmonicaC#() | Microsoft Docs
author: wenz
description: Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614555"
---
# <a name="databinding-to-an-accordion-c"></a><span data-ttu-id="0e023-104">Data binding a un controllo Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="0e023-104">Databinding to an Accordion (C#)</span></span>

<span data-ttu-id="0e023-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0e023-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0e023-106">[Scarica codice](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0e023-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="0e023-107">Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="0e023-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="0e023-108">I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il binding a un'origine dati offre maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="0e023-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="0e023-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0e023-109">Overview</span></span>

<span data-ttu-id="0e023-110">Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="0e023-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="0e023-111">I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il binding a un'origine dati offre maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="0e023-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="0e023-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0e023-112">Steps</span></span>

<span data-ttu-id="0e023-113">Prima di tutto, è necessaria un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="0e023-113">First of all, a data source is required.</span></span> <span data-ttu-id="0e023-114">In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0e023-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="0e023-115">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0e023-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0e023-116">Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="0e023-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="0e023-117">Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.</span><span class="sxs-lookup"><span data-stu-id="0e023-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="0e023-118">Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0e023-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0e023-119">Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="0e023-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0e023-120">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="0e023-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="0e023-121">Quindi, aggiungere un'origine dati alla pagina.</span><span class="sxs-lookup"><span data-stu-id="0e023-121">Then, add a data source to the page.</span></span> <span data-ttu-id="0e023-122">Per utilizzare una quantità limitata di dati, è possibile selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="0e023-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="0e023-123">Se si utilizza l'Assistente di Visual Studio per creare l'origine dati, tenere presente che un bug nella versione corrente non prevede il prefisso del nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="0e023-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="0e023-124">Il markup seguente mostra la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="0e023-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="0e023-125">Ricordare il nome (ID) dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="0e023-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="0e023-126">Questa identificazione deve quindi essere usata nella proprietà `DataSourceID` del controllo di Accordion:</span><span class="sxs-lookup"><span data-stu-id="0e023-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="0e023-127">All'interno del controllo di Accordion è possibile fornire modelli per varie parti del controllo, tra cui l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="0e023-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="0e023-128">All'interno di questi elementi, è sufficiente restituire i dati dall'origine dati, usando il metodo `DataBinder.Eval()`:</span><span class="sxs-lookup"><span data-stu-id="0e023-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="0e023-129">Quando la pagina viene caricata, l'origine dati deve essere associata alla fisarmonica con il codice sul lato server:</span><span class="sxs-lookup"><span data-stu-id="0e023-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="0e023-130">Per concludere questo esempio, è necessario definire le due classi CSS a cui viene fatto riferimento nel controllo della fisarmonica (nelle proprietà `HeaderCssClass` e `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="0e023-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="0e023-131">Inserire il markup seguente nella sezione `<head>` della pagina:</span><span class="sxs-lookup"><span data-stu-id="0e023-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

<span data-ttu-id="0e023-132">[![i dati della fisarmonica provengono direttamente dall'origine dati](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0e023-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="0e023-133">I dati della fisarmonica provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine con dimensioni complete](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0e023-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0e023-134">avanti</span><span class="sxs-lookup"><span data-stu-id="0e023-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)
