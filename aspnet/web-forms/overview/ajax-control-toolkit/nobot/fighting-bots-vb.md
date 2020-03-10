---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Fighting bot (VB) | Microsoft Docs
author: wenz
description: I bot automatici intonano i weblog e altri siti Web con posta indesiderata, inviando moduli di commento senza alcuna interazione dell'utente Il controllo NoBot in ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627393"
---
# <a name="fighting-bots-vb"></a><span data-ttu-id="57d38-104">Lotta contro i bot (VB)</span><span class="sxs-lookup"><span data-stu-id="57d38-104">Fighting Bots (VB)</span></span>

<span data-ttu-id="57d38-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57d38-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57d38-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="57d38-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="57d38-107">I bot automatici intonano i weblog e altri siti Web con posta indesiderata, inviando moduli di commento senza alcuna interazione dell'utente</span><span class="sxs-lookup"><span data-stu-id="57d38-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="57d38-108">Il controllo NoBot in ASP.NET AJAX Control Toolkit può aiutare a combattere questi bot.</span><span class="sxs-lookup"><span data-stu-id="57d38-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="57d38-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="57d38-109">Overview</span></span>

<span data-ttu-id="57d38-110">I bot automatici intonano i weblog e altri siti Web con posta indesiderata, inviando moduli di commento senza alcuna interazione dell'utente</span><span class="sxs-lookup"><span data-stu-id="57d38-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="57d38-111">Il controllo NoBot in ASP.NET AJAX Control Toolkit può aiutare a combattere questi bot.</span><span class="sxs-lookup"><span data-stu-id="57d38-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="57d38-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="57d38-112">Steps</span></span>

<span data-ttu-id="57d38-113">Un approccio comune per sconfiggere i bot è quello di usare CAPTCHA completamente automatizzato per distinguere i computer e gli utenti.</span><span class="sxs-lookup"><span data-stu-id="57d38-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="57d38-114">Un test di Turing era in origine un test dove un utente aveva bisogno di decidere se un partner di comunicazione è un computer o umano.</span><span class="sxs-lookup"><span data-stu-id="57d38-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="57d38-115">Sul Web, un CAPTCHA è in genere costituito da un'immagine con alcune lettere distorte.</span><span class="sxs-lookup"><span data-stu-id="57d38-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="57d38-116">L'idea è che solo un uomo può leggere le lettere nell'immagine, mentre gli algoritmi OCR avranno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="57d38-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="57d38-117">Questo approccio presenta diversi vantaggi e svantaggi, ma una discussione di questo approccio esula dall'ambito di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="57d38-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="57d38-118">Esiste tuttavia un controllo in ASP.NET AJAX Control Toolkit, che fornisce un approccio simile: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="57d38-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="57d38-119">È più facile da superare rispetto a un CAPTCHA, ma è molto facile da usare e fa molto bene per i siti Web, ad esempio i Blog in cui viene considerato un successo se la maggior parte dei tentativi di posta indesiderata viene sconfitta, operazione che può essere eseguita dal controllo `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="57d38-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="57d38-120">`NoBot` intercetta il postback del modulo Web ASP.NET corrente se viene soddisfatta almeno una di queste condizioni:</span><span class="sxs-lookup"><span data-stu-id="57d38-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="57d38-121">Il browser non riesce a risolvere un enigma JavaScript, ad esempio quando JavaScript è disattivato.</span><span class="sxs-lookup"><span data-stu-id="57d38-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="57d38-122">L'utente ha inviato il modulo a Fast</span><span class="sxs-lookup"><span data-stu-id="57d38-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="57d38-123">L'indirizzo IP del client ha inviato il modulo troppo spesso in un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="57d38-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="57d38-124">Per verificare la presenza di queste condizioni, il controllo `NoBot` richiede questi attributi (tutti facoltativi):</span><span class="sxs-lookup"><span data-stu-id="57d38-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="57d38-125">`ResponseMinimumDelaySeconds` quantità minima di secondi tra i postback</span><span class="sxs-lookup"><span data-stu-id="57d38-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="57d38-126">`CutoffWindowSeconds` la durata dell'intervallo di tempo in cui i postback di un indirizzo IP sono misure</span><span class="sxs-lookup"><span data-stu-id="57d38-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="57d38-127">`CutoffMaximumInstances` quantità massima di secondi per intervallo di tempo</span><span class="sxs-lookup"><span data-stu-id="57d38-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="57d38-128">Il markup seguente richiede che si verifichino almeno due secondi tra i postback e che siano presenti solo cinque postback o meno entro un intervallo di 30 secondi:</span><span class="sxs-lookup"><span data-stu-id="57d38-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="57d38-129">Quindi, come al solito, assicurarsi di includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX sia caricata e che sia possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="57d38-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="57d38-130">Poiché la maggior parte dei controlli `NoBot` si verifica sul lato server, è necessario verificare il risultato di queste convalide.</span><span class="sxs-lookup"><span data-stu-id="57d38-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="57d38-131">Questa operazione può essere eseguita chiamando il metodo `IsValid()` di `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="57d38-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="57d38-132">Ha un argomento, come parametro di `out`/parametro di`ByRef`, che è di tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="57d38-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="57d38-133">La relativa rappresentazione di stringa contiene il motivo per cui il controllo ha esito negativo e `Valid` in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="57d38-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="57d38-134">Il codice seguente restituisce un messaggio in base al risultato di `NoBot`:</span><span class="sxs-lookup"><span data-stu-id="57d38-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="57d38-135">Infine, è necessario un modulo da inviare e un elemento label per l'output del messaggio e l'operazione è stata completata.</span><span class="sxs-lookup"><span data-stu-id="57d38-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="57d38-136">Quando si esegue lo script e si disattiva JavaScript o si invia il modulo entro i primi due secondi oppure si invia il modulo sette volte entro trenta secondi, viene ricevuto un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="57d38-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="57d38-137">Usare tuttavia questo controllo in modo oculato, poiché solo il 90-95% degli utenti ha attivato JavaScript, quindi il 5-10% degli utenti non riuscirà a eseguire il test `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="57d38-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="57d38-138">[![questo messaggio di errore potrebbe essere stato causato da un bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57d38-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="57d38-139">Questo messaggio di errore potrebbe essere stato causato da un bot ([fare clic per visualizzare l'immagine con dimensioni complete](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57d38-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="57d38-140">Precedente</span><span class="sxs-lookup"><span data-stu-id="57d38-140">Previous</span></span>](fighting-bots-cs.md)
