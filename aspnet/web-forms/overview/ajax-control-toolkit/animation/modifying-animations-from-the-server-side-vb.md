---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modifica delle animazioni dal lato server (VB) | Microsoft Docs
author: wenz
description: Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo. Anche le animazioni possono...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575216"
---
# <a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="e3e42-104">Modifica delle animazioni dal lato server (VB)</span><span class="sxs-lookup"><span data-stu-id="e3e42-104">Modifying Animations From The Server Side (VB)</span></span>

<span data-ttu-id="e3e42-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e3e42-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e3e42-106">[Scarica codice](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e3e42-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="e3e42-107">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="e3e42-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e3e42-108">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="e3e42-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="e3e42-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="e3e42-109">Overview</span></span>

<span data-ttu-id="e3e42-110">Il controllo dell'animazione in ASP.NET AJAX Control Toolkit non è solo un controllo, ma un intero Framework per aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="e3e42-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e3e42-111">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="e3e42-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="e3e42-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="e3e42-112">Steps</span></span>

<span data-ttu-id="e3e42-113">Per prima cosa, includere il `ScriptManager` nella pagina; quindi, viene caricata la libreria ASP.NET AJAX, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="e3e42-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="e3e42-114">L'animazione verrà applicata a un pannello di testo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e3e42-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="e3e42-115">Nella classe CSS associata per il pannello definire un colore di sfondo gradevole e impostare anche una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="e3e42-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="e3e42-116">Il resto del codice viene eseguito sul lato server e non usa il markup; USA invece il codice per creare il controllo `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="e3e42-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="e3e42-117">Tuttavia, il Toolkit di controllo non fornisce attualmente l'accesso all'API per creare le singole animazioni.</span><span class="sxs-lookup"><span data-stu-id="e3e42-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="e3e42-118">È tuttavia possibile impostare la proprietà Animations di `AnimationExtender`su una stringa contenente il markup XML utilizzato durante l'assegnazione delle animazioni in modo dichiarativo.</span><span class="sxs-lookup"><span data-stu-id="e3e42-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="e3e42-119">Per creare il codice XML che non deve contenere l'elemento `<Animations>` è possibile utilizzare il supporto XML .NET Framework o, come nel codice seguente, fornire solo la stringa:</span><span class="sxs-lookup"><span data-stu-id="e3e42-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="e3e42-120">Infine, aggiungere il controllo `AnimationExtender` alla pagina corrente, all'interno dell'elemento `<form runat="server">`, verificando che l'animazione sia inclusa ed esegua:</span><span class="sxs-lookup"><span data-stu-id="e3e42-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

<span data-ttu-id="e3e42-121">[![l'animazione viene creata utilizzando il codice/VB C#lato server](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e3e42-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="e3e42-122">L'animazione viene creata usando il codice/VB C#lato server ([fare clic per visualizzare l'immagine con dimensioni complete](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e3e42-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3e42-123">[Precedente](triggering-an-animation-in-another-control-vb.md)
> [Successivo](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e3e42-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
