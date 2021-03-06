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
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a>Autorizzazione solo di caratteri specifici in una casella di testo (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri. Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.

## <a name="overview"></a>Panoramica

I controlli di convalida ASP.NET possono garantire che nell'input utente siano consentiti solo determinati caratteri. Tuttavia, ciò non impedisce agli utenti di digitare caratteri non validi e di inviare il modulo.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il controllo `FilteredTextBox` che estende una casella di testo. Una volta attivato, è possibile immettere nel campo solo un determinato set di caratteri.

Per eseguire questa operazione, è necessario prima di tutto il `ScriptManager` AJAX ASP.NET che carica le librerie JavaScript utilizzate anche da ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Quindi, è necessaria una casella di testo:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Infine, il controllo `FilteredTextBoxExtender` si occupa della limitazione dei caratteri che l'utente può digitare. Per prima cosa, impostare l'attributo `TargetControlID` sul `ID` del controllo `TextBox`. Scegliere quindi uno dei valori `FilterType` disponibili:

- `Custom` predefinita; è necessario fornire un elenco di caratteri validi
- `LowercaseLetters` solo lettere minuscole
- solo cifre `Numbers`
- `UppercaseLetters` solo lettere maiuscole

Se viene utilizzata la `Custom FilterType`, è necessario impostare la proprietà `ValidChars` e fornire un elenco di caratteri che possono essere digitati. Per il modo: se si tenta di incollare il testo nella casella di testo, vengono rimossi tutti i caratteri non validi.

Di seguito è riportato il markup per il controllo `FilteredTextBoxExtender` che consente solo cifre (un elemento che sarebbe stato possibile anche con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Eseguire la pagina e provare a immettere una lettera se JavaScript è abilitato e non funzionerà. le cifre vengono tuttavia visualizzate nella pagina. Si noti tuttavia che la protezione `FilteredTextBox` non è a prova di punta: se JavaScript è abilitato, tutti i dati possono essere immessi nella casella di testo, quindi è necessario usare altri metodi di convalida, ad esempio ASP. Controlli di convalida di NET.

[è possibile immettere solo le cifre ![](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

È possibile immettere solo cifre ([fare clic per visualizzare l'immagine con dimensioni complete](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](allowing-only-certain-characters-in-a-text-box-vb.md)
