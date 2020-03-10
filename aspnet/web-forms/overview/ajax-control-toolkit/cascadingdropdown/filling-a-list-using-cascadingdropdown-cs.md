---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Compilazione di un elenco tramite CascadingDropDownC#() | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614212"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="7ae89-103">Compilazione di un elenco tramite CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="7ae89-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="7ae89-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7ae89-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7ae89-105">[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7ae89-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="7ae89-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="7ae89-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="7ae89-107">Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.</span><span class="sxs-lookup"><span data-stu-id="7ae89-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="7ae89-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7ae89-108">Overview</span></span>

<span data-ttu-id="7ae89-109">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="7ae89-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="7ae89-110">Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Il primo problema da risolvere è quello di compilare effettivamente un elenco a discesa usando questo controllo.</span><span class="sxs-lookup"><span data-stu-id="7ae89-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="7ae89-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="7ae89-111">Steps</span></span>

<span data-ttu-id="7ae89-112">Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="7ae89-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="7ae89-113">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="7ae89-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="7ae89-114">Per questo elenco viene aggiunto un dispositivo Extender CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="7ae89-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="7ae89-115">Invierà una richiesta asincrona a un servizio Web che restituirà un elenco di voci da visualizzare nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="7ae89-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="7ae89-116">Per il corretto funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:</span><span class="sxs-lookup"><span data-stu-id="7ae89-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="7ae89-117">`ServicePath`: URL di un servizio Web che fornisce le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="7ae89-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="7ae89-118">`ServiceMethod`: metodo Web che fornisce le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="7ae89-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="7ae89-119">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="7ae89-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="7ae89-120">`Category`: informazioni sulla categoria inviate al metodo Web quando viene chiamata</span><span class="sxs-lookup"><span data-stu-id="7ae89-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="7ae89-121">`PromptText`: testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server</span><span class="sxs-lookup"><span data-stu-id="7ae89-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="7ae89-122">Ecco il markup per l'elemento `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="7ae89-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="7ae89-123">L'unica differenza tra C# e VB è il nome del servizio Web associato:</span><span class="sxs-lookup"><span data-stu-id="7ae89-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="7ae89-124">Il codice JavaScript proveniente dal `CascadingDropDown` Extender chiama un metodo del servizio Web con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="7ae89-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="7ae89-125">Quindi, l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definita da ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="7ae89-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="7ae89-126">Nel costruttore di `CascadingDropDownNameValue`, è necessario specificare prima il testo della voce dell'elenco e quindi il relativo valore, così come `<option value="VALUE">NAME</option>` funzionerebbe in HTML.</span><span class="sxs-lookup"><span data-stu-id="7ae89-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="7ae89-127">Ecco alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="7ae89-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="7ae89-128">Il caricamento della pagina nel browser attiverà l'elenco per compilarlo con tre fornitori.</span><span class="sxs-lookup"><span data-stu-id="7ae89-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="7ae89-129">[![l'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7ae89-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="7ae89-130">L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7ae89-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7ae89-131">avanti</span><span class="sxs-lookup"><span data-stu-id="7ae89-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
