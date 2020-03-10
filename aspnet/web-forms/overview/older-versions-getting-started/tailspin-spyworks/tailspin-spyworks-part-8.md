---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: pagine finali, gestione delle eccezioni e conclusione | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Parte 8 aggiunge una pagina di contatto, informazioni sulla pagina e sull'eccezione...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586884"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: pagine finali, gestione delle eccezioni e conclusione

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 8 aggiunge una pagina di contatto, informazioni sulla pagina e la gestione delle eccezioni. Questa è la conclusione della serie.

## <a id="_Toc260221680"></a>Pagina contatto (invio di messaggi di posta elettronica da ASP.NET)

Creare una nuova pagina denominata contactus. aspx

Utilizzando la finestra di progettazione, creare il seguente form prendendo nota speciale per includere ToolkitScriptManager e il controllo editor da AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Fare doppio clic sul pulsante "Invia" per generare un gestore dell'evento click nel file code-behind e implementare un metodo per inviare le informazioni di contatto come un messaggio di posta elettronica.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Questo codice richiede che il file Web. config includa una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio della posta.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Pagina informazioni

Crea una pagina denominata aboutus. aspx e Aggiungi il contenuto che preferisci.

## <a id="_Toc260221682"></a>Gestore eccezioni globale

Infine, nell'applicazione sono state generate eccezioni e si verificano circostanze non previste che a freddo provocano anche eccezioni non gestite nell'applicazione Web.

Non si vuole che venga visualizzata un'eccezione non gestita in un visitatore del sito Web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oltre a essere un'esperienza utente terribile, le eccezioni non gestite possono anche costituire un problema di sicurezza.

Per risolvere questo problema, verrà implementato un gestore di eccezioni globale.

A tale scopo, aprire il file Global. asax e prendere nota del seguente gestore eventi generato in precedenza.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Aggiungere il codice per implementare l'applicazione\_gestore errori come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Aggiungere quindi una pagina denominata Error. aspx alla soluzione e aggiungere il frammento di markup.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

A questo punto, nella pagina\_carica gestore eventi estrarre i messaggi di errore dall'oggetto richiesta.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Conclusione

Abbiamo visto che WebForm ASP.NET semplifica la creazione di un sito Web sofisticato con accesso al database, appartenenza, AJAX e così via. abbastanza rapidamente.

Speriamo che in questa esercitazione siano stati forniti gli strumenti necessari per iniziare a creare applicazioni Web Form ASP.NET.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-7.md)
