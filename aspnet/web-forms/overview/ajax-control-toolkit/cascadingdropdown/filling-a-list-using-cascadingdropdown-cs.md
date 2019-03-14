---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Compilazione di un elenco tramite CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 795b2a2ee80d3d2bed6f752887d5f9d974151a56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040038"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="b04e2-103">Compilazione di un elenco tramite CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="b04e2-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="b04e2-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b04e2-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b04e2-105">[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b04e2-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="b04e2-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="b04e2-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b04e2-107">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="b04e2-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="b04e2-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b04e2-108">Overview</span></span>

<span data-ttu-id="b04e2-109">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="b04e2-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b04e2-110">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="b04e2-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="b04e2-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="b04e2-111">Steps</span></span>

<span data-ttu-id="b04e2-112">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="b04e2-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="b04e2-113">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="b04e2-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="b04e2-114">Per questo elenco, viene aggiunto un controllo extender CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="b04e2-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="b04e2-115">Invia una richiesta asincrona a un servizio web che quindi restituirà un elenco di voci da visualizzare nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="b04e2-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="b04e2-116">Per il corretto funzionamento, è necessario impostare gli attributi di CascadingDropDown seguenti:</span><span class="sxs-lookup"><span data-stu-id="b04e2-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="b04e2-117">`ServicePath`: URL di un servizio web distribuisce le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="b04e2-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="b04e2-118">`ServiceMethod`: Metodo Web le voci dell'elenco di distribuzione</span><span class="sxs-lookup"><span data-stu-id="b04e2-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="b04e2-119">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="b04e2-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="b04e2-120">`Category`: Informazioni sulle categorie in cui viene inviati al metodo web quando viene chiamato</span><span class="sxs-lookup"><span data-stu-id="b04e2-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="b04e2-121">`PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server</span><span class="sxs-lookup"><span data-stu-id="b04e2-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="b04e2-122">Ecco il markup per il `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="b04e2-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="b04e2-123">L'unica differenza tra c# e Visual Basic è il nome del servizio web associato:</span><span class="sxs-lookup"><span data-stu-id="b04e2-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="b04e2-124">Il codice JavaScript provenienti dal `CascadingDropDown` extender chiama un metodo del servizio web con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="b04e2-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="b04e2-125">In modo che l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="b04e2-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="b04e2-126">Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, proprio come `<option value="VALUE">NAME</option>` verrebbero impiegati in HTML.</span><span class="sxs-lookup"><span data-stu-id="b04e2-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="b04e2-127">Ecco alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="b04e2-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="b04e2-128">Il caricamento della pagina nel browser verrà attivata l'elenco deve essere compilato con tre fornitori.</span><span class="sxs-lookup"><span data-stu-id="b04e2-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="b04e2-129">[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b04e2-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="b04e2-130">L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b04e2-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b04e2-131">avanti</span><span class="sxs-lookup"><span data-stu-id="b04e2-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
