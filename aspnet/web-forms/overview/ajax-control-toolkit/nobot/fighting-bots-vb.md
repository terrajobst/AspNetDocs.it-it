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
# <a name="fighting-bots-vb"></a>Lotta contro i bot (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> I bot automatici intonano i weblog e altri siti Web con posta indesiderata, inviando moduli di commento senza alcuna interazione dell'utente Il controllo NoBot in ASP.NET AJAX Control Toolkit può aiutare a combattere questi bot.

## <a name="overview"></a>Panoramica

I bot automatici intonano i weblog e altri siti Web con posta indesiderata, inviando moduli di commento senza alcuna interazione dell'utente Il controllo NoBot in ASP.NET AJAX Control Toolkit può aiutare a combattere questi bot.

## <a name="steps"></a>Passaggi

Un approccio comune per sconfiggere i bot è quello di usare CAPTCHA completamente automatizzato per distinguere i computer e gli utenti. Un test di Turing era in origine un test dove un utente aveva bisogno di decidere se un partner di comunicazione è un computer o umano. Sul Web, un CAPTCHA è in genere costituito da un'immagine con alcune lettere distorte. L'idea è che solo un uomo può leggere le lettere nell'immagine, mentre gli algoritmi OCR avranno esito negativo.

Questo approccio presenta diversi vantaggi e svantaggi, ma una discussione di questo approccio esula dall'ambito di questa esercitazione. Esiste tuttavia un controllo in ASP.NET AJAX Control Toolkit, che fornisce un approccio simile: `NoBot`. È più facile da superare rispetto a un CAPTCHA, ma è molto facile da usare e fa molto bene per i siti Web, ad esempio i Blog in cui viene considerato un successo se la maggior parte dei tentativi di posta indesiderata viene sconfitta, operazione che può essere eseguita dal controllo `NoBot`.

`NoBot` intercetta il postback del modulo Web ASP.NET corrente se viene soddisfatta almeno una di queste condizioni:

- Il browser non riesce a risolvere un enigma JavaScript, ad esempio quando JavaScript è disattivato.
- L'utente ha inviato il modulo a Fast
- L'indirizzo IP del client ha inviato il modulo troppo spesso in un determinato periodo di tempo.

Per verificare la presenza di queste condizioni, il controllo `NoBot` richiede questi attributi (tutti facoltativi):

- `ResponseMinimumDelaySeconds` quantità minima di secondi tra i postback
- `CutoffWindowSeconds` la durata dell'intervallo di tempo in cui i postback di un indirizzo IP sono misure
- `CutoffMaximumInstances` quantità massima di secondi per intervallo di tempo

Il markup seguente richiede che si verifichino almeno due secondi tra i postback e che siano presenti solo cinque postback o meno entro un intervallo di 30 secondi:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Quindi, come al solito, assicurarsi di includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX sia caricata e che sia possibile usare il Toolkit di controllo:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Poiché la maggior parte dei controlli `NoBot` si verifica sul lato server, è necessario verificare il risultato di queste convalide. Questa operazione può essere eseguita chiamando il metodo `IsValid()` di `NoBot`. Ha un argomento, come parametro di `out`/parametro di`ByRef`, che è di tipo `NoBotState`. La relativa rappresentazione di stringa contiene il motivo per cui il controllo ha esito negativo e `Valid` in caso contrario. Il codice seguente restituisce un messaggio in base al risultato di `NoBot`:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Infine, è necessario un modulo da inviare e un elemento label per l'output del messaggio e l'operazione è stata completata.

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Quando si esegue lo script e si disattiva JavaScript o si invia il modulo entro i primi due secondi oppure si invia il modulo sette volte entro trenta secondi, viene ricevuto un messaggio di errore. Usare tuttavia questo controllo in modo oculato, poiché solo il 90-95% degli utenti ha attivato JavaScript, quindi il 5-10% degli utenti non riuscirà a eseguire il test `NoBot`.

[![questo messaggio di errore potrebbe essere stato causato da un bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Questo messaggio di errore potrebbe essere stato causato da un bot ([fare clic per visualizzare l'immagine con dimensioni complete](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](fighting-bots-cs.md)
