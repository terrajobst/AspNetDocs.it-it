---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Ricerca per categorie usare il controllo editor HTML? (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor è un controllo AJAX ASP.NET che consente di creare e modificare facilmente contenuto HTML tramite pulsanti in una barra degli strumenti.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 20f8a2f8148bc658370ba1a939ebf1b62d376bc0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554152"
---
# <a name="how-do-i-use-the-html-editor-control-vb"></a>Ricerca per categorie usare il controllo editor HTML? (VB)

[Microsoft](https://github.com/microsoft)

> HTMLEditor è un controllo AJAX ASP.NET che consente di creare e modificare facilmente contenuto HTML tramite pulsanti in una barra degli strumenti.

L'obiettivo di questa esercitazione è fornire una panoramica del controllo dell'editor HTML incluso in AJAX Control Toolkit. Nell'editor HTML sono incluse opzioni per modificare le dimensioni del carattere, selezionare un tipo di carattere, modificare il colore di sfondo, modificare il colore di primo piano, aggiungere collegamenti, aggiungere immagini, modificare l'allineamento del testo ed eseguire operazioni Taglia, copia e incolla (vedere la figura 1).

[![editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: editor HTML ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-html-editor-control-vb/_static/image2.png))

L'editor HTML consente di immettere il contenuto usando una modalità di progettazione oppure di immettere direttamente il codice HTML. Viene anche fornito l'opzione per visualizzare in anteprima il contenuto HTML (vedere la figura 2).

[pulsanti di progettazione ![, HTML e anteprima](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: progettazione, HTML e pulsanti di anteprima ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-html-editor-control-vb/_static/image4.png))

In questa esercitazione si apprenderà come visualizzare l'editor HTML, come personalizzare i pulsanti della barra degli strumenti visualizzati nell'editor HTML e come evitare gli attacchi di scripting tra siti.

## <a name="displaying-the-html-editor"></a>Visualizzazione dell'editor HTML

Prima di poter utilizzare l'editor HTML in una pagina ASP.NET, è necessario innanzitutto aggiungere un controllo ScriptManager alla pagina. Il controllo ScriptManager si trova sotto la scheda Estensioni AJAX della casella degli strumenti di Visual Studio/Visual Web Developer Express.

È necessario inserire il controllo ScriptManager nella parte superiore della pagina prima di qualsiasi altro controllo nella pagina. Ad esempio, è possibile posizionarlo immediatamente al di sotto del modulo di apertura &lt;lato server&gt; tag.

Il controllo dell'editor HTML si trova nella casella degli strumenti con il resto dei controlli AJAX Control Toolkit. Il nome è il controllo dell'editor (vedere la figura 3).

[![il controllo editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: controllo dell'editor HTML ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-html-editor-control-vb/_static/image6.png))

Dopo aver trascinato l'editor HTML in una pagina, è possibile impostarne le proprietà nella finestra delle proprietà. Ad esempio, in genere si desidera impostare le proprietà Width e Height. Il listato 1 contiene l'origine di una pagina ASP.NET che contiene un editor HTML.

**Listato 1-SimpleEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

Nella pagina del listato 1 sono contenuti un controllo dell'editor HTML, un controllo Button e un controllo Literal. Quando si fa clic sul pulsante, il contenuto dell'editor HTML viene visualizzato nel controllo Literal (vedere la figura 4).

[![l'invio di un modulo con un editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: invio di un modulo con un editor HTML ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-html-editor-control-vb/_static/image8.png))

La proprietà Content dell'editor HTML viene utilizzata per recuperare il contenuto HTML immesso nell'editor HTML. Tenere presente che questo contenuto HTML può contenere JavaScript. Nella sezione successiva viene illustrato come impedire gli attacchi injection JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizzazione della barra degli strumenti dell'editor HTML

È possibile personalizzare esattamente i pulsanti visualizzati nell'editor. Ad esempio, potrebbe essere necessario rimuovere la scheda HTML per impedire agli utenti di passare dall'editor HTML alla modalità HTML. In alternativa, potrebbe essere necessario rimuovere l'elenco a discesa Dimensioni carattere per impedire agli utenti di creare testo eccessivamente grande in un post di messaggio del forum (vedere la figura 5).

[![un editor HTML personalizzato](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: un editor HTML personalizzato ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-html-editor-control-vb/_static/image10.png))

Per personalizzare i pulsanti della barra degli strumenti, derivare un nuovo editor HTML dalla classe editor di base. Ad esempio, l'editor personalizzato nel listato 2 contiene solo pulsanti della barra degli strumenti per grassetto e corsivo. Tutti gli altri pulsanti della barra degli strumenti sono stati rimossi. Inoltre, la scheda HTML è stata rimossa dalla parte inferiore dell'editor (ma le schede Progettazione e anteprima sono ancora presenti).

**Listato 2-app\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

È necessario aggiungere la classe nel listato 2 all'app\_cartella del codice in modo che la classe venga compilata automaticamente. Se la cartella del codice dell'app\_non esiste nel sito Web, è possibile semplicemente aggiungere la cartella.

Dopo aver creato un editor personalizzato, è possibile aggiungerlo a una pagina ASP.NET nello stesso modo in cui si aggiunge il normale editor HTML (vedere l'elenco 3).

**Listato 3-ShowCustomEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Prevenzione degli attacchi tramite scripting tra siti (XSS)

Ogni volta che si accetta l'input da un utente e si visualizza nuovamente tale input nel sito Web, si apre potenzialmente il sito Web per gli attacchi di scripting (XSS) tra siti. In teoria, un pirata informatico può inviare codice JavaScript che viene eseguito quando viene nuovamente visualizzato l'input. Il codice JavaScript può essere usato per rubare le password utente o altre informazioni riservate.

In genere, è possibile sconfiggere gli attacchi XSS dalla codifica HTML indipendentemente dall'input recuperato da un utente prima di visualizzarlo in una pagina Web. Tuttavia, la codifica HTML dell'output dell'editor HTML non solo codifica &lt;script&gt; tag, ma codifica anche tutti i tag HTML. In altre parole, si perde tutta la formattazione, ad esempio il tipo di carattere, le dimensioni del carattere e il colore di sfondo.

Se si raccolgono informazioni riservate dagli utenti, ad esempio password, numeri di carta di credito e codici fiscali, non è necessario visualizzare contenuto non codificato recuperato da un utente con l'editor HTML. È necessario usare l'editor HTML solo nelle situazioni in cui non si sta rivisualizzando il contenuto HTML oppure il contenuto HTML viene inviato al sito Web da parte di un utente attendibile.

Si supponga, ad esempio, di creare un'applicazione di Blog. In questa situazione è opportuno utilizzare l'editor HTML durante la composizione dei post di Blog. L'unico utente che invia un post di Blog e, presumibilmente, può fidarsi di non inviare JavaScript dannoso. Tuttavia, non ha senso utilizzare l'editor HTML quando si consente agli utenti anonimi di pubblicare commenti. È necessario prestare particolare attenzione nelle situazioni in cui gli utenti inviano informazioni riservate, ad esempio le password. Potenzialmente, un utente malintenzionato potrebbe inviare un commento che contiene il codice JavaScript corretto per rubare una password.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata fornita una breve panoramica del controllo editor HTML incluso in AJAX Control Toolkit. Si è appreso come usare l'editor HTML per accettare contenuti avanzati da un utente e inviare il contenuto al server. È stato inoltre illustrato come personalizzare i pulsanti della barra degli strumenti visualizzati nell'editor HTML. Infine, si è appreso come evitare gli attacchi di scripting tra siti quando si usa l'editor HTML per accettare input potenzialmente dannosi.

> [!div class="step-by-step"]
> [Precedente](how-do-i-use-the-html-editor-control-cs.md)
