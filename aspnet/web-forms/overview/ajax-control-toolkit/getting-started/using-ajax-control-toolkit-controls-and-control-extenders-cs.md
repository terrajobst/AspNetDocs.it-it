---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Uso di controlli e controllo Extender di AJAX ControlC#Toolkit () | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere controlli e Extender di AJAX Control Toolkit alle pagine di ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 1acafaadaf373b488b9e85b7ba31f08cd3b53e85
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535413"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Uso di controlli e controlli Extender di AJAX Control Toolkit (C#)

[Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere controlli e Extender di AJAX Control Toolkit alle pagine di ASP.NET.

AJAX Control Toolkit contiene un set di controlli e di controllo Extender. In questa breve esercitazione si apprenderà come aggiungere controlli e controlli Extender a una pagina ASP.NET.

> [!NOTE] 
> 
> Per istruzioni sull'installazione di AJAX Control Toolkit e sull'aggiunta di AJAX Control Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer, vedere l'esercitazione [Introduzione a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).

## <a name="using-ajax-control-toolkit-controls"></a>Uso di controlli AJAX Control Toolkit

Un controllo AJAX Control Toolkit funziona esattamente come un normale controllo ASP.NET. È possibile trascinare il controllo dalla casella degli strumenti su una pagina ASP.NET. È possibile aggiungere il controllo alla pagina nella visualizzazione progettazione o nella visualizzazione origine.

Quando si usano i controlli di AJAX Control Toolkit, è necessario un requisito speciale. La pagina deve contenere un controllo ScriptManager. Il controllo ScriptManager è responsabile dell'inclusione di tutti i necessari JavaScript necessari per i controlli AJAX Control Toolkit.

La scheda AJAX Control Toolkit, ad esempio, include un controllo denominato controllo editor. Questo controllo consente di visualizzare un editor HTML RTF. Per aggiungere il controllo editor a una pagina, attenersi alla procedura seguente:

1. Creare una nuova pagina ASP.NET denominata ShowEditor. aspx
2. Selezionare il controllo ScriptManager sotto la scheda AJAX Extensions nella casella degli strumenti e trascinare il controllo nella pagina.
3. Selezionare il controllo editor sotto la scheda AJAX Control Toolkit nella casella degli strumenti e trascinare il controllo nella pagina (vedere la figura 1). La finestra di progettazione avrà un aspetto simile a quello della figura 2.
4. Eseguire il sito Web selezionando l'opzione di menu **debug, Avvia debug** o premendo il tasto F5.
5. Verrà visualizzata la pagina nella figura 3.

[![selezione del controllo editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: selezione del controllo editor HTML ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))

[![la finestra di progettazione di Visual Studio con ScriptManager e il controllo di modifica](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: finestra di progettazione di Visual Studio con ScriptManager e controllo di modifica ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))

[![pagina DisplayEditor. aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: pagina DisplayEditor. aspx ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Uso delle estensioni di controllo di AJAX Control Toolkit

AJAX Control Toolkit contiene anche Extender di controllo. Come suggerisce il nome, un dispositivo Extender di controllo estende la funzionalità di un controllo esistente. Ad esempio, l'estensione del controllo ConfirmButton estende il controllo Button ASP.NET standard. Il dispositivo Extender modifica il comportamento del controllo Button in modo che il pulsante visualizzi una finestra di dialogo di conferma quando si fa clic su di essa.

Un controllo Extender di controllo, analogamente a un controllo AJAX Control Toolkit, richiede un controllo ScriptManager. È necessario aggiungere un controllo ScriptManager a una pagina prima di iniziare a utilizzare i dispositivi Extender di controllo nella pagina.

Seguire questa procedura per usare il dispositivo Extender del controllo ConfirmButton:

1. Creare una nuova pagina ASP.NET denominata ShowConfirmButton. aspx
2. Aggiungere un controllo ScriptManager alla pagina trascinando il controllo nella pagina dalla scheda Estensioni AJAX.
3. Aggiungere un controllo Button standard alla pagina trascinando il pulsante da sotto la scheda standard della casella degli strumenti nell'area di progettazione.
4. Fare clic sull'opzione **Aggiungi attività Extender** (vedere la figura 4).
5. Nella finestra di dialogo Scegli Extender selezionare ConfirmButtonExtender (vedere la figura 5) e fare clic sul pulsante OK.
6. Selezionare il controllo Button nella finestra di progettazione ed espandere il nodo Extender, Button1\_ConfirmButtonExtender nella Finestra Proprietà (vedere la figura 6). Assegnare il valore *realmente?* alla proprietà ConfirmText.
7. Eseguire la pagina selezionando l'opzione di menu **debug, Avvia debug** o premere il tasto F5.

[![l'opzione Aggiungi attività Extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: opzione Aggiungi attività Extender ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))

[![selezione dell'Extender del controllo ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: selezione dell'estensione del controllo ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))

[![impostazione di una proprietà ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: impostazione di una proprietà ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))

Quando si apre la pagina, viene visualizzato un pulsante. Quando si fa clic sul pulsante, viene visualizzata la finestra di dialogo di conferma nella figura 7.

[![visualizzare la finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: visualizzazione della finestra di dialogo[di conferma (fare clic per visualizzare l'immagine con dimensioni complete](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))

Si noti che in genere non si trascina un controllo Extender su una pagina. Usare invece l'opzione **Aggiungi attività Extender** per aggiungere un dispositivo Extender a un controllo che è già stato aggiunto a una pagina. Si noti inoltre che è possibile impostare le proprietà del controllo Extender aprendo la finestra delle proprietà per il controllo da estendere.

Un singolo controllo ASP.NET può essere esteso da più Extender del controllo. Nella finestra delle proprietà per il controllo da estendere saranno elencati tutti gli Extender del controllo associati al controllo.

> [!div class="step-by-step"]
> [Precedente](get-started-with-the-ajax-control-toolkit-cs.md)
> [Successivo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
