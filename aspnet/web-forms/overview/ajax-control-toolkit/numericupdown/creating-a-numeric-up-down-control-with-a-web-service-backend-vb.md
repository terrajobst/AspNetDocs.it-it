---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Creazione di un controllo su/giù numerico con un back-end del servizio Web (VB) | Microsoft Docs
author: wenz
description: Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe rivelarsi più c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606345"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="ea471-103">Creazione di un controllo su/giù numerico con un back-end del servizio Web (VB)</span><span class="sxs-lookup"><span data-stu-id="ea471-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>

<span data-ttu-id="ea471-104">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ea471-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ea471-105">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ea471-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="ea471-106">Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe risultare più comodo.</span><span class="sxs-lookup"><span data-stu-id="ea471-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ea471-107">Per impostazione predefinita, il controllo NumericUpDown aumenta o diminuisce sempre un valore di 1, ma un servizio Web dimostra maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="ea471-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="ea471-108">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="ea471-108">Overview</span></span>

<span data-ttu-id="ea471-109">Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe risultare più comodo.</span><span class="sxs-lookup"><span data-stu-id="ea471-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ea471-110">Per impostazione predefinita, il controllo `NumericUpDown` aumenta o diminuisce sempre un valore di 1, ma un servizio Web dimostra maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="ea471-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="ea471-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="ea471-111">Steps</span></span>

<span data-ttu-id="ea471-112">ASP.NET AJAX Control Toolkit contiene il `NumericUpDown` Extender che aggiunge automaticamente due pulsanti a una casella di testo: uno per l'aumento del valore, uno per la riduzione.</span><span class="sxs-lookup"><span data-stu-id="ea471-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="ea471-113">Tuttavia, il controllo supporta anche una chiamata al servizio Web (o chiamata al metodo della pagina).</span><span class="sxs-lookup"><span data-stu-id="ea471-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="ea471-114">Ogni volta che si fa clic sul pulsante su o giù, il codice JavaScript si connette al server Web ed esegue un metodo.</span><span class="sxs-lookup"><span data-stu-id="ea471-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="ea471-115">La firma del metodo è la seguente:</span><span class="sxs-lookup"><span data-stu-id="ea471-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="ea471-116">L'argomento `current` è il valore corrente nella casella di testo. l'attributo `tag` è costituito da dati di contesto aggiuntivi che possono essere impostati come proprietà del `NumericUpDown` Extender (ma non obbligatorio).</span><span class="sxs-lookup"><span data-stu-id="ea471-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="ea471-117">Per questo esempio, il controllo su/giù numerico deve consentire solo valori con potenze di due: 1, 2, 4, 8, 16, 32, 64 e così via.</span><span class="sxs-lookup"><span data-stu-id="ea471-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="ea471-118">Pertanto, il metodo eseguito quando l'utente desidera aumentare il valore deve raddoppiare il valore precedente; l'altro metodo deve dividere il valore per due.</span><span class="sxs-lookup"><span data-stu-id="ea471-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="ea471-119">Ecco il servizio Web completo:</span><span class="sxs-lookup"><span data-stu-id="ea471-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="ea471-120">Infine, creare una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ea471-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="ea471-121">Come di consueto, è necessario un controllo `ScriptManager`, un controllo `TextBox` e un controllo `NumericUpDownExtender`.</span><span class="sxs-lookup"><span data-stu-id="ea471-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="ea471-122">Per quest'ultimo, è necessario fornire le informazioni sul servizio Web:</span><span class="sxs-lookup"><span data-stu-id="ea471-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="ea471-123">nome `ServiceDownMethod` del metodo Web o del metodo di pagina inattivo</span><span class="sxs-lookup"><span data-stu-id="ea471-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="ea471-124">`ServiceDownPath` percorso del servizio Web con il metodo di servizio inattivo; omettere se si usa un metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="ea471-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="ea471-125">nome `ServiceUpMethod` del metodo Web o della pagina</span><span class="sxs-lookup"><span data-stu-id="ea471-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="ea471-126">`ServiceUpPath` percorso del servizio Web con il metodo up Service; omettere se si usa un metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="ea471-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="ea471-127">Ecco il markup completo per la pagina:</span><span class="sxs-lookup"><span data-stu-id="ea471-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="ea471-128">Se si esegue la pagina, si noterà che il valore nella casella di testo viene sempre raddoppiato quando si fa clic sul pulsante superiore e viene dimezzato quando si fa clic sul pulsante inferiore.</span><span class="sxs-lookup"><span data-stu-id="ea471-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="ea471-129">[![vengono visualizzati solo i numeri che sono una potenza di 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ea471-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="ea471-130">Vengono visualizzati solo i numeri che sono una potenza di 2 ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ea471-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ea471-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="ea471-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
