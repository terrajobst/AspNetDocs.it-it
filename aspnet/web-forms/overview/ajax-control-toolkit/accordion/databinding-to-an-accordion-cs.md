---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Associazione dati a una fisarmonicaC#() | Microsoft Docs
author: wenz
description: Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614555"
---
# <a name="databinding-to-an-accordion-c"></a>Data binding a un controllo Accordion (C#)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il binding a un'origine dati offre maggiore flessibilità.

## <a name="overview"></a>Panoramica

Il controllo di Accordion in AJAX Control Toolkit offre più riquadri e consente all'utente di visualizzarne uno alla volta. I pannelli vengono in genere dichiarati all'interno della pagina stessa, ma il binding a un'origine dati offre maggiore flessibilità.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessaria un'origine dati. In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.

Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina (ma all'interno dell'elemento `<form>`):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina. Per utilizzare una quantità limitata di dati, è possibile selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si utilizza l'Assistente di Visual Studio per creare l'origine dati, tenere presente che un bug nella versione corrente non prevede il prefisso del nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Ricordare il nome (ID) dell'origine dati. Questa identificazione deve quindi essere usata nella proprietà `DataSourceID` del controllo di Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

All'interno del controllo di Accordion è possibile fornire modelli per varie parti del controllo, tra cui l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`). All'interno di questi elementi, è sufficiente restituire i dati dall'origine dati, usando il metodo `DataBinder.Eval()`:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Quando la pagina viene caricata, l'origine dati deve essere associata alla fisarmonica con il codice sul lato server:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Per concludere questo esempio, è necessario definire le due classi CSS a cui viene fatto riferimento nel controllo della fisarmonica (nelle proprietà `HeaderCssClass` e `ContentCssClass`). Inserire il markup seguente nella sezione `<head>` della pagina:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![i dati della fisarmonica provengono direttamente dall'origine dati](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

I dati della fisarmonica provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine con dimensioni complete](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-adding-an-accordion-pane-cs.md)
