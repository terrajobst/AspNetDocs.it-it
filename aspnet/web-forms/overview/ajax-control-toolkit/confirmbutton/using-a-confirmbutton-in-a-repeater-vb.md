---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Uso di un ConfirmButton in un Repeater (VB) | Microsoft Docs
author: wenz
description: Il dispositivo Extender ConfirmButton in AJAX Control Toolkit crea una finestra popup Sì/No quando l'utente fa clic su un pulsante, incluso il controllo LinkButton. Solo se sì è...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 001233d866d8a731d93d6900f714cd2060f3d08c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599315"
---
# <a name="using-a-confirmbutton-in-a-repeater-vb"></a>Uso di ConfirmButton in un controllo Repeater (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Il dispositivo Extender ConfirmButton in AJAX Control Toolkit crea una finestra popup Sì/No quando l'utente fa clic su un pulsante, incluso il controllo LinkButton. Solo se si fa clic su Sì, viene eseguita l'azione del pulsante; in caso contrario, viene annullato. Questa operazione è possibile anche in un ripetitore.

## <a name="overview"></a>Panoramica di

Il dispositivo Extender ConfirmButton in AJAX Control Toolkit crea una finestra popup Sì/No quando l'utente fa clic su un pulsante, incluso il controllo LinkButton. Solo se si fa clic su Sì, viene eseguita l'azione del pulsante; in caso contrario, viene annullato. Questa operazione è possibile anche in un ripetitore.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessaria un'origine dati. In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.

Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Quindi, è necessaria un'origine dati. Per motivi di semplicità, vengono recuperate solo le prime cinque voci della tabella vendors di AdventureWorks. Si noti che quando si usa la procedura guidata di Visual Studio per creare l'origine dati, il nome della tabella (`Vendors`) attualmente non è preceduto correttamente `Purchasing`. Il markup seguente è quello corretto:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Questa origine dati può quindi essere utilizzata all'interno di un Repeater. Come di consueto, il metodo `DataBinder.Eval()` recupera i dati dall'origine dati. Il controllo `ConfirmButtonExtender` deve quindi essere inserito all'interno della sezione `<ItemTemplate>` del Repeater in modo che venga visualizzato per ogni voce nell'origine dati.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]

[![viene visualizzato il pulsante conferma accanto a ogni voce dell'origine dati](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Il pulsante conferma viene visualizzato accanto a ogni voce dall'origine dati ([fare clic per visualizzare l'immagine con dimensioni complete](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-a-confirmbutton-in-a-repeater-cs.md)
