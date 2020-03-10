---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Uso del postback automatico con CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535952"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="c3b2f-103">Uso del postback automatico con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="c3b2f-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>

<span data-ttu-id="c3b2f-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c3b2f-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c3b2f-105">[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3b2f-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="c3b2f-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c3b2f-107">Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario).</span><span class="sxs-lookup"><span data-stu-id="c3b2f-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c3b2f-108">Con alcuni codici JavaScript, questo effetto può essere evitato.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="c3b2f-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c3b2f-109">Overview</span></span>

<span data-ttu-id="c3b2f-110">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c3b2f-111">Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario).</span><span class="sxs-lookup"><span data-stu-id="c3b2f-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c3b2f-112">Con alcuni codici JavaScript, questo effetto può essere evitato.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="c3b2f-113">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c3b2f-113">Steps</span></span>

<span data-ttu-id="c3b2f-114">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina, ma all'interno dell'elemento &lt;`form`&gt;:</span><span class="sxs-lookup"><span data-stu-id="c3b2f-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="c3b2f-115">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="c3b2f-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="c3b2f-116">Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown, che fornisce l'URL del servizio Web e le informazioni sul metodo:</span><span class="sxs-lookup"><span data-stu-id="c3b2f-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="c3b2f-117">Il dispositivo Extender CascadingDropDown chiama quindi in modo asincrono un servizio Web con la firma del metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="c3b2f-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="c3b2f-118">Il metodo restituisce una matrice di tipo valore CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="c3b2f-119">Il costruttore del tipo prevede prima la didascalia della voce dell'elenco e quindi il valore (attributo `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="c3b2f-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="c3b2f-120">Il caricamento della pagina nel browser compilerà l'elenco a discesa con tre fornitori, il secondo preselezionato.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="c3b2f-121">Inoltre, ASP.NET definisce il metodo JavaScript `__doPostBack()`.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="c3b2f-122">Una volta caricata la pagina, questa chiamata JavaScript viene aggiunta all'elenco a discesa, ma solo se sono presenti elementi.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="c3b2f-123">Se nell'elenco non sono presenti elementi, il Toolkit di controllo li sta caricando, in modo che il codice JavaScript usi un timeout e tenti di nuovo tra mezzo secondo.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="c3b2f-124">In questo modo, viene eseguito un postback solo quando sono effettivamente presenti elementi nell'elenco e l'utente seleziona una voce.</span><span class="sxs-lookup"><span data-stu-id="c3b2f-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="c3b2f-125">[![la selezione di un elemento elenco causa un postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3b2f-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="c3b2f-126">La selezione di un elemento di elenco causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c3b2f-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3b2f-127">[Precedente](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Successivo](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c3b2f-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
