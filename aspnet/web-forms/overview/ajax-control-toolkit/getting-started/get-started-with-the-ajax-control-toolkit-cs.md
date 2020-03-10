---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introduzione a AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Scopri tutte le informazioni necessarie per iniziare a usare AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621604"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a>Introduzione ad AJAX Control Toolkit (C#)

[Microsoft](https://github.com/microsoft)

> Scopri tutte le informazioni necessarie per iniziare a usare AJAX Control Toolkit.

AJAX Control Toolkit contiene più di 30 controlli gratuiti che è possibile usare nelle applicazioni ASP.NET. In questa esercitazione si apprenderà come scaricare AJAX Control Toolkit e aggiungere i controlli del Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Download di AJAX Control Toolkit

[AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della community di ASP.NET e dal team di ASP.NET. 

[![download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: download di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))

Dopo aver scaricato il file, è necessario sbloccare il file. Fare clic con il pulsante destro del mouse sul file, scegliere Proprietà e fare clic sul pulsante **Sblocca** (vedere la figura 2).

[![sbloccare il file ZIP AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: sblocco del file zip di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))

Dopo aver sbloccato il file, è possibile decomprimere il file: fare clic con il pulsante destro del mouse sul file e selezionare l'opzione di menu **Estrai tutto** . A questo punto, è possibile aggiungere il Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Aggiunta di AJAX Control Toolkit alla casella degli strumenti

Il modo più semplice per utilizzare AJAX Control Toolkit consiste nell'aggiungere il Toolkit alla casella degli strumenti di Visual Studio/Visual Web Developer (vedere la figura 3). In questo modo, è possibile trascinare semplicemente un controllo Toolkit in una pagina quando si desidera utilizzarlo.

[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: AJAX Control Toolkit viene visualizzato nella casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))

Per prima cosa, è necessario aggiungere una scheda AJAX Control Toolkit alla casella degli strumenti. Seguire questa procedura.

1. Creare un nuovo sito Web ASP.NET selezionando l'opzione di menu file, nuovo sito Web. Fare doppio clic su default. aspx nella finestra Esplora soluzioni per aprire il file nell'editor.
2. Fare clic con il pulsante destro del mouse sulla casella degli strumenti sotto la scheda generale e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).
3. Immettere una nuova scheda denominata AJAX Control Toolkit.

[![l'aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: aggiunta di una nuova scheda ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))

Successivamente, è necessario aggiungere i controlli AJAX Control Toolkit alla nuova scheda. attenersi alla procedura seguente:

- Fare clic con il pulsante destro del mouse sotto la scheda AJAX Control Toolkit e selezionare l'opzione di menu **Choose Items (vedere la figura 5)** .
- Passare al percorso in cui è stato decompresso AJAX Control Toolkit e selezionare l'assembly AjaxControlToolkit. dll.

[![scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: scegliere gli elementi da aggiungere alla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))

Dopo aver completato questi passaggi, tutti i controlli del Toolkit verranno visualizzati nella casella degli strumenti.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Aggiornamento a una nuova versione del Toolkit

Se si usa una versione precedente del Toolkit e ora è necessario passare a una versione successiva, di seguito sono riportati i passaggi consigliati:

- Binari: eliminare la versione precedente dell'assembly AjaxControlToolkit. dll dalla cartella bin del sito Web.
- Elementi della casella degli strumenti: eliminare la scheda AJAX Control Toolkit e seguire i passaggi precedenti per ricreare la scheda con la nuova versione dell'assembly AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [avanti](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
