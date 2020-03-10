---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: pagine finali, gestione delle eccezioni e conclusione | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Parte 8 aggiunge una pagina di contatto, informazioni sulla pagina e sull'eccezione...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586884"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="e8af8-104">Parte 8: pagine finali, gestione delle eccezioni e conclusione</span><span class="sxs-lookup"><span data-stu-id="e8af8-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="e8af8-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e8af8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e8af8-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="e8af8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e8af8-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="e8af8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e8af8-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="e8af8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e8af8-109">La parte 8 aggiunge una pagina di contatto, informazioni sulla pagina e la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="e8af8-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="e8af8-110">Questa è la conclusione della serie.</span><span class="sxs-lookup"><span data-stu-id="e8af8-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="e8af8-111">Pagina contatto (invio di messaggi di posta elettronica da ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e8af8-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="e8af8-112">Creare una nuova pagina denominata contactus. aspx</span><span class="sxs-lookup"><span data-stu-id="e8af8-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="e8af8-113">Utilizzando la finestra di progettazione, creare il seguente form prendendo nota speciale per includere ToolkitScriptManager e il controllo editor da AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="e8af8-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="e8af8-114">.</span><span class="sxs-lookup"><span data-stu-id="e8af8-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="e8af8-115">Fare doppio clic sul pulsante "Invia" per generare un gestore dell'evento click nel file code-behind e implementare un metodo per inviare le informazioni di contatto come un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e8af8-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="e8af8-116">Questo codice richiede che il file Web. config includa una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio della posta.</span><span class="sxs-lookup"><span data-stu-id="e8af8-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="e8af8-117">Pagina informazioni</span><span class="sxs-lookup"><span data-stu-id="e8af8-117">About Page</span></span>

<span data-ttu-id="e8af8-118">Crea una pagina denominata aboutus. aspx e Aggiungi il contenuto che preferisci.</span><span class="sxs-lookup"><span data-stu-id="e8af8-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="e8af8-119">Gestore eccezioni globale</span><span class="sxs-lookup"><span data-stu-id="e8af8-119">Global Exception Handler</span></span>

<span data-ttu-id="e8af8-120">Infine, nell'applicazione sono state generate eccezioni e si verificano circostanze non previste che a freddo provocano anche eccezioni non gestite nell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="e8af8-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="e8af8-121">Non si vuole che venga visualizzata un'eccezione non gestita in un visitatore del sito Web.</span><span class="sxs-lookup"><span data-stu-id="e8af8-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="e8af8-122">Oltre a essere un'esperienza utente terribile, le eccezioni non gestite possono anche costituire un problema di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e8af8-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="e8af8-123">Per risolvere questo problema, verrà implementato un gestore di eccezioni globale.</span><span class="sxs-lookup"><span data-stu-id="e8af8-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="e8af8-124">A tale scopo, aprire il file Global. asax e prendere nota del seguente gestore eventi generato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e8af8-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="e8af8-125">Aggiungere il codice per implementare l'applicazione\_gestore errori come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e8af8-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="e8af8-126">Aggiungere quindi una pagina denominata Error. aspx alla soluzione e aggiungere il frammento di markup.</span><span class="sxs-lookup"><span data-stu-id="e8af8-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="e8af8-127">A questo punto, nella pagina\_carica gestore eventi estrarre i messaggi di errore dall'oggetto richiesta.</span><span class="sxs-lookup"><span data-stu-id="e8af8-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="e8af8-128">Conclusione</span><span class="sxs-lookup"><span data-stu-id="e8af8-128">Conclusion</span></span>

<span data-ttu-id="e8af8-129">Abbiamo visto che WebForm ASP.NET semplifica la creazione di un sito Web sofisticato con accesso al database, appartenenza, AJAX e così via.</span><span class="sxs-lookup"><span data-stu-id="e8af8-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="e8af8-130">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="e8af8-130">pretty quickly.</span></span>

<span data-ttu-id="e8af8-131">Speriamo che in questa esercitazione siano stati forniti gli strumenti necessari per iniziare a creare applicazioni Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8af8-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e8af8-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="e8af8-132">Previous</span></span>](tailspin-spyworks-part-7.md)
