---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Uso del controllo Extender ColorPicker (VB) | Microsoft Docs
author: microsoft
description: ColorPicker è un Extender AJAX ASP.NET che fornisce la funzionalità di selezione dei colori sul lato client con interfaccia utente in un controllo popup. Può essere allegato a qualsiasi ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 77e2e3bc61a5e1498570959ca40acff83dc3fc82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554502"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Uso di ColorPicker Control Extender (VB)

[Microsoft](https://github.com/microsoft)

> ColorPicker è un Extender AJAX ASP.NET che fornisce la funzionalità di selezione dei colori sul lato client con interfaccia utente in un controllo popup. Può essere collegato a qualsiasi controllo TextBox ASP.NET. È.

L'obiettivo di questa esercitazione è spiegare come è possibile usare il controllo Extender di AJAX Control Toolkit ColorPicker. Il dispositivo Extender del controllo ColorPicker Visualizza una finestra di dialogo popup che consente di selezionare un colore. ColorPicker è utile quando si desidera fornire un'interfaccia utente intuitiva che consentirà a un utente di selezionare un colore.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estensione di un controllo TextBox con l'Extender del controllo ColorPicker

Si supponga, ad esempio, di voler creare un sito Web che consenta ai visitatori di creare schede aziendali personalizzate. I visitatori possono immettere il testo per un biglietto da visita e selezionare il colore. La pagina ASP.NET nel listato 1 contiene due controlli TextBox denominati txtCardText e txtCardColor. Quando si invia il modulo, vengono visualizzati i valori selezionati (vedere la figura 1).

[![forma semplice per la creazione di un biglietto da lavoro](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: modulo semplice per la creazione di un biglietto da lavoro ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-colorpicker-control-extender-vb/_static/image2.png))

**Listato 1-CreateCard. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Il modulo nel listato 1 funziona, ma non offre un'esperienza utente eccezionale. L'utente deve digitare un colore nella casella di testo. Se l'utente vuole un colore specializzato, ad esempio, solo l'ombreggiatura corretta di pisello verde, l'utente deve individuare il codice colore HTML senza alcuna guida.

Per creare un'esperienza utente migliore, è possibile usare il dispositivo Extender del controllo ColorPicker. ColorPicker Visualizza una finestra di dialogo a colori quando lo stato attivo viene spostato in un controllo TextBox (vedere la figura 2).

[![il dispositivo Extender del controllo ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: controllo Extender ColorPicker ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-colorpicker-control-extender-vb/_static/image4.png))

È necessario completare due passaggi per usare il dispositivo Extender del controllo ColorPicker con il formato elencato 1:

1. Aggiungere un controllo ScriptManager alla pagina
2. Aggiungere l'estensione del controllo ColorPicker alla pagina

Prima di poter usare ColorPicker, è necessario aggiungere un ScriptManager alla pagina. Un posto ideale per aggiungere ScriptManager è immediatamente sotto il modulo di apertura &lt;lato server&gt; tag. È possibile trascinare ScriptManager nella pagina dalla casella degli strumenti (ScriptManager si trova nella scheda Estensioni AJAX). In alternativa, è possibile digitare il seguente tag nella visualizzazione origine sotto il tag di apertura del form lato server:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

Il modo più semplice per aggiungere l'estensione del controllo ColorPicker alla pagina è in visualizzazione progettazione. Se si passa il puntatore del mouse sulla casella di testo txtCardColor, viene visualizzata un'opzione di attività intelligente che consente di aggiungere un'estensione (vedere la figura 3). Se si seleziona questa opzione, viene visualizzata la procedura guidata Extender (vedere la figura 4).

[![l'aggiunta di un dispositivo Extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: aggiunta di un dispositivo Extender ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-colorpicker-control-extender-vb/_static/image6.png))

[![selezione di un controllo Extender con la procedura guidata Extender](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: selezione di un dispositivo Extender di controllo con la procedura guidata di estensione ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-colorpicker-control-extender-vb/_static/image8.png))

È possibile scegliere il dispositivo Extender ColorPicker per estendere la casella di testo txtCardColor con ColorPicker Extender. Scegliere OK per chiudere la finestra di dialogo.

Dopo aver apportato queste modifiche, l'origine della pagina è simile all'elenco 2.

**Listato 2-CreateCard. aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Si noti che nella pagina è ora contenuto un controllo ColorPickerExtender visualizzato direttamente sotto il controllo TextBox txtCardColor. Il controllo ColorPickerExtender estende il controllo txtCardColor in modo da visualizzare una finestra di dialogo di selezione colori.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Uso di un pulsante per aprire la finestra di dialogo Selezione colori

Il dispositivo Extender ColorPicker supporta le proprietà seguenti:

- PopupButtonId: ID di un pulsante della pagina che determina la visualizzazione della finestra di dialogo di selezione dei colori.
- PopupPosition: posizione, relativa al controllo di destinazione, della finestra di dialogo di selezione dei colori. I valori possibili sono Absolute, Center, BottomLeft, BottomRight, Left, ToRight, Right e Left (il valore predefinito è BottomLeft).
- SampleControlId: ID di un controllo che Visualizza il colore selezionato.
- SelectedColor: colore iniziale selezionato dal ColorPicker.

È possibile utilizzare queste proprietà per personalizzare la modalità di visualizzazione della finestra di dialogo di selezione dei colori e il modo in cui viene visualizzato il colore selezionato. Nella pagina del listato 3 viene illustrato come è possibile utilizzare diverse di queste proprietà.

**Listato 3-CreateCardButton. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Nella pagina del listato 3 è incluso un pulsante Seleziona colore (vedere la figura 5). Quando si fa clic su questo pulsante, viene visualizzata la finestra di dialogo Selezione colori sopra la casella di testo. Se si seleziona un colore dalla finestra di dialogo, il colore selezionato viene visualizzato come colore di sfondo del controllo etichetta lblSample.

La proprietà PopupButtonID di ColorPicker viene utilizzata per associare il pulsante Seleziona colore al dispositivo Extender ColorPicker. Quando si specifica un valore per la proprietà PopupButtonID, la finestra di dialogo di selezione dei colori non viene più visualizzata quando il controllo di destinazione ha lo stato attivo. È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.

La proprietà SampleControlID viene usata per associare un controllo che Visualizza il colore selezionato con il ColorPicker. ColorPicker imposta il colore di sfondo del controllo sul colore attualmente selezionato.

[![visualizzare la finestra di dialogo di selezione colori con un pulsante](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: visualizzazione della finestra di dialogo di selezione colori con un pulsante ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-colorpicker-control-extender-vb/_static/image10.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come usare ColorPicker Control Extender per visualizzare una finestra di dialogo di selezione dei colori popup. In primo luogo, è stato esaminato come è possibile visualizzare la finestra di dialogo quando lo stato attivo viene spostato in un controllo TextBox. Si è quindi appreso come creare un pulsante che visualizza la finestra di dialogo Selezione colori quando si fa clic sul pulsante.

> [!div class="step-by-step"]
> [Precedente](using-the-colorpicker-control-extender-cs.md)
