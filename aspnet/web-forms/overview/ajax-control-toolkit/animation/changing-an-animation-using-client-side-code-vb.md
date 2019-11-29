---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Modifica di un'animazione tramite codice lato client (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. L'animazione può anche...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cce0a5a901f71edd40eada59ac7eeba93222e2b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606942"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="7152d-104">Modifica di un'animazione tramite codice lato client (VB)</span><span class="sxs-lookup"><span data-stu-id="7152d-104">Changing an Animation Using Client-Side Code (VB)</span></span>

<span data-ttu-id="7152d-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7152d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7152d-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7152d-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="7152d-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="7152d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7152d-108">È inoltre possibile modificare l'animazione utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7152d-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="7152d-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="7152d-109">Overview</span></span>

<span data-ttu-id="7152d-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="7152d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7152d-111">È inoltre possibile modificare l'animazione utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7152d-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7152d-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="7152d-112">Steps</span></span>

<span data-ttu-id="7152d-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="7152d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="7152d-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7152d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="7152d-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="7152d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="7152d-116">L'animazione effettiva viene avviata da un pulsante HTML:</span><span class="sxs-lookup"><span data-stu-id="7152d-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="7152d-117">Aggiungere quindi il `AnimationExtender` alla pagina, fornendo un `ID`, l'attributo `TargetControlID` e il `runat="server"`obbligatorio:</span><span class="sxs-lookup"><span data-stu-id="7152d-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="7152d-118">Si noti che non è presente alcun nodo `<Animations>` all'interno del controllo `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7152d-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="7152d-119">Il codice JavaScript personalizzato viene usato per fornire le animazioni da usare con il controllo.</span><span class="sxs-lookup"><span data-stu-id="7152d-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="7152d-120">Come per l'API server di `AnimationExtender`, non esiste ancora un modo semplice per assegnare un'animazione al dispositivo Extender.</span><span class="sxs-lookup"><span data-stu-id="7152d-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="7152d-121">Tuttavia, il dispositivo Extender espone diversi metodi per leggere e scrivere animazioni registrate con i vari eventi (`OnClick`, `OnLoad`e così via).</span><span class="sxs-lookup"><span data-stu-id="7152d-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="7152d-122">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="7152d-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="7152d-123">Il formato del valore restituito delle funzioni `get_*()` e il formato dell'argomento per le funzioni `set_*()` è una stringa JSON che fornisce una rappresentazione dell'oggetto del markup XML.</span><span class="sxs-lookup"><span data-stu-id="7152d-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="7152d-124">Attualmente, non esiste alcun modo per passare un oggetto in, ma è possibile leggere un oggetto da un'animazione specificata (`get_OnXXXBehavior()` metodi).</span><span class="sxs-lookup"><span data-stu-id="7152d-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="7152d-125">Ecco una stringa JSON (senza le virgolette di delimitazione e formattata in modo corretto) che rappresenta un'animazione attivata dal pulsante, ma animando il pannello mediante il ridimensionamento e la dissolvenza allo stesso tempo:</span><span class="sxs-lookup"><span data-stu-id="7152d-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="7152d-126">Il codice JavaScript seguente assegna questo descrittore JSON all'animazione `OnClick` del dispositivo Extender corrente e lo esegue:</span><span class="sxs-lookup"><span data-stu-id="7152d-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]

<span data-ttu-id="7152d-127">[![l'animazione viene eseguita immediatamente, senza un clic del mouse (e con un markup molto ridotto)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7152d-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="7152d-128">L'animazione viene eseguita immediatamente, senza un clic del mouse (e con un markup molto breve) ([fare clic per visualizzare l'immagine con dimensioni complete](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7152d-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7152d-129">[Precedente](executing-animations-using-client-side-code-vb.md)
> [Successivo](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7152d-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
