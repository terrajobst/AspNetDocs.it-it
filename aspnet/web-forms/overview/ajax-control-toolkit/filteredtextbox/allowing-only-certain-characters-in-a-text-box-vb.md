---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Consentire solo determinati caratteri in una casella di testo (VB) | Microsoft Docs
author: wenz
description: I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri. Tuttavia, ciò non impedisce agli utenti di digitare non valido...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573960"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="17674-104">Autorizzazione solo di caratteri specifici in una casella di testo (VB)</span><span class="sxs-lookup"><span data-stu-id="17674-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>

<span data-ttu-id="17674-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="17674-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="17674-106">[Scarica codice](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="17674-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="17674-107">I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri.</span><span class="sxs-lookup"><span data-stu-id="17674-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="17674-108">Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="17674-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="17674-109">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="17674-109">Overview</span></span>

<span data-ttu-id="17674-110">I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri.</span><span class="sxs-lookup"><span data-stu-id="17674-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="17674-111">Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="17674-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="17674-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="17674-112">Steps</span></span>

<span data-ttu-id="17674-113">ASP.NET AJAX Control Toolkit contiene il controllo `FilteredTextBox` che estende una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="17674-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="17674-114">Una volta attivato, è possibile immettere nel campo solo un determinato set di caratteri.</span><span class="sxs-lookup"><span data-stu-id="17674-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="17674-115">Per eseguire questa operazione, è necessario prima di tutto il `ScriptManager` AJAX ASP.NET che carica le librerie JavaScript utilizzate anche da ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="17674-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="17674-116">Quindi, è necessaria una casella di testo:</span><span class="sxs-lookup"><span data-stu-id="17674-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="17674-117">Infine, il controllo `FilteredTextBoxExtender` si occupa della limitazione dei caratteri che l'utente può digitare.</span><span class="sxs-lookup"><span data-stu-id="17674-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="17674-118">Per prima cosa, impostare l'attributo `TargetControlID` sul `ID` del controllo `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="17674-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="17674-119">Scegliere quindi uno dei valori `FilterType` disponibili:</span><span class="sxs-lookup"><span data-stu-id="17674-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="17674-120">`Custom` predefinita; è necessario fornire un elenco di caratteri validi</span><span class="sxs-lookup"><span data-stu-id="17674-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="17674-121">`LowercaseLetters` solo lettere minuscole</span><span class="sxs-lookup"><span data-stu-id="17674-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="17674-122">solo cifre `Numbers`</span><span class="sxs-lookup"><span data-stu-id="17674-122">`Numbers` digits only</span></span>
- <span data-ttu-id="17674-123">`UppercaseLetters` solo lettere maiuscole</span><span class="sxs-lookup"><span data-stu-id="17674-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="17674-124">Se viene utilizzata la `Custom FilterType`, è necessario impostare la proprietà `ValidChars` e fornire un elenco di caratteri che possono essere digitati.</span><span class="sxs-lookup"><span data-stu-id="17674-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="17674-125">Per il modo: se si tenta di incollare il testo nella casella di testo, vengono rimossi tutti i caratteri non validi.</span><span class="sxs-lookup"><span data-stu-id="17674-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="17674-126">Di seguito è riportato il markup per il controllo `FilteredTextBoxExtender` che consente solo cifre (un elemento che sarebbe stato possibile anche con `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="17674-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="17674-127">Eseguire la pagina e provare a immettere una lettera se JavaScript è abilitato e non funzionerà. le cifre vengono tuttavia visualizzate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="17674-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="17674-128">Si noti tuttavia che la protezione `FilteredTextBox` non è a prova di punta: se JavaScript è abilitato, tutti i dati possono essere immessi nella casella di testo, quindi è necessario usare altri metodi di convalida, ad esempio ASP. Controlli di convalida di NET.</span><span class="sxs-lookup"><span data-stu-id="17674-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="17674-129">[è possibile immettere solo le cifre ![](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17674-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="17674-130">È possibile immettere solo cifre ([fare clic per visualizzare l'immagine con dimensioni complete](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="17674-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="17674-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="17674-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
