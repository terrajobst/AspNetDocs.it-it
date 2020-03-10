---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Preimpostazione delle voci dell'elenco conC#CascadingDropDown () | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bb4a51092534e6fddbd40f868c53c58d12eef2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597902"
---
# <a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="0f2f1-103">Preimpostazione delle voci dell'elenco con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="0f2f1-103">Presetting List Entries with CascadingDropDown (C#)</span></span>

<span data-ttu-id="0f2f1-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0f2f1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0f2f1-105">[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0f2f1-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="0f2f1-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0f2f1-107">Con un po' di codice è possibile che un elemento elenco venga preselezionato dopo che i dati sono stati caricati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="0f2f1-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0f2f1-108">Overview</span></span>

<span data-ttu-id="0f2f1-109">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0f2f1-110">Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Con un po' di codice è possibile che un elemento elenco venga preselezionato dopo che i dati sono stati caricati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="0f2f1-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0f2f1-111">Steps</span></span>

<span data-ttu-id="0f2f1-112">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="0f2f1-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="0f2f1-113">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="0f2f1-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="0f2f1-114">Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown, che fornisce l'URL del servizio Web e le informazioni sul metodo:</span><span class="sxs-lookup"><span data-stu-id="0f2f1-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="0f2f1-115">Il dispositivo Extender CascadingDropDown chiama quindi in modo asincrono un servizio Web con la firma del metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="0f2f1-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="0f2f1-116">Il metodo restituisce una matrice di tipo valore CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="0f2f1-117">Il costruttore del tipo prevede prima la didascalia della voce dell'elenco e quindi il valore (attributo `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="0f2f1-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="0f2f1-118">Se il terzo argomento è impostato su true, l'elemento elenco viene selezionato automaticamente nel browser.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="0f2f1-119">Il caricamento della pagina nel browser compilerà l'elenco a discesa con tre fornitori, il secondo preselezionato.</span><span class="sxs-lookup"><span data-stu-id="0f2f1-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="0f2f1-120">[![l'elenco viene compilato e preselezionato automaticamente](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0f2f1-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="0f2f1-121">L'elenco viene compilato e preselezionato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0f2f1-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f2f1-122">[Precedente](using-cascadingdropdown-with-a-database-cs.md)
> [Successivo](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0f2f1-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
