---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Consentire solo determinati caratteri in una casella di testoC#() | Microsoft Docs
author: wenz
description: I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri. Tuttavia, ciò non impedisce agli utenti di digitare non valido...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d1e367becd574e31d24fca8545f76b1ed3c4d85e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554236"
---
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="bd68b-104">Autorizzazione solo di caratteri specifici in una casella di testo (C#)</span><span class="sxs-lookup"><span data-stu-id="bd68b-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>

<span data-ttu-id="bd68b-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd68b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd68b-106">[Scarica codice](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd68b-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="bd68b-107">I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri.</span><span class="sxs-lookup"><span data-stu-id="bd68b-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="bd68b-108">Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="bd68b-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="bd68b-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bd68b-109">Overview</span></span>

<span data-ttu-id="bd68b-110">I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri.</span><span class="sxs-lookup"><span data-stu-id="bd68b-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="bd68b-111">Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="bd68b-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="bd68b-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="bd68b-112">Steps</span></span>

<span data-ttu-id="bd68b-113">ASP.NET AJAX Control Toolkit contiene il controllo `FilteredTextBox` che estende una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="bd68b-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="bd68b-114">Una volta attivato, è possibile immettere nel campo solo un determinato set di caratteri.</span><span class="sxs-lookup"><span data-stu-id="bd68b-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="bd68b-115">Per eseguire questa operazione, è necessario prima di tutto il `ScriptManager` AJAX ASP.NET che carica le librerie JavaScript utilizzate anche da ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="bd68b-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="bd68b-116">Quindi, è necessaria una casella di testo:</span><span class="sxs-lookup"><span data-stu-id="bd68b-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="bd68b-117">Infine, il controllo `FilteredTextBoxExtender` si occupa della limitazione dei caratteri che l'utente può digitare.</span><span class="sxs-lookup"><span data-stu-id="bd68b-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="bd68b-118">Per prima cosa, impostare l'attributo `TargetControlID` sul `ID` del controllo `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="bd68b-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="bd68b-119">Scegliere quindi uno dei valori `FilterType` disponibili:</span><span class="sxs-lookup"><span data-stu-id="bd68b-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="bd68b-120">`Custom` predefinita; è necessario fornire un elenco di caratteri validi</span><span class="sxs-lookup"><span data-stu-id="bd68b-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="bd68b-121">`LowercaseLetters` solo lettere minuscole</span><span class="sxs-lookup"><span data-stu-id="bd68b-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="bd68b-122">solo cifre `Numbers`</span><span class="sxs-lookup"><span data-stu-id="bd68b-122">`Numbers` digits only</span></span>
- <span data-ttu-id="bd68b-123">`UppercaseLetters` solo lettere maiuscole</span><span class="sxs-lookup"><span data-stu-id="bd68b-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="bd68b-124">Se viene utilizzata la `Custom FilterType`, è necessario impostare la proprietà `ValidChars` e fornire un elenco di caratteri che possono essere digitati.</span><span class="sxs-lookup"><span data-stu-id="bd68b-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="bd68b-125">Per il modo: se si tenta di incollare il testo nella casella di testo, vengono rimossi tutti i caratteri non validi.</span><span class="sxs-lookup"><span data-stu-id="bd68b-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="bd68b-126">Di seguito è riportato il markup per il controllo `FilteredTextBoxExtender` che consente solo cifre (un elemento che sarebbe stato possibile anche con `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="bd68b-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="bd68b-127">Eseguire la pagina e provare a immettere una lettera se JavaScript è abilitato e non funzionerà. le cifre vengono tuttavia visualizzate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="bd68b-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="bd68b-128">Si noti tuttavia che la protezione `FilteredTextBox` non è a prova di punta: se JavaScript è abilitato, tutti i dati possono essere immessi nella casella di testo, quindi è necessario usare altri metodi di convalida, ad esempio ASP. Controlli di convalida di NET.</span><span class="sxs-lookup"><span data-stu-id="bd68b-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="bd68b-129">[è possibile immettere solo le cifre ![](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd68b-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="bd68b-130">È possibile immettere solo cifre ([fare clic per visualizzare l'immagine con dimensioni complete](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd68b-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bd68b-131">avanti</span><span class="sxs-lookup"><span data-stu-id="bd68b-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
