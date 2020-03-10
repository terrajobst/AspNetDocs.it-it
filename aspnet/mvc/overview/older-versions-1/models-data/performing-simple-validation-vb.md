---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Esecuzione di una convalida semplice (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione, Stephen Walther introduce lo stato del modello e l'helper HTML di convalida...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 46925f22b7dfc23f2bb89b8d2fff0cbd8ae49062
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542931"
---
# <a name="performing-simple-validation-vb"></a>Esecuzione di una convalida semplice (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione Stephen Walther introduce lo stato del modello e gli helper HTML di convalida.

L'obiettivo di questa esercitazione è spiegare come è possibile eseguire la convalida in un'applicazione MVC ASP.NET. Si apprenderà ad esempio come impedire a un utente di inviare un modulo che non contiene un valore per un campo obbligatorio. Si apprenderà come usare lo stato del modello e gli helper HTML di convalida.

## <a name="understanding-model-state"></a>Informazioni sullo stato del modello

Si usa lo stato del modello, o più accuratamente, il dizionario di stato del modello per rappresentare gli errori di convalida. Ad esempio, l'azione crea () nel listato 1 convalida le proprietà di una classe prodotto prima di aggiungere la classe prodotto a un database.

Non è consigliabile aggiungere la logica di convalida o di database a un controller. Un controller deve contenere solo la logica correlata al controllo del flusso dell'applicazione. È in corso un collegamento per semplificare le operazioni.

**Listato 1-Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Nel listato 1, vengono convalidate le proprietà Name, Description e UnitsInStock della classe Product. Se una di queste proprietà ha esito negativo di un test di convalida, viene aggiunto un errore al dizionario di stato del modello (rappresentato dalla proprietà ModelState della classe controller).

Se si verificano errori nello stato del modello, la proprietà ModelState. IsValid restituisce false. In tal caso, viene visualizzato nuovamente il formato HTML per la creazione di un nuovo prodotto. In caso contrario, se non sono presenti errori di convalida, il nuovo prodotto verrà aggiunto al database.

## <a name="using-the-validation-helpers"></a>Utilizzo degli helper di convalida

Il framework ASP.NET MVC include due helper di convalida: l'helper HTML. ValidationMessage () e l'helper HTML. ValidationSummary (). Usare questi due helper in una visualizzazione per visualizzare i messaggi di errore di convalida.

Gli helper HTML. ValidationMessage () e HTML. ValidationSummary () vengono usati nelle visualizzazioni create e Edit generate automaticamente da ASP.NET MVC impalcature. Seguire questa procedura per generare la visualizzazione di creazione:

1. Fare clic con il pulsante destro del mouse sull'azione crea () nel controller del prodotto e selezionare l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 1).
2. Nella finestra di dialogo **Aggiungi visualizzazione** selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata** (vedere la figura 2).
3. Dall'elenco a discesa della **classe di dati View** selezionare la classe Product.
4. Dall'elenco a discesa **Visualizza contenuto** selezionare Crea.
5. Fare clic sul pulsante **Aggiungi**.

Assicurarsi di compilare l'applicazione prima di aggiungere una vista. In caso contrario, l'elenco delle classi non verrà visualizzato nell'elenco a discesa della **classe di dati View** .

[![finestra di dialogo nuovo progetto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: aggiunta di una visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](performing-simple-validation-vb/_static/image2.png))

[![finestra di dialogo nuovo progetto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: creazione di una visualizzazione fortemente tipizzata ([fare clic per visualizzare l'immagine con dimensioni complete](performing-simple-validation-vb/_static/image4.png))

Dopo aver completato questi passaggi, è possibile ottenere la visualizzazione create nel listato 2.

**Listato 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Nel listato 2, l'helper HTML. ValidationSummary () viene chiamato immediatamente sopra il form HTML. Questo helper viene utilizzato per visualizzare un elenco di messaggi di errore di convalida. L'helper HTML. ValidationSummary () esegue il rendering degli errori in un elenco puntato.

L'helper HTML. ValidationMessage () viene chiamato accanto a ognuno dei campi del form HTML. Questo helper viene usato per visualizzare un messaggio di errore accanto a un campo del modulo. Nel caso del listato 2, l'helper HTML. ValidationMessage () Visualizza un asterisco quando si verifica un errore.

La pagina nella figura 3 illustra i messaggi di errore di cui è stato eseguito il rendering dagli helper di convalida quando il modulo viene inviato con campi mancanti e valori non validi.

[![finestra di dialogo nuovo progetto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: creazione della vista inviata con problemi ([fare clic per visualizzare l'immagine con dimensioni complete](performing-simple-validation-vb/_static/image6.png))

Si noti che l'aspetto dei campi di input HTML viene modificato anche quando si verifica un errore di convalida. L'helper HTML. TextBox () esegue il rendering di un attributo *Class = "Input-Validation-Error"* quando si verifica un errore di convalida associato alla proprietà sottoposta a rendering dall'helper HTML. TextBox ().

Per controllare l'aspetto degli errori di convalida sono disponibili tre classi di fogli di stile CSS:

- Input-Validation-Error-applicato al tag di input&gt; di &lt;sottoposto a rendering da HTML. TextBox () helper.
- Field-Validation-Error-applicato al &lt;span&gt; tag sottoposto a rendering dall'helper HTML. ValidationMessage ().
- validation-summary-errors-applicato al tag di&gt; UL di &lt;sottoposto a rendering dall'helper HTML. ValidationSummary ().

È possibile modificare le classi del foglio di stile CSS e quindi modificare l'aspetto degli errori di convalida modificando il file site. CSS che si trova nella cartella Content.

> [!NOTE] 
> 
> La classe HtmlHelper include proprietà statiche di sola lettura per il recupero dei nomi delle classi CSS correlate alla convalida. Queste proprietà statiche sono denominate ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.

## <a name="prebinding-validation-and-postbinding-validation"></a>Convalida prebinding e convalida di post-associazione

Se si invia il modulo HTML per la creazione di un prodotto e si immette un valore non valido per il campo Price e nessun valore per il campo UnitsInStock, si otterranno i messaggi di convalida visualizzati nella figura 4. Da dove provengono i messaggi di errore di convalida?

[![finestra di dialogo nuovo progetto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: errori di convalida del prebinding ([fare clic per visualizzare l'immagine con dimensioni complete](performing-simple-validation-vb/_static/image8.png))

Esistono effettivamente due tipi di messaggi di errore di convalida, quelli generati prima che i campi del modulo HTML siano associati a una classe e quelli generati dopo che i campi del modulo sono associati alla classe. In altre parole, sono presenti errori di convalida prebinding ed errori di convalida di post-binding.

L'azione Create () esposta dal controller del prodotto nel listato 1 accetta un'istanza della classe Product. La firma del metodo Create ha un aspetto simile al seguente:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

I valori dei campi del form HTML dal modulo di creazione sono associati alla classe productToCreate da un elemento denominato strumento di associazione di modelli. Lo strumento di associazione di modelli predefinito aggiunge automaticamente un messaggio di errore allo stato del modello quando non è in grado di associare un campo del modulo a una proprietà Form.

Lo strumento di associazione di modelli predefinito non è in grado di associare la stringa "Apple" alla proprietà price della classe Product. Non è possibile assegnare una stringa a una proprietà Decimal. Pertanto, lo strumento di associazione di modelli aggiunge un errore allo stato del modello.

Lo strumento di associazione di modelli predefinito non è inoltre in grado di assegnare il valore a una proprietà che non accetta alcun valore. In particolare, lo strumento di associazione di modelli non può assegnare alcun valore alla proprietà UnitsInStock. Ancora una volta, lo strumento di associazione di modelli consente di aggiungere un messaggio di errore allo stato del modello.

Se si desidera personalizzare l'aspetto dei messaggi di errore di preassociazione, è necessario creare stringhe di risorse per questi messaggi.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è descrivere i meccanismi di convalida di base in ASP.NET MVC Framework. Si è appreso come usare lo stato del modello e gli helper HTML di convalida. È stata inoltre illustrata la differenza tra la convalida del prebinding e l'associazione dell'associazione. In altre esercitazioni verranno illustrate diverse strategie per lo stato di trasferimento del codice di convalida dai controller e dalle classi del modello.

> [!div class="step-by-step"]
> [Precedente](displaying-a-table-of-database-data-vb.md)
> [Successivo](validating-with-the-idataerrorinfo-interface-vb.md)
