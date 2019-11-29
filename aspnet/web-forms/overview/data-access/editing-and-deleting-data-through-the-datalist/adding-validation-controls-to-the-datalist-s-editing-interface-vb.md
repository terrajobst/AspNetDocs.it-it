---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Aggiunta di controlli di convalida all'interfaccia di modifica di DataList (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida ai EditItemTemplate di DataList per fornire un utente di modifica più infallibile...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: f952a7bb95e956a2ad935f8bdef5c3efa7437ecb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621841"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Aggiunta di controlli di convalida all'interfaccia di modifica di DataList (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) o [scaricare il file PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida ai EditItemTemplate di DataList per fornire un'interfaccia utente di modifica più infallibile.

## <a name="introduction"></a>Introduzione

Nelle esercitazioni di modifica di DataList finora, le interfacce di modifica di datalists non includono la convalida dell'input utente proattiva anche se l'input utente non valido, ad esempio un nome di prodotto mancante o un prezzo negativo, genera un'eccezione. Nell' [esercitazione precedente](handling-bll-and-dal-level-exceptions-vb.md) è stato esaminato come aggiungere il codice di gestione delle eccezioni al gestore eventi `UpdateCommand` DataList per intercettare e visualizzare normalmente le informazioni sulle eccezioni generate. Idealmente, tuttavia, l'interfaccia di modifica includerà i controlli di convalida per impedire a un utente di immettere tali dati non validi nella prima posizione.

In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida al `EditItemTemplate` DataList per fornire un'interfaccia utente di modifica più infallibile. In particolare, questa esercitazione prende in considerazione l'esempio creato nell'esercitazione precedente e aumenta l'interfaccia di modifica per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Passaggio 1: replica dell'esempio dalla[gestione delle eccezioni a livello BLL e dal](handling-bll-and-dal-level-exceptions-vb.md)

Nell'esercitazione relativa alla [gestione delle eccezioni di livello BLL e dal](handling-bll-and-dal-level-exceptions-vb.md) è stata creata una pagina che elenca i nomi e i prezzi dei prodotti in un oggetto DataList modificabile a due colonne. L'obiettivo di questa esercitazione è quello di aumentare l'interfaccia di modifica di DataList per includere i controlli di convalida. In particolare, la logica di convalida:

- Richiedi l'inclusione del nome del prodotto
- Verificare che il valore immesso per il prezzo sia un formato di valuta valido
- Verificare che il valore immesso per il prezzo sia maggiore o uguale a zero, perché un valore `UnitPrice` negativo non è valido

Prima di poter esaminare l'aumento dell'esempio precedente per includere la convalida, è prima di tutto necessario replicare l'esempio dalla pagina `ErrorHandling.aspx` nella cartella `EditDeleteDataList` alla pagina per questa esercitazione `UIValidation.aspx`. Per ottenere questo risultato, è necessario copiare sia il markup dichiarativo della pagina `ErrorHandling.aspx` che il relativo codice sorgente. Prima di tutto, copiare il markup dichiarativo attenendosi alla procedura seguente:

1. Aprire la pagina `ErrorHandling.aspx` in Visual Studio
2. Passare al markup dichiarativo della pagina. fare clic sul pulsante origine nella parte inferiore della pagina.
3. Copiare il testo all'interno dei tag `<asp:Content>` e `</asp:Content>` (righe da 3 a 32), come illustrato nella figura 1.

[![copiare il testo nel controllo &lt;ASP: content&gt;](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 2**: copiare il testo all'interno del controllo `<asp:Content>` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))

1. Aprire la pagina `UIValidation.aspx`
2. Vai al markup dichiarativo della pagina
3. Incollare il testo all'interno del controllo `<asp:Content>`.

Per copiare il codice sorgente, aprire la pagina `ErrorHandling.aspx.vb` e copiare solo il testo *all'interno* della classe `EditDeleteDataList_ErrorHandling`. Copiare i tre gestori eventi (`Products_EditCommand`, `Products_CancelCommand`e `Products_UpdateCommand`) insieme al metodo `DisplayExceptionDetails`, ma **non** copiare la dichiarazione della classe o le istruzioni di `using`. Incollare il testo copiato *all'interno* della classe `EditDeleteDataList_UIValidation` in `UIValidation.aspx.vb`.

Dopo aver spostato il contenuto e il codice da `ErrorHandling.aspx` a `UIValidation.aspx`, provare a eseguire il test delle pagine in un browser. Verrà visualizzato lo stesso output e si verificherà la stessa funzionalità in ognuna di queste due pagine (vedere la figura 2).

[![pagina UIValidation. aspx riproduce la funzionalità in ErrorHandling. aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: la pagina `UIValidation.aspx` simula la funzionalità `ErrorHandling.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Passaggio 2: aggiunta dei controlli di convalida a DataList s EditItemTemplate

Quando si creano form di immissione dati, è importante che gli utenti immettano tutti i campi obbligatori e che tutti gli input specificati siano valori validi e formattati correttamente. Per garantire la validità degli input di un utente, ASP.NET fornisce cinque controlli di convalida predefiniti progettati per convalidare il valore di un singolo controllo Web di input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantisce che sia stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida un valore rispetto a un altro valore di controllo Web o a un valore costante oppure garantisce che il formato del valore sia valido per un tipo di dati specificato.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantisce che un valore si trovi all'interno di un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida un valore rispetto a un' [espressione regolare](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida un valore rispetto a un metodo personalizzato definito dall'utente

Per ulteriori informazioni su questi cinque controlli, vedere l'esercitazione [aggiunta di controlli di convalida all'esercitazione sulle interfacce di modifica e inserimento](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) oppure consultare la [sezione controlli di convalida](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) delle [esercitazioni introduttive di ASP.NET](https://quickstarts.asp.net).

Per questa esercitazione è necessario usare un RequiredFieldValidator per assicurarsi che sia stato fornito un valore per il nome del prodotto e un oggetto CompareValidator per garantire che il prezzo immesso abbia un valore maggiore o uguale a 0 e venga presentato in un formato di valuta valido.

> [!NOTE]
> Mentre ASP.NET 1. x aveva questi stessi cinque controlli di convalida, ASP.NET 2,0 ha aggiunto diversi miglioramenti, i due principali sono il supporto di script lato client per i browser oltre a Internet Explorer e la possibilità di partizionare i controlli di convalida in una pagina in gruppi di convalida. Per ulteriori informazioni sulle nuove funzionalità di controllo di convalida in 2,0, vedere l'articolo relativo alla [dissezione dei controlli di convalida in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Per iniziare, aggiungere i controlli di convalida necessari a DataList s `EditItemTemplate`. Questa attività può essere eseguita tramite la finestra di progettazione facendo clic sul collegamento modifica modelli dallo smart tag di DataList o tramite la sintassi dichiarativa. Consente di eseguire il processo utilizzando l'opzione modifica modelli dalla visualizzazione progettazione. Dopo aver scelto di modificare il `EditItemTemplate`di DataList, aggiungerne uno trascinandoli dalla casella degli strumenti all'interfaccia di modifica del modello, inserendolo successivamente alla casella di testo `ProductName`.

[![aggiungere un oggetto RequiredFieldValidator a EditItemTemplate dopo la casella di testo ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: aggiungere un RequiredFieldValidator al `EditItemTemplate After` casella di testo `ProductName` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))

Tutti i controlli di convalida funzionano convalidando l'input di un singolo controllo Web ASP.NET. Pertanto, è necessario indicare che il RequiredFieldValidator appena aggiunto deve essere convalidato in base alla casella di testo `ProductName`; Questa operazione viene eseguita impostando la [proprietà`ControlToValidate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) del controllo di convalida sul `ID` del controllo Web appropriato (`ProductName`in questa istanza). Impostare quindi la [proprietà`ErrorMessage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) su è necessario fornire il nome del prodotto e la [proprietà`Text`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) per \*. Il valore della proprietà `Text`, se specificato, è il testo visualizzato dal controllo di convalida se la convalida ha esito negativo. Il valore della proprietà `ErrorMessage`, che è obbligatorio, viene utilizzato dal controllo ValidationSummary; Se il valore della proprietà `Text` viene omesso, il valore della proprietà `ErrorMessage` viene visualizzato dal controllo di convalida sull'input non valido.

Dopo aver impostato queste tre proprietà del RequiredFieldValidator, la schermata dovrebbe essere simile alla figura 4.

[![impostare le proprietà ControlToValidate, ErrorMessage e Text di RequiredFieldValidator](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: impostare le proprietà `ControlToValidate`, `ErrorMessage`e `Text` di RequiredFieldValidator ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))

Con il RequiredFieldValidator aggiunto al `EditItemTemplate`, resta solo aggiungere la convalida necessaria per la casella di testo Product s price. Poiché la `UnitPrice` è facoltativa quando si modifica un record, non è necessario aggiungere un RequiredFieldValidator. Tuttavia, è necessario aggiungere un oggetto CompareValidator per garantire che il `UnitPrice`, se specificato, sia formattato correttamente come valuta ed è maggiore o uguale a 0.

Aggiungere il CompareValidator all'`EditItemTemplate` e impostare la relativa proprietà `ControlToValidate` su `UnitPrice`, la relativa proprietà `ErrorMessage` sul prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta e la relativa proprietà `Text` su \*. Per indicare che il valore di `UnitPrice` deve essere maggiore o uguale a 0, impostare la [proprietà`Operator`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) CompareValidator s su `GreaterThanEqual`, la relativa [Proprietà`ValueToCompare`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) su 0 e la relativa [Proprietà`Type`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) su `Currency`.

Dopo l'aggiunta di questi due controlli di convalida, la sintassi dichiarativa di DataList s `EditItemTemplate` s dovrebbe essere simile alla seguente:

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Dopo avere apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o si immette un valore di prezzo non valido durante la modifica di un prodotto, accanto alla casella di testo viene visualizzato un asterisco. Come illustrato nella figura 5, un valore del prezzo che include il simbolo di valuta, ad esempio $19,95, viene considerato non valido. Il `Currency` CompareValidator s `Type` consente separatori di cifre, ad esempio virgole o punti, a seconda delle impostazioni cultura, e un segno più o meno, ma *non consente un* simbolo di valuta. Questo comportamento può avere perplessità sugli utenti perché l'interfaccia di modifica esegue attualmente il rendering del `UnitPrice` usando il formato di valuta.

[![viene visualizzato un asterisco accanto alle caselle di testo con input non valido](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: viene visualizzato un asterisco accanto alle caselle di testo con input non valido ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))

Mentre la convalida funziona così com'è, l'utente deve rimuovere manualmente il simbolo di valuta quando si modifica un record, il che non è accettabile. Inoltre, se sono presenti input non validi nell'interfaccia di modifica né i pulsanti Aggiorna né Annulla, quando si fa clic su un postback, viene richiamato un postback. Idealmente, il pulsante Annulla restituisce il DataList allo stato di pre-modifica indipendentemente dalla validità degli input dell'utente. Inoltre, è necessario assicurarsi che i dati della pagina siano validi prima di aggiornare le informazioni sul prodotto nel gestore dell'evento DataList s `UpdateCommand`, perché i controlli di convalida la logica lato client possono essere ignorati dagli utenti i cui browser non supportano JavaScript o che ne è stato disabilitato il supporto.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Rimozione del simbolo di valuta dalla casella di testo EditItemTemplate s PrezzoUnitario

Quando si usa il `Currency``Type`CompareValidator, l'input da convalidare non deve includere alcun simbolo di valuta. La presenza di tali simboli fa sì che il CompareValidator contrassegni l'input come non valido. Tuttavia, l'interfaccia di modifica include attualmente un simbolo di valuta nella casella di testo `UnitPrice`, il che significa che l'utente deve rimuovere in modo esplicito il simbolo di valuta prima di salvare le modifiche. Per risolvere questo problema, sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il valore della casella di testo `UnitPrice` non sia formattato come valuta.
2. Consente all'utente di immettere un simbolo di valuta rimuovendo il controllo CompareValidator e sostituendolo con un RegularExpressionValidator che verifica la presenza di un valore di valuta formattato correttamente. Il problema è che l'espressione regolare per convalidare un valore di valuta non è semplice come il CompareValidator e richiederebbe la scrittura di codice se si desiderasse incorporare le impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e basarsi sulla logica di convalida sul lato server personalizzata nel gestore dell'evento GridView s `RowUpdating`.

Per questa esercitazione, è possibile usare l'opzione 1. Attualmente il `UnitPrice` è formattato come valore di valuta a causa dell'espressione DataBinding per la casella di testo nella `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Modificare l'istruzione `Eval` in `Eval("UnitPrice", "{0:n2}")`, che formatta il risultato come numero con due cifre di precisione. Questa operazione può essere eseguita direttamente tramite la sintassi dichiarativa oppure facendo clic sul collegamento Modifica DataBindings nella casella di testo `UnitPrice` del `EditItemTemplate`di DataList.

Con questa modifica, il prezzo formattato nell'interfaccia di modifica include le virgole come separatore di gruppi e un punto come separatore decimale, ma lascia il simbolo di valuta.

> [!NOTE]
> Quando si rimuove il formato di valuta dall'interfaccia modificabile, è utile inserire il simbolo di valuta come testo all'esterno della casella di testo. Questa operazione funge da hint per l'utente che non è necessario fornire il simbolo di valuta.

## <a name="fixing-the-cancel-button"></a>Correzione del pulsante Annulla

Per impostazione predefinita, i controlli Web di convalida emettono JavaScript per eseguire la convalida sul lato client. Quando si fa clic su un pulsante, un LinkButton o un controllo ImageButton, i controlli di convalida della pagina vengono controllati sul lato client prima che si verifichi il postback. Se sono presenti dati non validi, il postback viene annullato. Per determinati pulsanti, tuttavia, la validità dei dati potrebbe essere irrilevante; in tal caso, il postback annullato a causa di dati non validi costituisce un fastidio.

Il pulsante Annulla è un esempio di questo tipo. Si supponga che un utente immetta dati non validi, ad esempio omettendo il nome del prodotto, quindi decide di non salvare il prodotto dopo tutto e di premere il pulsante Annulla. Attualmente, il pulsante Annulla attiva i controlli di convalida della pagina, che segnalano che il nome del prodotto è mancante e impedisce il postback. L'utente deve digitare del testo nella casella di testo `ProductName` solo per annullare il processo di modifica.

Fortunatamente, il pulsante, LinkButton e ImageButton hanno una [proprietà`CausesValidation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) che può indicare se fare clic sul pulsante deve avviare la logica di convalida (il valore predefinito è `True`). Impostare la proprietà `CausesValidation` del pulsante Annulla su `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Verifica della validità degli input nel gestore eventi UpdateCommand

A causa dello script lato client emesso dai controlli di convalida, se un utente immette un input non valido i controlli di convalida annullano i postback avviati dai controlli Button, LinkButton o ImageButton le cui proprietà `CausesValidation` sono `True` (impostazione predefinita). Tuttavia, se un utente visita un browser obsoleto o uno il cui supporto JavaScript è stato disabilitato, i controlli di convalida lato client non vengono eseguiti.

Tutti i controlli di convalida ASP.NET ripetono la logica di convalida immediatamente dopo il postback e segnalano la validità complessiva degli input della pagina tramite la [proprietà`Page.IsValid`](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Tuttavia, il flusso di pagina non viene interrotto o interrotto in alcun modo in base al valore di `Page.IsValid`. Gli sviluppatori hanno la responsabilità di garantire che la proprietà `Page.IsValid` abbia un valore di `True` prima di procedere con il codice che presuppone i dati di input validi.

Se un utente dispone di JavaScript disabilitato, visita la pagina, modifica un prodotto, immette un valore di prezzo troppo costoso e fa clic sul pulsante Aggiorna, la convalida sul lato client verrà ignorata e verrà introdotto un postback. Durante il postback, il ASP.NET della pagina s `UpdateCommand` gestore dell'evento viene eseguito e viene generata un'eccezione durante il tentativo di analisi troppo costosa per un `Decimal`. Poiché è presente una gestione delle eccezioni, un'eccezione di questo tipo verrà gestita normalmente, ma è possibile evitare che i dati non validi vengano sottoposti a inserirsi nella prima posizione continuando con il gestore dell'evento `UpdateCommand` se `Page.IsValid` ha un valore di `True`.

Aggiungere il codice seguente all'inizio del gestore dell'evento `UpdateCommand`, immediatamente prima del blocco di `Try`:

[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Con questa aggiunta, il prodotto tenterà di essere aggiornato solo se i dati inviati sono validi. La maggior parte degli utenti non sarà in grado di eseguire il postback dei dati non validi a causa dei controlli di convalida degli script sul lato client, ma gli utenti i cui browser non supportano JavaScript o che dispongono di supporto JavaScript disabilitato possono ignorare i controlli lato client e inviare dati non validi.

> [!NOTE]
> Quando si aggiornano i dati con GridView, non è stato necessario controllare in modo esplicito la proprietà `Page.IsValid` nella classe code-behind della pagina. Questo perché GridView consulta la proprietà `Page.IsValid` per gli Stati Uniti e procede solo con l'aggiornamento solo se restituisce un valore di `True`.

## <a name="step-3-summarizing-data-entry-problems"></a>Passaggio 3: riepilogo dei problemi di immissione dati

Oltre ai cinque controlli di convalida, ASP.NET include il [controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che visualizza i `ErrorMessage` di questi controlli di convalida che hanno rilevato dati non validi. È possibile visualizzare i dati di riepilogo come testo nella pagina Web o tramite un MessageBox modale sul lato client. Consente di migliorare questa esercitazione per includere un MessageBox sul lato client Riepilogando eventuali problemi di convalida.

A tale scopo, trascinare un controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. La posizione del controllo ValidationSummary non è rilevante, dal momento che verrà configurata in modo da visualizzare solo il riepilogo come MessageBox. Dopo l'aggiunta del controllo, impostare la relativa [proprietà`ShowSummary`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) su `False` e la relativa [proprietà`ShowMessageBox`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) su `True`. Con questa aggiunta, tutti gli errori di convalida vengono riepilogati in un MessageBox sul lato client (vedere la figura 6).

[![gli errori di convalida sono riepilogati in un MessageBox sul lato client](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: gli errori di convalida sono riepilogati in un MessageBox sul lato client ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come ridurre la probabilità di eccezioni usando i controlli di convalida per garantire in modo proattivo che gli input degli utenti siano validi prima di provare a usarli nel flusso di lavoro di aggiornamento. ASP.NET fornisce cinque controlli Web di convalida progettati per esaminare un particolare input del controllo Web e segnalare la validità dell'input. In questa esercitazione sono stati usati due di questi cinque controlli, RequiredFieldValidator e CompareValidator, per garantire che il nome del prodotto sia stato specificato e che il prezzo avesse un formato di valuta con un valore maggiore o uguale a zero.

L'aggiunta di controlli di convalida all'interfaccia di modifica di DataList è semplice come trascinarli nel `EditItemTemplate` dalla casella degli strumenti e impostare alcune proprietà. Per impostazione predefinita, i controlli di convalida emettono automaticamente lo script di convalida lato client. forniscono inoltre la convalida sul lato server per il postback, archiviando il risultato cumulativo nella proprietà `Page.IsValid`. Per ignorare la convalida lato client quando si fa clic su un pulsante, un LinkButton o un ImageButton, impostare la proprietà `CausesValidation` del pulsante su `False`. Inoltre, prima di eseguire qualsiasi attività con i dati inviati durante il postback, verificare che la proprietà `Page.IsValid` restituisca `True`.

Tutte le esercitazioni di modifica di DataList esaminate finora hanno una semplice interfaccia di modifica per il nome del prodotto e l'altra per il prezzo. L'interfaccia di modifica può tuttavia contenere una combinazione di controlli Web diversi, ad esempio DropDownList, calendari, radiopulsanti, caselle di controllo e così via. Nell'esercitazione successiva si esaminerà la creazione di un'interfaccia che usa un'ampia gamma di controlli Web.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Dennis Patterson, Ken Pespisa e Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](handling-bll-and-dal-level-exceptions-vb.md)
> [Successivo](customizing-the-datalist-s-editing-interface-vb.md)
