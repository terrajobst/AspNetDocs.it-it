---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilizzo di TextBoxWatermark con i controlliC#di convalida () | Microsoft Docs
author: wenz
description: Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577840"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="fa5f8-104">Uso di TextBoxWatermark con i controlli di convalida (C#)</span><span class="sxs-lookup"><span data-stu-id="fa5f8-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="fa5f8-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fa5f8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fa5f8-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fa5f8-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="fa5f8-107">Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5f8-108">Quando un utente fa clic sulla casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5f8-109">Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5f8-110">Questo potrebbe essere in conflitto con i controlli di convalida di ASP.NET nella stessa pagina, ma questi problemi possono essere superati.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="fa5f8-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fa5f8-111">Overview</span></span>

<span data-ttu-id="fa5f8-112">Il controllo `TextBoxWatermark` in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5f8-113">Quando un utente fa clic sulla casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5f8-114">Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5f8-115">Questo potrebbe essere in conflitto con i controlli di convalida di ASP.NET nella stessa pagina, ma questi problemi possono essere superati.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="fa5f8-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="fa5f8-116">Steps</span></span>

<span data-ttu-id="fa5f8-117">La configurazione di base dell'esempio è la seguente: un controllo `TextBox` è filigranato usando un controllo `TextBoxWatermarkExtender`.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="fa5f8-118">Un pulsante attiva un postback e verrà usato in un secondo momento per attivare i controlli di convalida nella pagina.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="fa5f8-119">Inoltre, è necessario un controllo `ScriptManager` per inizializzare ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="fa5f8-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="fa5f8-120">A questo punto, aggiungere un controllo `RequiredFieldValidator` che controlla se è presente un testo nel campo quando viene inviato il modulo.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="fa5f8-121">È necessario impostare la proprietà `InitialValue` del validator sullo stesso valore usato nel controllo `TextBoxWatermarkExtender`: quando il modulo viene inviato, il valore di una casella di testo non modificato è il valore della filigrana al suo interno:</span><span class="sxs-lookup"><span data-stu-id="fa5f8-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="fa5f8-122">Tuttavia, esiste un problema con questo approccio: se il client Disabilita JavaScript, il campo di testo non viene precompilato con il testo della filigrana, quindi la `RequiredFieldValidator` non attiva un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="fa5f8-123">Pertanto, è necessario un secondo controllo `RequiredFieldValidator` che controlla la presenza di una casella di testo vuota (omettendo l'attributo `InitialValue`).</span><span class="sxs-lookup"><span data-stu-id="fa5f8-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="fa5f8-124">Poiché entrambi i validator utilizzano `Display`=`"Dynamic"`, l'utente finale non può distinguere dall'aspetto visivo quale dei due validator è stato generato; sembra che sia presente solo uno di essi.</span><span class="sxs-lookup"><span data-stu-id="fa5f8-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="fa5f8-125">Infine, aggiungere parte del codice sul lato server per restituire il testo nel campo se nessun validator ha emesso un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="fa5f8-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="fa5f8-126">[![il validator indica che non è presente alcun testo nel campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fa5f8-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="fa5f8-127">Il validator indica che non è presente alcun testo nel campo ([fare clic per visualizzare l'immagine con dimensioni complete](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fa5f8-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa5f8-128">[Precedente](using-textboxwatermark-in-a-formview-cs.md)
> [Successivo](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fa5f8-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
