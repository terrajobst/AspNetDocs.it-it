---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Creazione di un controller (VB) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther illustra come è possibile aggiungere un controller a un'applicazione MVC ASP.NET.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601458"
---
# <a name="creating-a-controller-vb"></a>Creazione di un controller (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther illustra come è possibile aggiungere un controller a un'applicazione MVC ASP.NET.

L'obiettivo di questa esercitazione è spiegare come è possibile creare nuovi controller MVC ASP.NET. Si apprenderà come creare i controller usando l'opzione di menu Aggiungi controller di Visual Studio e creando manualmente un file di classe.

### <a name="using-the-add-controller-menu-option"></a>Utilizzo dell'opzione di menu Aggiungi controller

Il modo più semplice per creare un nuovo controller è fare clic con il pulsante destro del mouse sulla cartella controller nella finestra di Esplora soluzioni di Visual Studio e selezionare l'opzione di menu **Aggiungi, controller** (vedere la figura 1). Selezionando questa opzione di menu viene visualizzata la finestra di dialogo **Aggiungi controller** (vedere la figura 2).

[![finestra di dialogo nuovo progetto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figura 01**: aggiunta di un nuovo controller ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image2.png))

[![finestra di dialogo nuovo progetto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figura 02**: finestra di dialogo Aggiungi controller ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image4.png))

Si noti che la prima parte del nome del controller viene evidenziata nella finestra di dialogo **Aggiungi controller** . Ogni nome di controller deve terminare con il *controller*del suffisso. Ad esempio, è possibile creare un controller denominato *ProductController* , ma non un controller denominato *Product*.

Se si crea un controller in cui manca il suffisso del *controller* , non sarà possibile richiamare il controller. Non eseguire questa operazione: ho perso innumerevoli ore di vita dopo aver commesso questo errore.

**Listato 1-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

È sempre necessario creare i controller nella cartella Controllers. In caso contrario, si violeranno le convenzioni di ASP.NET MVC e gli altri sviluppatori avranno un tempo più difficile per comprendere l'applicazione.

### <a name="scaffolding-action-methods"></a>Metodi di azione di impalcatura

Quando si crea un controller, è possibile scegliere di generare automaticamente metodi di azione di creazione, aggiornamento e dettagli (vedere la figura 3). Se si seleziona questa opzione, viene generata la classe controller nel listato 2.

[![la creazione automatica di metodi di azione](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figura 03**: creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image6.png))

**Listato 2-Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Questi metodi generati sono metodi stub. È necessario aggiungere la logica effettiva per la creazione, l'aggiornamento e la visualizzazione dei dettagli per un cliente. Tuttavia, i metodi dello stub forniscono un ottimo punto di partenza.

### <a name="creating-a-controller-class"></a>Creazione di una classe controller

Il controller MVC ASP.NET è semplicemente una classe. Se si preferisce, è possibile ignorare la pratica impalcatura del controller di Visual Studio e creare manualmente una classe controller. Attenersi ai passaggi riportati di seguito.

1. Fare clic con il pulsante destro del mouse sulla cartella controller e selezionare l'opzione di menu **Aggiungi, nuovo elemento** e selezionare il modello di **classe** (vedere la figura 4).
2. Assegnare alla nuova classe il nome PersonController. vb, quindi fare clic sul pulsante **Aggiungi** .
3. Modificare il file di classe risultante in modo che la classe erediti dalla classe System. Web. Mvc. controller di base (vedere l'elenco 3).

[![la creazione di una nuova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figura 04**: creazione di una nuova classe ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-controller-vb/_static/image8.png))

**Listato 3-Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!". È possibile richiamare questa azione del controller eseguendo l'applicazione e richiedendo un URL simile al seguente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Il Server di sviluppo ASP.NET utilizza un numero di porta casuale, ad esempio 40071. Quando si immette un URL per richiamare un controller, è necessario specificare il numero di porta corretto. È possibile determinare il numero di porta posizionando il puntatore del mouse sull'icona del Server di sviluppo ASP.NET nell'area di notifica di Windows (in basso a destra dello schermo).
> 
> [!div class="step-by-step"]
> [Precedente](adding-dynamic-content-to-a-cached-page-vb.md)
> [Successivo](creating-an-action-vb.md)
