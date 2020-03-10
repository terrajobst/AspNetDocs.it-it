---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Uso di HoverMenu con un controllo RepeaterC#() | Microsoft Docs
author: wenz
description: 'Il controllo HoverMenu in AJAX Control Toolkit fornisce un semplice effetto popup: quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata una finestra popup in corrispondenza di un oggetto specifi...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e38b91d837c65191d4b3797fa31ef6112a1f070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578127"
---
# <a name="using-hovermenu-with-a-repeater-control-c"></a>Uso di HoverMenu con un controllo Repeater (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> Il controllo HoverMenu in AJAX Control Toolkit fornisce un semplice effetto popup: quando il puntatore del mouse viene posizionato su un elemento, un popup viene visualizzato in una posizione specificata. È anche possibile usare questo controllo all'interno di un Repeater.

## <a name="overview"></a>Panoramica

Il controllo `HoverMenu` in AJAX Control Toolkit fornisce un semplice effetto popup: quando il puntatore del mouse viene posizionato su un elemento, un popup viene visualizzato in una posizione specificata. È anche possibile usare questo controllo all'interno di un Repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessaria un'origine dati. In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.

Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina. Per utilizzare una quantità limitata di dati, è possibile selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si utilizza l'Assistente di Visual Studio per creare l'origine dati, tenere presente che un bug nella versione corrente non prevede il prefisso del nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Aggiungere quindi un pannello che funge da popup modale:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

A questo punto, il `HoverMenuExtender` entra in gioco. In modo che ogni elemento nell'origine dati ottenga il proprio popup, l'estensione deve essere inserita nella sezione `<ItemTemplate>` del Repeater. Questo è il markup:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

A questo punto, ogni elemento nell'origine dati Visualizza un popup a destra (attributo`PopupPosition`) dopo un ritardo di 50 millisecondi (attributo`PopDelay`).

[![il menu di spostamento accanto a ogni elemento del Repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Il menu di spostamento viene visualizzato accanto a ogni elemento nel ripetitore ([fare clic per visualizzare l'immagine con dimensioni complete](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-hovermenu-with-a-repeater-control-vb.md)
