---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Creazione di un controllo Rating (VB) | Microsoft Docs
author: wenz
description: Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli. Questa operazione richiede in genere un po' di codice, ma il...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611536"
---
# <a name="creating-a-rating-control-vb"></a>Creazione di un controllo Rating (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli. Questa operazione richiede in genere una certa attività di scrittura del codice, ma il Toolkit di controllo è a disposizione.

## <a name="overview"></a>Panoramica di

Molti siti Web, dall'e-commerce ai siti della community, offrono agli utenti la possibilità di valutare articoli o articoli. Questa operazione richiede in genere una certa attività di scrittura del codice, ma il Toolkit di controllo è a disposizione.

## <a name="steps"></a>Passaggi

Prima di tutto, sono necessari almeno due tipi di immagini: uno per un elemento di classificazione compilato e uno per un elemento di classificazione vuoto. Un elemento di classificazione è in genere una stella o uno smiley. Per questo scenario, sono disponibili tre file, smiley. png e Empty. png e smiley-done. png come parte del download del codice sorgente per questa esercitazione.

Quindi, creare un nuovo file ASP.NET e iniziare con l'aggiunta di un controllo `ScriptManager`:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Aggiungere quindi il controllo `Rating` da ASP.NET AJAX Control Toolkit. Per questo esempio è necessario impostare gli attributi seguenti:

- `CurrentRating` della classificazione iniziale da usare
- `MaxRating` la classificazione massima
- `EmptyStarCssClass` la classe CSS da utilizzare quando un elemento della classificazione (stella) è vuoto
- `FilledStarCssClass` la classe CSS da utilizzare quando un elemento della classificazione (stella) viene compilato
- `StarCssClass` la classe CSS da utilizzare per una stat visibile
- `WaitingStarCssClass` la classe CSS da usare mentre una classificazione a stelle viene restituita al server

Di seguito è riportato il markup che consente di creare un controllo Rating con cinque elementi (smiley) di cui non è stato compilato inizialmente alcuno:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Le tre classi CSS a cui viene fatto riferimento devono ora visualizzare i file di immagine appropriati, operazione semplice con CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Assicurarsi di specificare la larghezza e l'altezza delle tre immagini; in caso contrario, la visualizzazione potrebbe sembrare un po' complicata.

Infine, il risultato della classificazione deve essere visualizzato all'utente (o almeno salvato in un database). Aggiungere quindi un'etichetta per l'output di un messaggio di testo e un pulsante Submit (Invia) per eseguire il postback del modulo rating al server:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Nel codice sul lato server accedere al controllo Rating tramite la relativa `ID` e quindi accedere alla relativa proprietà `CurrentRating`, ovvero il numero degli elementi di classificazione selezionati, nell'esempio un valore compreso tra 0 e 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Salvare la pagina e caricarla nel browser. Quando si passa il mouse sugli elementi della classificazione (inizialmente vuoti), si verifica un effetto JavaScript: la classificazione viene modificata. Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente. Infine, quando si invia il modulo, il codice lato server restituisce la classificazione selezionata.

[![la creazione di un sistema di classificazione con codice minimo](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Creazione di un sistema di classificazione con codice minimo ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-rating-control-cs.md)
