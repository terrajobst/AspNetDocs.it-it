---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: appartenenza a ASP.NET | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 6 aggiunge l'appartenenza a ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564183"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="5f20d-104">Parte 6: appartenenza a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5f20d-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="5f20d-105">di [Joe Stagner spiega](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5f20d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5f20d-106">In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="5f20d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5f20d-107">Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="5f20d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5f20d-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="5f20d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5f20d-109">La parte 6 aggiunge l'appartenenza a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5f20d-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="5f20d-110">Uso dell'appartenenza a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5f20d-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="5f20d-111">Fare clic su sicurezza</span><span class="sxs-lookup"><span data-stu-id="5f20d-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="5f20d-112">Assicurarsi di usare l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="5f20d-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="5f20d-113">Usare il collegamento "Crea utente" per creare un paio di utenti.</span><span class="sxs-lookup"><span data-stu-id="5f20d-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="5f20d-114">Al termine, fare riferimento alla finestra Esplora soluzioni e aggiornare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="5f20d-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="5f20d-115">Si noti che ASPNETDB. La fine di MDF è stata creata.</span><span class="sxs-lookup"><span data-stu-id="5f20d-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="5f20d-116">Questo file contiene le tabelle per supportare i servizi ASP.NET di base come l'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="5f20d-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="5f20d-117">A questo punto è possibile iniziare a implementare il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="5f20d-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="5f20d-118">Per iniziare, creare una pagina CheckOut. aspx.</span><span class="sxs-lookup"><span data-stu-id="5f20d-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="5f20d-119">La pagina CheckOut. aspx dovrebbe essere disponibile solo per gli utenti che hanno eseguito l'accesso, in modo da limitare l'accesso agli utenti connessi e reindirizzare gli utenti che non sono connessi alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="5f20d-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="5f20d-120">A tale scopo, aggiungere quanto segue alla sezione di configurazione del file Web. config.</span><span class="sxs-lookup"><span data-stu-id="5f20d-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="5f20d-121">Il modello per le applicazioni Web Form ASP.NET aggiunge automaticamente una sezione di autenticazione al file Web. config e stabilisce la pagina di accesso predefinita.</span><span class="sxs-lookup"><span data-stu-id="5f20d-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="5f20d-122">È necessario modificare il file code-behind login. aspx per eseguire la migrazione di un carrello acquisti anonimo quando l'utente esegue l'accesso.</span><span class="sxs-lookup"><span data-stu-id="5f20d-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="5f20d-123">Modificare la pagina\_evento Load come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="5f20d-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="5f20d-124">Aggiungere quindi un gestore dell'evento "login" come questo per impostare il nome della sessione sull'utente appena connesso e modificare l'ID sessione temporanea nel carrello acquisti con quello dell'utente chiamando il metodo MigrateCart nella classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5f20d-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="5f20d-125">(Implementato nel file con estensione CS)</span><span class="sxs-lookup"><span data-stu-id="5f20d-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="5f20d-126">Implementare il metodo MigrateCart () come questo.</span><span class="sxs-lookup"><span data-stu-id="5f20d-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="5f20d-127">In checkout. aspx verrà usato un oggetto EntityDataSource e un GridView nella pagina di estrazione, così come nella pagina del carrello della spesa.</span><span class="sxs-lookup"><span data-stu-id="5f20d-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="5f20d-128">Si noti che il controllo GridView specifica un gestore eventi "OnDataBound" denominato elenco\_RowDataBound, quindi si implementerà questo gestore eventi come questo.</span><span class="sxs-lookup"><span data-stu-id="5f20d-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="5f20d-129">Questo metodo mantiene un totale parziale del carrello acquisti quando ogni riga è associata e aggiorna la riga inferiore di GridView.</span><span class="sxs-lookup"><span data-stu-id="5f20d-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="5f20d-130">In questa fase è stata implementata una presentazione di "revisione" dell'ordine da inserire.</span><span class="sxs-lookup"><span data-stu-id="5f20d-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="5f20d-131">Per gestire uno scenario di carrello vuoto, è possibile aggiungere alcune righe di codice alla pagina\_evento Load:</span><span class="sxs-lookup"><span data-stu-id="5f20d-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="5f20d-132">Quando l'utente fa clic sul pulsante "Submit", verrà eseguito il codice seguente nel gestore dell'evento click del pulsante Submit.</span><span class="sxs-lookup"><span data-stu-id="5f20d-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="5f20d-133">La "carne" del processo di invio dell'ordine deve essere implementata nel metodo SubmitOrder () della classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5f20d-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="5f20d-134">SubmitOrder:</span><span class="sxs-lookup"><span data-stu-id="5f20d-134">SubmitOrder will:</span></span>

- <span data-ttu-id="5f20d-135">Prendere tutte le voci del carrello acquisti e usarle per creare un nuovo record di ordine e i record OrderDetails associati.</span><span class="sxs-lookup"><span data-stu-id="5f20d-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="5f20d-136">Calcolare la data di spedizione.</span><span class="sxs-lookup"><span data-stu-id="5f20d-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="5f20d-137">Cancellare il carrello della spesa.</span><span class="sxs-lookup"><span data-stu-id="5f20d-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="5f20d-138">Ai fini di questa applicazione di esempio, verrà calcolata una data di spedizione semplicemente aggiungendo due giorni alla data corrente.</span><span class="sxs-lookup"><span data-stu-id="5f20d-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="5f20d-139">L'esecuzione dell'applicazione consente ora di testare il processo di acquisto dall'inizio alla fine.</span><span class="sxs-lookup"><span data-stu-id="5f20d-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f20d-140">[Precedente](tailspin-spyworks-part-5.md)
> [Successivo](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="5f20d-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
