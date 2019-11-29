---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Verifica della complessità di una password (VB) | Microsoft Docs
author: wenz
description: Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere. Controllo PasswordStrength in ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606268"
---
# <a name="testing-the-strength-of-a-password-vb"></a>Test della complessità di una password (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere. Il controllo PasswordStrength in ASP.NET AJAX Control Toolkit può verificare l'efficacia di una password.

## <a name="overview"></a>Panoramica di

Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere. Il controllo `PasswordStrength` in ASP.NET AJAX Control Toolkit può verificare l'efficacia di una password.

## <a name="steps"></a>Passaggi

Il controllo `PasswordStrength` estende una casella di testo e controlla se la password è sufficientemente adeguata. Offre un'ampia gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:

- `MinimumNumericCharacters` numero minimo di caratteri numerici richiesti nella password
- `MinimumSymbolCharacters` numero minimo di caratteri simbolici (non lettere e cifre) richiesti nella password
- `PreferredPasswordLength` lunghezza minima della password
- `RequiresUpperAndLowerCaseCharacters` se la password deve usare caratteri sia maiuscoli che minuscoli

Il `StrengthIndicatorType` fornisce le informazioni su come presentare il livello di attendibilità della password, come testo (valore `"Text"`) o come indicatore di stato (valore `"BarIndicator"`). Nell'attributo `DisplayPosition` è possibile configurare la posizione in cui vengono visualizzate le informazioni. Di seguito è riportato un esempio completo, tra cui il controllo `ScriptManager` AJAX ASP.NET, il controllo `PasswordStrength` e ovviamente una casella di testo in cui l'utente può immettere una password. Ai fini della dimostrazione, il campo del secondo form è un campo di testo normale e non un campo password, in modo che sia possibile visualizzare durante lo sviluppo ciò che si sta digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Esegui la pagina e digita il testo: solo dopo aver immesso lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata inviolabile.

[![ora la password è (piuttosto) corretta](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

A questo punto la password è (piuttosto) corretta ([fare clic per visualizzare l'immagine con dimensioni complete](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](testing-the-strength-of-a-password-cs.md)
