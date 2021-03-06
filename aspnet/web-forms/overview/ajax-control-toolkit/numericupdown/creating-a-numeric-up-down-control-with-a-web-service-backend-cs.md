---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Creazione di un controllo su/giù numerico con un back-endC#del servizio Web () | Microsoft Docs
author: wenz
description: Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe rivelarsi più c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627351"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Creazione di un controllo su/giù numerico con un back-end del servizio Web (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe risultare più comodo. Per impostazione predefinita, il controllo NumericUpDown aumenta o diminuisce sempre un valore di 1, ma un servizio Web dimostra maggiore flessibilità.

## <a name="overview"></a>Panoramica

Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo su/giù numerico (presente in Windows e in altri sistemi operativi) potrebbe risultare più comodo. Per impostazione predefinita, il controllo `NumericUpDown` aumenta o diminuisce sempre un valore di 1, ma un servizio Web dimostra maggiore flessibilità.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il `NumericUpDown` Extender che aggiunge automaticamente due pulsanti a una casella di testo: uno per l'aumento del valore, uno per la riduzione. Tuttavia, il controllo supporta anche una chiamata al servizio Web (o chiamata al metodo della pagina). Ogni volta che si fa clic sul pulsante su o giù, il codice JavaScript si connette al server Web ed esegue un metodo. La firma del metodo è la seguente:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

L'argomento `current` è il valore corrente nella casella di testo. l'attributo `tag` è costituito da dati di contesto aggiuntivi che possono essere impostati come proprietà del `NumericUpDown` Extender (ma non obbligatorio).

Per questo esempio, il controllo su/giù numerico deve consentire solo valori con potenze di due: 1, 2, 4, 8, 16, 32, 64 e così via. Pertanto, il metodo eseguito quando l'utente desidera aumentare il valore deve raddoppiare il valore precedente; l'altro metodo deve dividere il valore per due. Ecco il servizio Web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Infine, creare una nuova pagina ASP.NET. Come di consueto, è necessario un controllo `ScriptManager`, un controllo `TextBox` e un controllo `NumericUpDownExtender`. Per quest'ultimo, è necessario fornire le informazioni sul servizio Web:

- nome `ServiceDownMethod` del metodo Web o del metodo di pagina inattivo
- `ServiceDownPath` percorso del servizio Web con il metodo di servizio inattivo; omettere se si usa un metodo di pagina
- nome `ServiceUpMethod` del metodo Web o della pagina
- `ServiceUpPath` percorso del servizio Web con il metodo up Service; omettere se si usa un metodo di pagina

Ecco il markup completo per la pagina:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Se si esegue la pagina, si noterà che il valore nella casella di testo viene sempre raddoppiato quando si fa clic sul pulsante superiore e viene dimezzato quando si fa clic sul pulsante inferiore.

[![vengono visualizzati solo i numeri che sono una potenza di 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Vengono visualizzati solo i numeri che sono una potenza di 2 ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
