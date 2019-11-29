---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Uso del postback automatico con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574451"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="121d2-103">Uso del postback automatico con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="121d2-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>

<span data-ttu-id="121d2-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="121d2-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="121d2-105">[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="121d2-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="121d2-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="121d2-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="121d2-107">Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario).</span><span class="sxs-lookup"><span data-stu-id="121d2-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="121d2-108">Con alcuni codici JavaScript, questo effetto può essere evitato.</span><span class="sxs-lookup"><span data-stu-id="121d2-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="121d2-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="121d2-109">Overview</span></span>

<span data-ttu-id="121d2-110">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="121d2-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="121d2-111">Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Tuttavia, quando si usa il controllo CascadingDropDown, ASP. La funzionalità di PostBack automatico del controllo DropDownList di NET non funziona, poiché il caricamento asincrono dei dati nell'elenco genera un postback (non necessario).</span><span class="sxs-lookup"><span data-stu-id="121d2-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="121d2-112">Con alcuni codici JavaScript, questo effetto può essere evitato.</span><span class="sxs-lookup"><span data-stu-id="121d2-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="121d2-113">Passaggi</span><span class="sxs-lookup"><span data-stu-id="121d2-113">Steps</span></span>

<span data-ttu-id="121d2-114">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina, ma all'interno dell'elemento &lt;`form`&gt;:</span><span class="sxs-lookup"><span data-stu-id="121d2-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="121d2-115">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="121d2-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="121d2-116">Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown, che fornisce l'URL del servizio Web e le informazioni sul metodo:</span><span class="sxs-lookup"><span data-stu-id="121d2-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="121d2-117">Il dispositivo Extender CascadingDropDown chiama quindi in modo asincrono un servizio Web con la firma del metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="121d2-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="121d2-118">Il metodo restituisce una matrice di tipo valore CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="121d2-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="121d2-119">Il costruttore del tipo prevede prima la didascalia della voce dell'elenco e quindi il valore (attributo `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="121d2-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="121d2-120">Il caricamento della pagina nel browser compilerà l'elenco a discesa con tre fornitori, il secondo preselezionato.</span><span class="sxs-lookup"><span data-stu-id="121d2-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="121d2-121">Inoltre, ASP.NET definisce il metodo JavaScript `__doPostBack()`.</span><span class="sxs-lookup"><span data-stu-id="121d2-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="121d2-122">Una volta caricata la pagina, questa chiamata JavaScript viene aggiunta all'elenco a discesa, ma solo se sono presenti elementi.</span><span class="sxs-lookup"><span data-stu-id="121d2-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="121d2-123">Se nell'elenco non sono presenti elementi, il Toolkit di controllo li sta caricando, in modo che il codice JavaScript usi un timeout e tenti di nuovo tra mezzo secondo.</span><span class="sxs-lookup"><span data-stu-id="121d2-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="121d2-124">In questo modo, viene eseguito un postback solo quando sono effettivamente presenti elementi nell'elenco e l'utente seleziona una voce.</span><span class="sxs-lookup"><span data-stu-id="121d2-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="121d2-125">[![la selezione di un elemento elenco causa un postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="121d2-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="121d2-126">La selezione di un elemento di elenco causa un postback ([fare clic per visualizzare l'immagine con dimensioni complete](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="121d2-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="121d2-127">Precedente</span><span class="sxs-lookup"><span data-stu-id="121d2-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
