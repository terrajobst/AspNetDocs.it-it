---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Popolamento dinamico di un controllo (C#) | Microsoft Docs
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione su t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 24f88e44e0f878127314774d4e8846f80133413e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599266"
---
# <a name="dynamically-populating-a-control-c"></a><span data-ttu-id="6bf67-103">Popolamento dinamico di un controllo (C#)</span><span class="sxs-lookup"><span data-stu-id="6bf67-103">Dynamically Populating a Control (C#)</span></span>

<span data-ttu-id="6bf67-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6bf67-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6bf67-105">[Scarica codice](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6bf67-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="6bf67-106">Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="6bf67-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>

## <a name="overview"></a><span data-ttu-id="6bf67-107">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="6bf67-107">Overview</span></span>

<span data-ttu-id="6bf67-108">Il controllo `DynamicPopulate` in ASP.NET AJAX Control Toolkit chiama un servizio Web o un metodo di pagina e inserisce il valore risultante in un controllo di destinazione nella pagina, senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="6bf67-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="6bf67-109">Questa esercitazione illustra come impostare questa impostazione.</span><span class="sxs-lookup"><span data-stu-id="6bf67-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="6bf67-110">Passaggi</span><span class="sxs-lookup"><span data-stu-id="6bf67-110">Steps</span></span>

<span data-ttu-id="6bf67-111">Prima di tutto, è necessario un servizio Web ASP.NET che implementi il metodo che deve essere chiamato da `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="6bf67-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="6bf67-112">La classe del servizio Web richiede l'attributo `ScriptService` definito all'interno `Microsoft.Web.Script.Services`; in caso contrario, ASP.NET AJAX non è in grado di creare il proxy JavaScript sul lato client per il servizio Web, che a sua volta è richiesto da `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="6bf67-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="6bf67-113">Il metodo Web deve prevedere un argomento di tipo stringa, denominato `contextKey`, perché il controllo `DynamicPopulate` invia una parte delle informazioni di contesto a ogni chiamata del servizio Web.</span><span class="sxs-lookup"><span data-stu-id="6bf67-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="6bf67-114">Il servizio Web seguente restituisce la data corrente in un formato rappresentato dall'argomento `contextKey`:</span><span class="sxs-lookup"><span data-stu-id="6bf67-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="6bf67-115">Il servizio Web viene quindi salvato come `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="6bf67-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="6bf67-116">In alternativa, è possibile implementare il metodo `getDate()` come metodo di pagina all'interno della pagina ASP.NET effettiva con il controllo `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="6bf67-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="6bf67-117">Nel passaggio successivo creare un nuovo file ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6bf67-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="6bf67-118">Come sempre, il primo passaggio consiste nell'includere il `ScriptManager` nella pagina corrente per caricare la libreria ASP.NET AJAX e fare in modo che il Toolkit di controllo funzioni:</span><span class="sxs-lookup"><span data-stu-id="6bf67-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="6bf67-119">Quindi, aggiungere un controllo etichetta (ad esempio utilizzando il controllo HTML con lo stesso nome o il &lt;`asp:Label` /controllo Web &gt;) che in seguito visualizzerà il risultato della chiamata al servizio Web.</span><span class="sxs-lookup"><span data-stu-id="6bf67-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="6bf67-120">Un pulsante HTML (come controllo HTML, dal momento che non è necessario un postback al server) verrà usato per attivare il popolamento dinamico:</span><span class="sxs-lookup"><span data-stu-id="6bf67-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="6bf67-121">Infine, è necessario il controllo `DynamicPopulateExtender` per collegare le cose.</span><span class="sxs-lookup"><span data-stu-id="6bf67-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="6bf67-122">Verranno impostati i seguenti attributi (oltre a quelli più evidenti, `ID` e `runat`=`"server"`):</span><span class="sxs-lookup"><span data-stu-id="6bf67-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="6bf67-123">`TargetControlID` dove inserire il risultato dalla chiamata al servizio Web</span><span class="sxs-lookup"><span data-stu-id="6bf67-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="6bf67-124">`ServicePath` percorso del servizio Web (omettere se si desidera utilizzare un metodo di pagina)</span><span class="sxs-lookup"><span data-stu-id="6bf67-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="6bf67-125">nome `ServiceMethod` del metodo Web o del metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="6bf67-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="6bf67-126">`ContextKey` le informazioni sul contesto da inviare al servizio Web</span><span class="sxs-lookup"><span data-stu-id="6bf67-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="6bf67-127">`PopulateTriggerControlID` elemento che attiva la chiamata al servizio Web</span><span class="sxs-lookup"><span data-stu-id="6bf67-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="6bf67-128">`ClearContentsDuringUpdate` se svuotare l'elemento di destinazione durante la chiamata al servizio Web</span><span class="sxs-lookup"><span data-stu-id="6bf67-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="6bf67-129">Come si può notare, il controllo richiede alcune informazioni, ma l'inserimento di tutti gli elementi è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="6bf67-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="6bf67-130">Ecco il markup per il controllo `DynamicPopulateExtender` nello scenario corrente:</span><span class="sxs-lookup"><span data-stu-id="6bf67-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="6bf67-131">Eseguire la pagina ASP.NET nel browser e fare clic sul pulsante. si riceverà la data corrente nel formato mese-giorno-anno.</span><span class="sxs-lookup"><span data-stu-id="6bf67-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>

<span data-ttu-id="6bf67-132">[![un clic sul pulsante Recupera la data dal server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6bf67-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="6bf67-133">Facendo clic sul pulsante viene recuperata la data dal server ([fare clic per visualizzare l'immagine con dimensioni complete](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6bf67-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6bf67-134">avanti</span><span class="sxs-lookup"><span data-stu-id="6bf67-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
