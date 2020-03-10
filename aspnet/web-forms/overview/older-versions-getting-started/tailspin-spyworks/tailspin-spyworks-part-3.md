---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: menu layout e categoria | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639118"
---
# <a name="part-3-layout-and-category-menu"></a>Parte 3: menu layout e categoria

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.

## <a id="_Toc260221669"></a>Aggiunta di un layout e di un menu di categoria

Nella pagina master del sito verrà aggiunto un div per la colonna sul lato sinistro che conterrà il menu Product Category.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Si noti che l'allineamento e la formattazione desiderate verranno fornite dalla classe CSS aggiunta al file Style. CSS.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Il menu Categoria prodotto verrà creato dinamicamente in fase di esecuzione eseguendo una query sul database Commerce per le categorie di prodotti esistenti e creando le voci di menu e i collegamenti corrispondenti.

A tale scopo, si utilizzeranno due di ASP. Controlli dati potenti di NET. Il controllo "Entity Data Source" e il controllo "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Passiamo a "visualizzazione progettazione" e utilizzeremo gli helper per configurare i controlli.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Impostare la proprietà ID EntityDataSource sul menu EDS\_categoria\_e fare clic su "Configura origine dati".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selezionare la connessione CommerceEntities creata per l'utente quando è stato creato il modello di origine dati dell'entità per il database Commerce e fare clic su "Avanti".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selezionare il nome del set di entità "Categories" e lasciare le altre opzioni predefinite. Fare clic su "fine".

A questo punto è possibile impostare la proprietà ID dell'istanza del controllo ListView che è stata inserita nella pagina su ListView\_ProductsMenu e attivare il relativo helper.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Sebbene sia possibile utilizzare le opzioni di controllo per formattare la visualizzazione e la formattazione degli elementi di dati, la creazione del menu richiederà solo markup semplice, quindi il codice sarà immesso nella visualizzazione origine.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Si noti l'istruzione "eval": &lt;% # Eval ("CategoryName")%&gt;

La sintassi ASP.NET &lt;% #%&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire qualsiasi contenuto all'interno di e di restituire i risultati "nella riga".

L'istruzione eval ("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi degli elementi del modello di entità "CategoryName". Si tratta di una sintassi concisa per una funzionalità molto potente.

Consente ora di eseguire l'applicazione.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Si noti che il menu Product Category è ora visualizzato e quando si passa il puntatore del mouse su una delle voci di menu Category è possibile vedere che il collegamento voce di menu fa riferimento a una pagina che è ancora necessario implementare denominato Products. aspx e che è stato compilato un argomento della stringa di query dinamica che contiene  ID categoria.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-2.md)
> [Successivo](tailspin-spyworks-part-4.md)
