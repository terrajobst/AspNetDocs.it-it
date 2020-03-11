---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Creazione di un controllo Extender AJAX Control Toolkit personalizzatoC#() | Microsoft Docs
author: microsoft
description: I dispositivi Extender personalizzati consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 7850e745f5985688c95fc7f649ccbb06b2f66e20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535490"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Creazione di un controllo Extender di AJAX Control Toolkit personalizzato (C#)

[Microsoft](https://github.com/microsoft)

> I dispositivi Extender personalizzati consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.

In questa esercitazione si apprenderà come creare un controllo Extender AJAX Control Toolkit personalizzato. Viene creato un Extender semplice, ma utile, nuovo che modifica lo stato di un pulsante da Disabled a Enabled quando si digita il testo in una casella di testo. Dopo aver letto questa esercitazione, sarà possibile estendere il toolkit ASP.NET AJAX con i propri Extender di controllo.

È possibile creare estensioni di controllo personalizzate utilizzando Visual Studio o Visual Web Developer (assicurarsi di disporre della versione più recente di Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Panoramica del dispositivo Extender DisabledButton

Il nuovo dispositivo Extender di controllo è denominato DisabledButton Extender. Questa estensione avrà tre proprietà:

- TargetControlID: casella di testo estesa dal controllo.
- TargetButtonIID: pulsante disabilitato o abilitato.
- DisabledText: testo inizialmente visualizzato nel pulsante. Quando si inizia a digitare, il pulsante Visualizza il valore della proprietà testo pulsante.

Il dispositivo Extender DisabledButton viene collegato a un controllo TextBox e Button. Prima di digitare il testo, il pulsante è disabilitato e la casella di testo e il pulsante hanno un aspetto simile al seguente:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))

Dopo aver iniziato a digitare il testo, il pulsante è abilitato e la casella di testo e il pulsante hanno un aspetto simile al seguente:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))

Per creare il controllo Extender, è necessario creare i tre file seguenti:

- DisabledButtonExtender.cs: questo file è la classe del controllo lato server che gestirà la creazione del dispositivo Extender e consente di impostare le proprietà in fase di progettazione. Definisce inoltre le proprietà che è possibile impostare nel dispositivo Extender. Queste proprietà sono accessibili tramite codice e in fase di progettazione e corrispondono alle proprietà definite nel file DisableButtonBehavior. js.
- DisabledButtonBehavior. js: questo file è il punto in cui verrà aggiunta tutta la logica dello script client.
- DisabledButtonDesigner.cs: questa classe Abilita la funzionalità in fase di progettazione. Questa classe è necessaria se si desidera che il controllo Extender funzioni correttamente con Visual Studio/Visual Web Developer Designer.

Un controllo Extender è quindi costituito da un controllo lato server, da un comportamento lato client e da una classe della finestra di progettazione sul lato server. Si apprenderà come creare tutti e tre questi file nelle sezioni riportate di seguito.

## <a name="creating-the-custom-extender-website-and-project"></a>Creazione del sito Web e del progetto Extender personalizzati

Il primo passaggio consiste nel creare un progetto libreria di classi e un sito Web in Visual Studio/Visual Web Developer. Il dispositivo Extender personalizzato viene creato nel progetto di libreria di classi e viene testato il dispositivo Extender personalizzato nel sito Web.

Inizia con il sito Web. Per creare il sito Web, attenersi alla procedura seguente:

1. Selezionare il file dell'opzione **di menu nuovo sito Web**.
2. Selezionare il modello di **sito Web ASP.NET** .
3. Assegnare al nuovo sito Web il nome *WebSite1*.
4. Fare clic sul pulsante **OK** .

Successivamente, è necessario creare il progetto di libreria di classi che conterrà il codice per l'estensione del controllo:

1. Selezionare l'opzione di menu **file, Aggiungi, nuovo progetto**.
2. Selezionare il modello **libreria di classi** .
3. Denominare la nuova libreria di classi con il nome **CustomExtenders**.
4. Fare clic sul pulsante **OK** .

Dopo aver completato questi passaggi, la finestra di Esplora soluzioni avrà un aspetto simile a quello illustrato nella figura 1.

[![soluzione con il progetto di libreria di classi e siti Web](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Figura 01**: soluzione con sito Web e progetto libreria di classi ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))

Successivamente, è necessario aggiungere tutti i riferimenti ad assembly necessari al progetto di libreria di classi:

1. Fare clic con il pulsante destro del mouse sul progetto CustomExtenders e selezionare l'opzione di menu **Aggiungi riferimento**.
2. Selezionare la scheda .NET.
3. Aggiungere riferimenti agli assembly riportati di seguito:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selezionare la scheda Sfoglia.
5. Aggiungere un riferimento all'assembly AjaxControlToolkit. dll. Questo assembly si trova nella cartella in cui è stato scaricato AJAX Control Toolkit.

Dopo aver completato questi passaggi, la cartella riferimenti al progetto di libreria di classi sarà simile alla figura 2.

[![la cartella riferimenti con i riferimenti necessari](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Figura 02**: fare riferimento alla cartella con i riferimenti richiesti ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Creazione dell'estensione del controllo personalizzato

Ora che la libreria di classi è disponibile, è possibile iniziare a creare il controllo Extender. Inizia con il Bare Bones di una classe del controllo Extender personalizzata (vedere il listato 1).

**Listato 1-MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Esistono diversi aspetti che si notano sulla classe di controllo Extender nel listato 1. Si noti prima di tutto che la classe eredita dalla classe di base ExtenderControlBase. Tutti i controlli Extender di AJAX Control Toolkit derivano da questa classe base. La classe di base, ad esempio, include la proprietà TargetID che è una proprietà obbligatoria di ogni Extender del controllo.

Si noti quindi che la classe include i due attributi seguenti correlati allo script client:

- WebResource: comporta l'inclusione di un file come risorsa incorporata in un assembly.
- ClientScriptResource: consente di recuperare una risorsa script da un assembly.

L'attributo WebResource viene usato per incorporare il file JavaScript MyControlBehavior. js nell'assembly quando viene compilato il dispositivo Extender personalizzato. L'attributo ClientScriptResource viene usato per recuperare lo script MyControlBehavior. js dall'assembly quando l'estensione personalizzata viene usata in una pagina Web.

Affinché gli attributi WebResource e ClientScriptResource funzionino, è necessario compilare il file JavaScript come risorsa incorporata. Selezionare il file nella finestra di Esplora soluzioni, aprire la finestra delle proprietà e assegnare la *risorsa incorporata* value alla proprietà **azione di compilazione** .

Si noti che l'estensione del controllo include anche un attributo TargetControlType. Questo attributo viene usato per specificare il tipo di controllo esteso dall'estensione del controllo. Nel caso del listato 1, l'estensione del controllo viene usata per estendere una casella di testo.

Infine, si noti che il dispositivo Extender personalizzato include una proprietà denominata SetProperty. La proprietà è contrassegnata con l'attributo ExtenderControlProperty. I metodi GetPropertyValue () e sepropertyvalue () vengono usati per passare il valore della proprietà dall'Extender del controllo lato server al comportamento sul lato client.

Si procederà quindi all'implementazione del codice per il dispositivo Extender DisabledButton. Il codice per questo dispositivo Extender è disponibile nel listato 2.

**Listato 2-DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Il dispositivo Extender DisabledButton nel listato 2 presenta due proprietà denominate TargetButtonID e DisabledText. La IDReferenceProperty applicata alla proprietà TargetButtonID impedisce di assegnare a questa proprietà un valore diverso dall'ID di un controllo Button.

Gli attributi WebResource e ClientScriptResource associano un comportamento sul lato client che si trova in un file denominato DisabledButtonBehavior. js con questo Extender. Questo file JavaScript verrà illustrato nella sezione successiva.

## <a name="creating-the-custom-extender-behavior"></a>Creazione del comportamento Extender personalizzato

Il componente lato client di un'estensione di controllo è denominato comportamento. La logica effettiva per la disabilitazione e l'abilitazione del pulsante è contenuta nel comportamento DisabledButton. Il codice JavaScript per il comportamento è incluso nell'elenco 3.

**Listato 3-DisabledButton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Il file JavaScript nel listato 3 contiene una classe lato client denominata DisabledButtonBehavior. Questa classe, come il gemello sul lato server, include due proprietà denominate TargetButtonID e DisabledText a cui è possibile accedere usando Get\_TargetButtonID/set\_TargetButtonID e Get\_DisabledText/set\_DisabledText.

Il metodo Initialize () associa un gestore eventi KeyUp all'elemento target per il comportamento. Ogni volta che si digita una lettera nella casella di testo associata a questo comportamento, il gestore KeyUp viene eseguito. Il gestore KeyUp Abilita o Disabilita il pulsante a seconda che la casella di testo associata al comportamento contenga testo.

Tenere presente che è necessario compilare il file JavaScript nel listato 3 come risorsa incorporata. Selezionare il file nella finestra di Esplora soluzioni, aprire la finestra delle proprietà e assegnare la *risorsa incorporata* value alla proprietà **azione di compilazione** (vedere la figura 3). Questa opzione è disponibile sia in Visual Studio che in Visual Web Developer.

[![l'aggiunta di un file JavaScript come risorsa incorporata](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Figura 03**: aggiunta di un file JavaScript come risorsa incorporata ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Creazione della finestra di progettazione Extender personalizzata

Per completare l'estensione, è necessario creare un'ultima classe. È necessario creare la classe della finestra di progettazione nel listato 4. Questa classe è necessaria per rendere il comportamento appropriato del dispositivo Extender con Visual Studio/Visual Web Developer Designer.

**Listato 4-DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Associare la finestra di progettazione nel listato 4 al dispositivo Extender DisabledButton con l'attributo della finestra di progettazione. È necessario applicare l'attributo della finestra di progettazione alla classe DisabledButtonExtender come segue:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Uso del dispositivo Extender personalizzato

Ora che è stata completata la creazione del controllo Extender DisabledButton, è possibile usarlo nel sito Web di ASP.NET. Prima di tutto, è necessario aggiungere il dispositivo Extender personalizzato alla casella degli strumenti. Attenersi ai passaggi riportati di seguito.

1. Aprire una pagina di ASP.NET facendo doppio clic sulla pagina nella finestra Esplora soluzioni.
2. Fare clic con il pulsante destro del mouse sulla casella degli strumenti e selezionare l'opzione di menu **Scegli elementi**.
3. Nella finestra di dialogo Scegli elementi della casella degli strumenti individuare l'assembly CustomExtenders. dll.
4. Fare clic sul pulsante **OK** per chiudere la finestra di dialogo.

Dopo aver completato questi passaggi, l'Extender del controllo DisabledButton dovrebbe essere visualizzato nella casella degli strumenti (vedere la figura 4).

[![DisabledButton nella casella degli strumenti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Figura 04**: DisabledButton nella casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))

Successivamente, è necessario creare una nuova pagina ASP.NET. Attenersi ai passaggi riportati di seguito.

1. Creare una nuova pagina ASP.NET denominata ShowDisabledButton. aspx.
2. Trascinare un ScriptManager nella pagina.
3. Trascinare un controllo TextBox nella pagina.
4. Trascinare un controllo Button nella pagina.
5. Nella Finestra Proprietà impostare la proprietà Button ID sul valore <em>btnSave</em> e la proprietà Text sul valore *Save\** .

È stata creata una pagina con una casella di testo ASP.NET standard e un controllo Button.

Successivamente, è necessario estendere il controllo TextBox con il dispositivo Extender DisabledButton:

1. Selezionare l'opzione **Aggiungi attività Extender** per aprire la finestra di dialogo della procedura guidata Extender (vedere la figura 5). Si noti che la finestra di dialogo include il dispositivo Extender DisabledButton personalizzato.
2. Selezionare il dispositivo Extender DisabledButton e fare clic sul pulsante **OK** .

[![finestra di dialogo della procedura guidata Extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Figura 05**: finestra di dialogo della procedura guidata Extender ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))

Infine, è possibile impostare le proprietà dell'estensione DisabledButton. È possibile modificare le proprietà del dispositivo Extender DisabledButton modificando le proprietà del controllo TextBox:

1. Consente di selezionare la casella di testo nella finestra di progettazione.
2. Nel Finestra Proprietà espandere il nodo Extender (vedere la figura 6).
3. Assegnare il valore *Save* alla proprietà DisabledText e il valore *BtnSave* alla proprietà TargetButtonID.

[Impostazione ![proprietà Extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Figura 06**: impostazione delle proprietà di Extender ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))

Quando si esegue la pagina (premendo F5), il controllo Button viene inizialmente disabilitato. Non appena si inizia a immettere il testo nella casella di testo, il controllo Button è abilitato (vedere la figura 7).

[![il dispositivo Extender DisabledButton in azione](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Figura 07**: estensione DisabledButton in azione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è spiegare come estendere AJAX Control Toolkit con controlli Extender personalizzati. In questa esercitazione è stata creata una semplice estensione del controllo DisabledButton. Questa estensione è stata implementata creando una classe DisabledButtonExtender, un comportamento JavaScript DisabledButtonBehavior e una classe DisabledButtonDesigner. Si segue un set di passaggi analogo ogni volta che si crea un'estensione del controllo personalizzata.

> [!div class="step-by-step"]
> [Precedente](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Successivo](get-started-with-the-ajax-control-toolkit-vb.md)
