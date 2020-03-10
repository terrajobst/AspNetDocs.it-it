---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Popolamento dinamico di un controllo tramite codice JavaScript (C#) | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535742"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="68bec-103">Popolamento dinamico di un controllo tramite codice JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="68bec-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>

<span data-ttu-id="68bec-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68bec-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68bec-105">[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="68bec-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="68bec-106">Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="68bec-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="68bec-107">È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.</span><span class="sxs-lookup"><span data-stu-id="68bec-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="68bec-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="68bec-108">Overview</span></span>

<span data-ttu-id="68bec-109">Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="68bec-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="68bec-110">È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.</span><span class="sxs-lookup"><span data-stu-id="68bec-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="68bec-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="68bec-111">Steps</span></span>

<span data-ttu-id="68bec-112">Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato dal controllo `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="68bec-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="68bec-113">Il servizio Web implementa il metodo `getDate()` che prevede un argomento di tipo stringa, denominato `contextKey`, poiché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web.</span><span class="sxs-lookup"><span data-stu-id="68bec-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="68bec-114">Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="68bec-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="68bec-115">Nel passaggio successivo, creare un nuovo sito di ASP.NET e iniziare con il controllo ASP.NET AJAX ScriptManager:</span><span class="sxs-lookup"><span data-stu-id="68bec-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="68bec-116">Quindi, aggiungere un controllo etichetta (ad esempio utilizzando il controllo HTML con lo stesso nome o il controllo Web `<asp:Label />`) che in seguito visualizzerà il risultato della chiamata al servizio Web.</span><span class="sxs-lookup"><span data-stu-id="68bec-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="68bec-117">Includere quindi un controllo `DynamicPopulateExtender` e fornire informazioni sul servizio Web, il controllo di destinazione, ma non il nome del controllo che attiva il popolamento. questa operazione verrà eseguita in un secondo momento usando JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="68bec-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="68bec-118">A questo punto, la parte relativa a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="68bec-118">Now to the JavaScript part.</span></span> <span data-ttu-id="68bec-119">La funzione `$find()`, definita dalla libreria ASP.NET AJAX, restituisce un riferimento a oggetti lato server di ASP.NET AJAX Control Toolkit, ad esempio `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="68bec-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="68bec-120">Nel file corrente `$find("dpe")` restituisce un riferimento a un controllo `DynamicPopulateExtender` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="68bec-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="68bec-121">Espone un metodo denominato `populate()` che attiva il processo di popolamento dinamico.</span><span class="sxs-lookup"><span data-stu-id="68bec-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="68bec-122">Il metodo `populate()` richiede un argomento: la chiave di contesto che fungerà da argomento per il metodo Web `getDate()`.</span><span class="sxs-lookup"><span data-stu-id="68bec-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="68bec-123">Quindi, ad esempio, `$find("dpe").populate("format1")` compilerà l'etichetta con la data corrente nel formato mese-giorno-anno.</span><span class="sxs-lookup"><span data-stu-id="68bec-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="68bec-124">Per rendere l'esempio leggermente più flessibile, l'utente può ora scegliere tra diversi formati di data.</span><span class="sxs-lookup"><span data-stu-id="68bec-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="68bec-125">Per ognuno di essi, viene visualizzato un pulsante di opzione.</span><span class="sxs-lookup"><span data-stu-id="68bec-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="68bec-126">Quando l'utente fa clic su un pulsante di opzione, il codice JavaScript popola in modo dinamico l'etichetta con il formato di data selezionato.</span><span class="sxs-lookup"><span data-stu-id="68bec-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="68bec-127">Ecco i pulsanti di opzione:</span><span class="sxs-lookup"><span data-stu-id="68bec-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="68bec-128">Si noti che all'interno del contesto di un pulsante di opzione, l'espressione JavaScript `this.value` si riferisce al valore del pulsante corrente, che è esattamente le stesse informazioni che il metodo `getDate()` può utilizzare.</span><span class="sxs-lookup"><span data-stu-id="68bec-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="68bec-129">[![facendo clic sul pulsante viene recuperata la data dal server, nel formato specificato](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68bec-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="68bec-130">Facendo clic sul pulsante viene recuperata la data dal server, nel formato specificato ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68bec-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="68bec-131">[Precedente](dynamically-populating-a-control-cs.md)
> [Successivo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="68bec-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
