---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Uso di DynamicPopulate con un controllo utente e JavaScriptC#() | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613708"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="c7caa-103">Uso di DynamicPopulate con un controllo utente e JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="c7caa-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>

<span data-ttu-id="c7caa-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c7caa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c7caa-105">[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7caa-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="c7caa-106">Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="c7caa-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c7caa-107">È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c7caa-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="c7caa-108">Tuttavia è necessario prestare particolare attenzione quando il dispositivo Extender risiede in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="c7caa-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="c7caa-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c7caa-109">Overview</span></span>

<span data-ttu-id="c7caa-110">Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="c7caa-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c7caa-111">È anche possibile attivare il popolamento usando codice JavaScript personalizzato sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c7caa-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="c7caa-112">Tuttavia è necessario prestare particolare attenzione quando il dispositivo Extender risiede in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="c7caa-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="c7caa-113">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c7caa-113">Steps</span></span>

<span data-ttu-id="c7caa-114">Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato dal controllo `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="c7caa-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="c7caa-115">Il servizio Web implementa il metodo `getDate()` che prevede un argomento di tipo stringa, denominato `contextKey`, poiché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web.</span><span class="sxs-lookup"><span data-stu-id="c7caa-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="c7caa-116">Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7caa-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="c7caa-117">Nel passaggio successivo, creare un nuovo controllo utente (file`.ascx`), indicato dalla seguente dichiarazione nella prima riga:</span><span class="sxs-lookup"><span data-stu-id="c7caa-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="c7caa-118">Viene utilizzato un elemento &lt;`label`&gt; per visualizzare i dati provenienti dal server.</span><span class="sxs-lookup"><span data-stu-id="c7caa-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="c7caa-119">Nel file di controllo utente, inoltre, si utilizzeranno tre pulsanti di opzione, ognuno dei quali rappresenta uno dei tre formati di data possibili supportati dal servizio Web.</span><span class="sxs-lookup"><span data-stu-id="c7caa-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="c7caa-120">Quando l'utente fa clic su uno dei pulsanti di opzione, il browser eseguirà codice JavaScript simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c7caa-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="c7caa-121">Questo codice accede al `DynamicPopulateExtender` (non preoccuparsi ancora dell'ID strano, questo verrà trattato in un secondo momento) e attiverà il popolamento dinamico con i dati.</span><span class="sxs-lookup"><span data-stu-id="c7caa-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="c7caa-122">Nel contesto del pulsante di opzione corrente, `this.value` fa riferimento al relativo valore `format1`, `format2` o `format3` esattamente ciò che il metodo Web prevede.</span><span class="sxs-lookup"><span data-stu-id="c7caa-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="c7caa-123">L'unico elemento mancante nel controllo utente è ancora il controllo `DynamicPopulateExtender` che collega i pulsanti di opzione al servizio Web.</span><span class="sxs-lookup"><span data-stu-id="c7caa-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="c7caa-124">Anche in questo caso, è possibile notare l'ID strano utilizzato nel controllo: `mcd1$myDate` anziché `myDate`.</span><span class="sxs-lookup"><span data-stu-id="c7caa-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="c7caa-125">In precedenza, il codice JavaScript usava `mcd1_dpe1` per accedere al `DynamicPopulateExtender` invece che `dpe1`. Questa strategia di denominazione è un requisito speciale quando si utilizza `DynamicPopulateExtender` all'interno di un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="c7caa-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="c7caa-126">Inoltre, è necessario incorporare il controllo utente in un modo specifico per eseguire tutto il lavoro.</span><span class="sxs-lookup"><span data-stu-id="c7caa-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="c7caa-127">Creare una nuova pagina ASP.NET e registrare un prefisso tag per il controllo utente appena implementato:</span><span class="sxs-lookup"><span data-stu-id="c7caa-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="c7caa-128">Includere quindi il controllo `ScriptManager` AJAX ASP.NET nella nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="c7caa-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="c7caa-129">Infine, aggiungere il controllo utente alla pagina.</span><span class="sxs-lookup"><span data-stu-id="c7caa-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="c7caa-130">È necessario impostare solo il relativo attributo `ID` (e `runat="server"`naturalmente), ma è necessario impostarlo anche su un nome specifico: `mcd1` poiché questo è il prefisso utilizzato all'interno del controllo utente per accedervi utilizzando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c7caa-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="c7caa-131">L'operazione è ora completata.</span><span class="sxs-lookup"><span data-stu-id="c7caa-131">And that's it!</span></span> <span data-ttu-id="c7caa-132">Il comportamento della pagina è quello previsto: un utente fa clic su uno dei pulsanti di opzione, il controllo nel Toolkit chiama il servizio Web e visualizza la data corrente nel formato desiderato.</span><span class="sxs-lookup"><span data-stu-id="c7caa-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="c7caa-133">[![i pulsanti di opzione si trovano in un controllo utente](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7caa-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="c7caa-134">I pulsanti di opzione si trovano in un controllo utente ([fare clic per visualizzare l'immagine con dimensioni complete](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c7caa-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7caa-135">[Precedente](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Successivo](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c7caa-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
