---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Utilizzo di TextBoxWatermark in un oggetto FormView (VB) | Microsoft Docs
author: wenz
description: Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1dd17ed57383680122c4ca613ca71f6682dec40e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627106"
---
# <a name="using-textboxwatermark-in-a-formview-vb"></a>Uso di TextBoxWatermark in un controllo FormView (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Il controllo TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, viene svuotato. Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato. Questa operazione è possibile anche in un controllo FormView.

## <a name="overview"></a>Panoramica

Il controllo `TextBoxWatermark` in AJAX Control Toolkit estende una casella di testo in modo che un testo venga visualizzato all'interno della casella. Quando un utente fa clic sulla casella, viene svuotato. Se l'utente lascia la casella senza immettere il testo, il testo precompilato viene nuovamente visualizzato. Questa operazione è possibile anche all'interno di un controllo `FormView`.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessaria un'origine dati. In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.

Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Aggiungere quindi un'origine dati alla pagina che supporta le istruzioni SQL `DELETE`, `INSERT` e `UPDATE`. Se si utilizza l'Assistente di Visual Studio per creare l'origine dati, tenere presente che un bug nella versione corrente non prevede il prefisso del nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Ricordare il nome (`ID`) dell'origine dati, poiché verrà usato nella proprietà `DataSourceID` del controllo `FormView`. La sezione `<InsertItemTemplate>` della `FormView` contiene una casella di testo che viene estesa dal controllo `TextBoxWatermarkExtender`. Verificare che il dispositivo Extender si trovi all'interno del modello e non al suo esterno.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

A questo punto, quando l'utente passa alla modalità di inserimento del controllo `FormView`, il campo di testo per il nuovo fornitore viene precompilato grazie al controllo `TextBoxWatermarkExtender`. Un clic all'interno della casella di testo consente di scomparire.

[![la filigrana nel campo deriva dal dispositivo Extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

La filigrana nel campo deriva dal dispositivo Extender ([fare clic per visualizzare l'immagine con dimensioni complete](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-textboxwatermark-with-validation-controls-cs.md)
> [Successivo](using-textboxwatermark-with-validation-controls-vb.md)
