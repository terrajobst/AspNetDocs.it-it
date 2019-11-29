---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: Aggiunta di controlli di convalida alle interfacce di modifica e inserimentoC#() | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida a EditItemTemplate e InsertItemTemplate di un controllo Web di dati, per fornire un'ulteriore...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: 110ee08f1d0707664ef6268f34ceab9da30a3e61
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589987"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Aggiunta di controlli di convalida alle interfacce di modifica e inserimento (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) o [scaricare il file PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida a EditItemTemplate e InsertItemTemplate di un controllo Web di dati, per fornire un'interfaccia utente più infallibile.

## <a name="introduction"></a>Introduzione

I controlli GridView e DetailsView negli esempi esaminati nelle ultime tre esercitazioni sono stati composte da BoundField e CheckBoxFields (i tipi di campo aggiunti automaticamente da Visual Studio durante l'associazione di GridView o DetailsView a un'origine dati controllo tramite lo smart tag. Quando si modifica una riga in GridView o DetailsView, i BoundField che non sono di sola lettura vengono convertiti in caselle di testo, da cui l'utente finale può modificare i dati esistenti. Analogamente, quando si inserisce un nuovo record in un controllo DetailsView, i BoundField la cui proprietà `InsertVisible` è impostata su `true` (impostazione predefinita) vengono visualizzati come caselle di testo vuote, in cui l'utente può fornire i valori dei campi del nuovo record. Analogamente, CheckBoxFields, che sono disabilitati nell'interfaccia standard di sola lettura, vengono convertiti in caselle di controllo abilitate nelle interfacce di modifica e inserimento.

Mentre le interfacce di modifica e inserimento predefinite per BoundField e CheckBoxField possono essere utili, l'interfaccia non dispone di alcun tipo di convalida. Se un utente commette un errore di immissione dei dati, ad esempio omettendo il campo `ProductName` o immettendo un valore non valido per `UnitsInStock` (ad esempio-50), verrà generata un'eccezione dall'interno delle profondità dell'architettura dell'applicazione. Sebbene questa eccezione possa essere gestita normalmente come illustrato nell' [esercitazione precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), idealmente l'interfaccia utente di modifica o inserimento includerà i controlli di convalida per impedire a un utente di immettere tali dati non validi nella prima posizione.

Per fornire un'interfaccia di modifica o inserimento personalizzata, è necessario sostituire BoundField o CheckBoxField con un TemplateField. TemplateFields, che rappresenta l'argomento di discussione nell' [utilizzo di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e l' [utilizzo di TemplateFields nelle](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) esercitazioni sui controlli DetailsView, può essere costituito da più modelli che definiscono interfacce separate per diversi Stati di riga. Il `ItemTemplate` di TemplateField viene usato per il rendering di campi o righe di sola lettura nei controlli DetailsView o GridView, mentre le `EditItemTemplate` e `InsertItemTemplate` indicano rispettivamente le interfacce da usare per le modalità di modifica e inserimento.

In questa esercitazione verrà illustrato quanto sia semplice aggiungere controlli di convalida al `EditItemTemplate` e `InsertItemTemplate` di TemplateField per fornire un'interfaccia utente più infallibile. In particolare, questa esercitazione prende in considerazione l'esempio creato nell'esercitazione [analisi degli eventi associati all'inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) e potenzia le interfacce di modifica e inserimento per includere la convalida appropriata.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>Passaggio 1: replica dell'esempio dall'[analisi degli eventi associati all'inserimento, all'aggiornamento e all'eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

Nell'esercitazione [analisi degli eventi associati a inserimento, aggiornamento ed eliminazione](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) è stata creata una pagina che elenca i nomi e i prezzi dei prodotti in un GridView modificabile. Inoltre, nella pagina è incluso un oggetto DetailsView la cui proprietà `DefaultMode` è stata impostata su `Insert`, quindi viene sempre eseguito il rendering in modalità di inserimento. Da questo DetailsView, l'utente può immettere il nome e il prezzo per un nuovo prodotto, fare clic su Inserisci e aggiungerlo al sistema (vedere la figura 1).

[![esempio precedente consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Figura 1**: l'esempio precedente consente agli utenti di aggiungere nuovi prodotti e modificare quelli esistenti ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))

L'obiettivo di questa esercitazione è quello di migliorare DetailsView e GridView per fornire controlli di convalida. In particolare, la logica di convalida:

- Richiedi che il nome venga fornito durante l'inserimento o la modifica di un prodotto
- Richiedere che venga fornito il prezzo durante l'inserimento di un record; Quando si modifica un record, è comunque necessario un prezzo, ma userà la logica a livello di codice nel gestore dell'evento `RowUpdating` di GridView già presente nell'esercitazione precedente
- Verificare che il valore immesso per il prezzo sia un formato di valuta valido

Prima di poter esaminare l'aumento dell'esempio precedente per includere la convalida, è prima di tutto necessario replicare l'esempio dalla pagina `DataModificationEvents.aspx` alla pagina per questa esercitazione, `UIValidation.aspx`. A tale scopo, è necessario copiare sia il markup dichiarativo della pagina `DataModificationEvents.aspx` che il relativo codice sorgente. Prima di tutto, copiare il markup dichiarativo attenendosi alla procedura seguente:

1. Aprire la pagina `DataModificationEvents.aspx` in Visual Studio
2. Passare al markup dichiarativo della pagina. fare clic sul pulsante origine nella parte inferiore della pagina.
3. Copiare il testo all'interno dei tag `<asp:Content>` e `</asp:Content>` (righe da 3 a 44), come illustrato nella figura 2.

[![copiare il testo nel controllo &lt;ASP: content&gt;](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Figura 2**: copiare il testo all'interno del controllo `<asp:Content>` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))

1. Aprire la pagina `UIValidation.aspx`
2. Vai al markup dichiarativo della pagina
3. Incollare il testo all'interno del controllo `<asp:Content>`.

Per copiare il codice sorgente, aprire la pagina `DataModificationEvents.aspx.cs` e copiare solo il testo *all'interno* della classe `EditInsertDelete_DataModificationEvents`. Copiare i tre gestori eventi (`Page_Load`, `GridView1_RowUpdating`e `ObjectDataSource1_Inserting`), ma **non** copiare la dichiarazione della classe o le istruzioni `using`. Incollare il testo copiato *all'interno* della classe `EditInsertDelete_UIValidation` in `UIValidation.aspx.cs`.

Dopo aver spostato il contenuto e il codice da `DataModificationEvents.aspx` a `UIValidation.aspx`, è necessario provare a testare lo stato di avanzamento in un browser. Verrà visualizzato lo stesso output e si verificherà la stessa funzionalità in ognuna di queste due pagine (fare riferimento alla figura 1 per uno screenshot di `DataModificationEvents.aspx` in azione).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Passaggio 2: conversione dei BoundField in TemplateFields

Per aggiungere controlli di convalida alle interfacce di modifica e inserimento, i BoundField utilizzati dai controlli DetailsView e GridView devono essere convertiti in TemplateFields. A tale scopo, fare clic sui collegamenti Edit Columns e Edit Fields rispettivamente negli smart tag GridView e DetailsView. Selezionare ognuno dei BoundField e fare clic sul collegamento "Converti questo campo in un TemplateField".

[![convertire i BoundField di DetailsView e GridView in TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Figura 3**: convertire i BoundField di DetailsView e GridView in TemplateFields ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))

La conversione di un BoundField in un TemplateField tramite la finestra di dialogo campi genera un TemplateField che presenta le stesse interfacce di sola lettura, modifica e inserimento come il BoundField stesso. Il markup seguente mostra la sintassi dichiarativa per il campo `ProductName` in DetailsView dopo che è stata convertita in TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Si noti che questo TemplateField aveva tre modelli creati automaticamente `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate`. Il `ItemTemplate` Visualizza un singolo valore del campo dati (`ProductName`) utilizzando un controllo Web etichetta, mentre i `EditItemTemplate` e `InsertItemTemplate` presentano il valore del campo dati in un controllo Web TextBox che associa il campo dati alla proprietà di `Text` della casella di testo utilizzando l'associazione dati bidirezionale. Poiché in questa pagina viene utilizzato solo il DetailsView per l'inserimento, è possibile rimuovere il `ItemTemplate` e `EditItemTemplate` dai due TemplateFields, anche se non vi è alcun danno in uscita.

Poiché GridView non supporta le funzionalità di inserimento predefinite di DetailsView, la conversione del campo `ProductName` di GridView in un TemplateField restituisce solo un `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

Facendo clic sul pulsante "Converti questo campo in un TemplateField", Visual Studio ha creato un TemplateField i cui modelli simulano l'interfaccia utente del BoundField convertito. Per verificarlo, è possibile visitare questa pagina tramite un browser. Si noterà che l'aspetto e il comportamento di TemplateFields sono identici a quelli dell'esperienza di utilizzo dei BoundField.

> [!NOTE]
> È possibile personalizzare le interfacce di modifica nei modelli in base alle esigenze. È ad esempio possibile che sia necessario che la casella di testo `UnitPrice` sottoposta a TemplateFields rendering come casella di testo più piccola rispetto alla casella di testo `ProductName`. A tale scopo, è possibile impostare la proprietà `Columns` della casella di testo su un valore appropriato o fornire una larghezza assoluta tramite la proprietà `Width`. Nell'esercitazione successiva si vedrà come personalizzare completamente l'interfaccia di modifica sostituendo la casella di testo con un controllo Web di immissione dati alternativo.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Passaggio 3: aggiunta dei controlli di convalida al`EditItemTemplate` s di GridView

Quando si creano form di immissione dati, è importante che gli utenti immettano tutti i campi obbligatori e che tutti gli input forniti siano valori validi e formattati correttamente. Per garantire la validità degli input di un utente, ASP.NET fornisce cinque controlli di convalida predefiniti progettati per essere usati per convalidare il valore di un singolo controllo di input:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantisce che sia stato fornito un valore
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) convalida un valore rispetto a un altro valore di controllo Web o a un valore costante oppure garantisce che il formato del valore sia valido per un tipo di dati specificato.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantisce che un valore si trovi all'interno di un intervallo di valori
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) convalida un valore rispetto a un' [espressione regolare](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) convalida un valore rispetto a un metodo personalizzato definito dall'utente

Per altre informazioni su questi cinque controlli, vedere la [sezione controlli di convalida](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) delle [esercitazioni introduttive di ASP.NET](https://asp.net/QuickStart/aspnet/).

Per questa esercitazione è necessario usare un oggetto RequiredFieldValidator sia nel `ProductName` TemplateFields di DetailsView che in GridView e in un oggetto RequiredFieldValidator nel `UnitPrice` TemplateField di DetailsView. Inoltre, è necessario aggiungere un oggetto CompareValidator a entrambi i controlli ' `UnitPrice` TemplateFields che garantisce che il prezzo immesso abbia un valore maggiore o uguale a 0 e venga presentato in un formato di valuta valido.

> [!NOTE]
> Mentre ASP.NET 1. x aveva questi stessi cinque controlli di convalida, ASP.NET 2,0 ha aggiunto una serie di miglioramenti, i due principali sono il supporto degli script lato client per browser diversi da Internet Explorer e la possibilità di partizionare i controlli di convalida in una pagina in gruppi di convalida. Per ulteriori informazioni sulle nuove funzionalità di controllo di convalida in 2,0, vedere l'articolo relativo alla [dissezione dei controlli di convalida in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Per iniziare, aggiungere i controlli di convalida necessari all'`EditItemTemplate` s nel TemplateFields di GridView. A tale scopo, fare clic sul collegamento modifica modelli dallo smart tag di GridView per visualizzare l'interfaccia di modifica dei modelli. Da qui è possibile selezionare il modello da modificare dall'elenco a discesa. Poiché si vuole aumentare l'interfaccia di modifica, è necessario aggiungere i controlli di convalida al `ProductName` e `UnitPrice``EditItemTemplate` s.

[![è necessario estendere il EditItemTemplates di ProductName e di PrezzoUnitario](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Figura 4**: è necessario estendere il `ProductName` e `UnitPrice`di `EditItemTemplate` s ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))

Nel `EditItemTemplate``ProductName` aggiungere un RequiredFieldValidator trascinandolo dalla casella degli strumenti nell'interfaccia di modifica del modello, inserendo dopo la casella di testo.

[![aggiungere un RequiredFieldValidator al EditItemTemplate ProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Figura 5**: aggiungere un RequiredFieldValidator al `EditItemTemplate` `ProductName` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))

Tutti i controlli di convalida funzionano convalidando l'input di un singolo controllo Web ASP.NET. Pertanto, è necessario indicare che il RequiredFieldValidator appena aggiunto deve essere convalidato in base alla casella di testo nell'`EditItemTemplate`; Questa operazione viene eseguita impostando la [proprietà ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) del controllo di convalida sul `ID` del controllo Web appropriato. Alla casella di testo è attualmente assegnata una `ID` piuttosto anonima di `TextBox1`, ma è possibile modificarla in modo più appropriato. Fare clic sulla casella di testo nel modello e quindi, dal Finestra Proprietà, modificare la `ID` da `TextBox1` a `EditProductName`.

[![modificare l'ID della casella di testo in EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Figura 6**: modificare la `ID` della casella di testo in `EditProductName` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))

Impostare quindi la proprietà `ControlToValidate` di RequiredFieldValidator su `EditProductName`. Infine, impostare la [proprietà ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) su "è necessario specificare il nome del prodotto" e la [proprietà Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) su "\*". Il valore della proprietà `Text`, se specificato, è il testo visualizzato dal controllo di convalida se la convalida ha esito negativo. Il valore della proprietà `ErrorMessage`, che è obbligatorio, viene utilizzato dal controllo ValidationSummary; Se il valore della proprietà `Text` viene omesso, il valore della proprietà `ErrorMessage` è anche il testo visualizzato dal controllo di convalida sull'input non valido.

Dopo aver impostato queste tre proprietà del RequiredFieldValidator, la schermata dovrebbe essere simile alla figura 7.

[![impostare le proprietà ControlToValidate, ErrorMessage e Text di RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Figura 7**: impostare le proprietà `ControlToValidate`, `ErrorMessage`e `Text` di RequiredFieldValidator ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))

Con il RequiredFieldValidator aggiunto al `ProductName` `EditItemTemplate`, resta solo aggiungere la convalida necessaria al `UnitPrice` `EditItemTemplate`. Poiché abbiamo deciso che, per questa pagina, la `UnitPrice` è facoltativa quando si modifica un record, non è necessario aggiungere un RequiredFieldValidator. Tuttavia, è necessario aggiungere un oggetto CompareValidator per garantire che il `UnitPrice`, se specificato, sia formattato correttamente come valuta ed è maggiore o uguale a 0.

Prima di aggiungere il controllo CompareValidator alla `UnitPrice` `EditItemTemplate`, è necessario modificare l'ID del controllo Web TextBox da `TextBox2` a `EditUnitPrice`. Dopo avere apportato questa modifica, aggiungere il CompareValidator, impostare la relativa proprietà `ControlToValidate` su `EditUnitPrice`, la relativa proprietà `ErrorMessage` su "il prezzo deve essere maggiore o uguale a zero e non può includere il simbolo di valuta" e la relativa proprietà `Text` su "\*".

Per indicare che il valore di `UnitPrice` deve essere maggiore o uguale a 0, impostare la [Proprietà Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) del CompareValidator su `GreaterThanEqual`, la relativa [proprietà ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) su "0" e la relativa [proprietà Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) su `Currency`. La sintassi dichiarativa seguente mostra i `EditItemTemplate` di `UnitPrice` TemplateField dopo che sono state apportate queste modifiche:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Dopo avere apportato queste modifiche, aprire la pagina in un browser. Se si tenta di omettere il nome o si immette un valore di prezzo non valido durante la modifica di un prodotto, accanto alla casella di testo viene visualizzato un asterisco. Come illustrato nella figura 8, un valore del prezzo che include il simbolo di valuta, ad esempio $19,95, viene considerato non valido. Il `Currency` del CompareValidator `Type` consente separatori di cifre, ad esempio virgole o punti, a seconda delle impostazioni cultura, e un segno più o meno, ma *non consente un* simbolo di valuta. Questo comportamento può avere perplessità sugli utenti perché l'interfaccia di modifica esegue attualmente il rendering del `UnitPrice` usando il formato di valuta.

> [!NOTE]
> Tenere presente che negli *eventi associati all'esercitazione di inserimento, aggiornamento ed eliminazione* è stata impostata la proprietà `DataFormatString` di BoundField su `{0:c}` per formattarla come valuta. Inoltre, la proprietà `ApplyFormatInEditMode` viene impostata su true, facendo in modo che l'interfaccia di modifica di GridView formatti la `UnitPrice` come valuta. Quando si converte il BoundField in un TemplateField, Visual Studio ha annotato queste impostazioni e formattato la proprietà `Text` della casella di testo come valuta usando la sintassi di associazione dati `<%# Bind("UnitPrice", "{0:c}") %>`.

[![viene visualizzato un asterisco accanto alle caselle di testo con input non valido](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Figura 8**: viene visualizzato un asterisco accanto alle caselle di testo con input non valido ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))

Mentre la convalida funziona così com'è, l'utente deve rimuovere manualmente il simbolo di valuta quando si modifica un record, il che non è accettabile. Per risolvere questo problema, sono disponibili tre opzioni:

1. Configurare il `EditItemTemplate` in modo che il valore di `UnitPrice` non venga formattato come valuta.
2. Consente all'utente di immettere un simbolo di valuta rimuovendo il CompareValidator e sostituendolo con un RegularExpressionValidator che controlla correttamente la presenza di un valore di valuta formattato correttamente. Il problema in questo caso è che l'espressione regolare per convalidare un valore di valuta non è abbastanza e richiederebbe la scrittura di codice per incorporare le impostazioni cultura.
3. Rimuovere completamente il controllo di convalida e basarsi sulla logica di convalida lato server nel gestore dell'evento `RowUpdating` di GridView.

Per questo esercizio verrà ora usata l'opzione #1. Attualmente il `UnitPrice` viene formattato come valuta a causa dell'espressione DataBinding per la casella di testo nel `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Modificare l'istruzione bind in `Bind("UnitPrice", "{0:n2}")`, che formatta il risultato come numero con due cifre di precisione. Questa operazione può essere eseguita direttamente tramite la sintassi dichiarativa oppure facendo clic sul collegamento Modifica DataBindings dalla casella di testo `EditUnitPrice` nella `EditItemTemplate` di `UnitPrice` TemplateField (vedere figure 9 e 10).

[![fare clic sul collegamento Modifica DataBindings della casella di testo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Figura 9**: fare clic sul collegamento Modifica DataBindings della casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))

[![specificare l'identificatore di formato nell'istruzione bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Figura 10**: specificare l'identificatore di formato nell'istruzione `Bind` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))

Con questa modifica, il prezzo formattato nell'interfaccia di modifica include le virgole come separatore di gruppi e un punto come separatore decimale, ma lascia il simbolo di valuta.

> [!NOTE]
> Il `UnitPrice` `EditItemTemplate` non include un RequiredFieldValidator, consentendo il postback e la logica di aggiornamento per iniziare. Tuttavia, il gestore dell'evento `RowUpdating` copiato dall'esercitazione *analisi degli eventi associati a inserimento, aggiornamento ed eliminazione* include un controllo a livello di codice che garantisce che venga fornito il `UnitPrice`. È possibile rimuovere questa logica, lasciarla così com'è o aggiungere un RequiredFieldValidator al `EditItemTemplate``UnitPrice`.

## <a name="step-4-summarizing-data-entry-problems"></a>Passaggio 4: riepilogo dei problemi di immissione dati

Oltre ai cinque controlli di convalida, ASP.NET include il [controllo ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), che visualizza i `ErrorMessage` di questi controlli di convalida che hanno rilevato dati non validi. È possibile visualizzare i dati di riepilogo come testo nella pagina Web o tramite un MessageBox modale sul lato client. Verrà ora migliorata questa esercitazione per includere un MessageBox sul lato client che riepiloga gli eventuali problemi di convalida.

A tale scopo, trascinare un controllo ValidationSummary dalla casella degli strumenti nella finestra di progettazione. Il percorso del controllo di convalida non è rilevante, perché verrà configurato in modo da visualizzare solo il riepilogo come MessageBox. Dopo l'aggiunta del controllo, impostare la relativa [proprietà ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) su `false` e la relativa [proprietà Metodo ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) su `true`. Con questa aggiunta, tutti gli errori di convalida vengono riepilogati in un MessageBox sul lato client.

[![gli errori di convalida sono riepilogati in un MessageBox sul lato client](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Figura 11**: gli errori di convalida sono riepilogati in un MessageBox sul lato client ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Passaggio 5: aggiunta dei controlli di convalida al`InsertItemTemplate` di DetailsView

Per questa esercitazione rimane solo l'aggiunta di controlli di convalida all'interfaccia di inserimento di DetailsView. Il processo di aggiunta di controlli di convalida ai modelli di DetailsView è identico a quello esaminato nel passaggio 3. quindi, l'attività verrà rimossa in questo passaggio. Come è stato fatto con la `EditItemTemplate` s di GridView, si consiglia di rinominare i `ID` di caselle di testo dal `TextBox1` anonimo e `TextBox2` a `InsertProductName` e `InsertUnitPrice`.

Aggiungere un RequiredFieldValidator al `InsertItemTemplate``ProductName`. Impostare il `ControlToValidate` sul `ID` della casella di testo nel modello, la relativa proprietà `Text` su "\*" e la relativa proprietà `ErrorMessage` su "è necessario fornire il nome del prodotto".

Poiché il `UnitPrice` è necessario per questa pagina quando si aggiunge un nuovo record, aggiungere un RequiredFieldValidator al `UnitPrice` `InsertItemTemplate`, impostando le proprietà `ControlToValidate`, `Text`e `ErrorMessage` in modo appropriato. Infine, aggiungere un oggetto CompareValidator anche al `UnitPrice` `InsertItemTemplate`, configurando le proprietà `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`e `ValueToCompare` esattamente come è stato fatto con il controllo CompareValidator `UnitPrice`nell'`EditItemTemplate`di GridView.

Dopo l'aggiunta di questi controlli di convalida, non è possibile aggiungere un nuovo prodotto al sistema se il nome non viene fornito o se il relativo prezzo è un numero negativo o non è formattato in modo valido.

[![logica di convalida è stata aggiunta all'interfaccia di inserimento di DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Figura 12**: è stata aggiunta la logica di convalida all'interfaccia di inserimento di DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Passaggio 6: partizionamento dei controlli di convalida in gruppi di convalida

La pagina è costituita da due set di controlli di convalida logicamente diversi, ovvero quelli che corrispondono all'interfaccia di modifica di GridView e quelli che corrispondono all'interfaccia di inserimento di DetailsView. Per impostazione predefinita, quando si verifica un postback *tutti i* controlli di convalida della pagina sono controllati. Tuttavia, quando si modifica un record, non si vuole che i controlli di convalida dell'interfaccia di inserimento di DetailsView vengano convalidati. Nella figura 13 viene illustrato il dilemma corrente quando un utente sta modificando un prodotto con valori perfettamente validi, facendo clic su Aggiorna viene generato un errore di convalida perché i valori di nome e prezzo nell'interfaccia di inserimento sono vuoti.

[![l'aggiornamento di un prodotto comporta l'attivazione dei controlli di convalida dell'interfaccia di inserimento](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Figura 13**: l'aggiornamento di un prodotto provoca l'attivazione dei controlli di convalida dell'interfaccia di inserimento ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))

I controlli di convalida in ASP.NET 2,0 possono essere partizionati in gruppi di convalida tramite la relativa proprietà `ValidationGroup`. Per associare un set di controlli di convalida in un gruppo, è sufficiente impostare la relativa proprietà `ValidationGroup` sullo stesso valore. Per questa esercitazione, impostare le proprietà `ValidationGroup` dei controlli di convalida nel TemplateFields di GridView su `EditValidationControls` e le proprietà `ValidationGroup` del TemplateFields di DetailsView su `InsertValidationControls`. Queste modifiche possono essere eseguite direttamente nel markup dichiarativo o tramite il Finestra Proprietà quando si usa l'interfaccia di modifica modello della finestra di progettazione.

Oltre ai controlli di convalida, i controlli correlati a pulsanti e pulsanti in ASP.NET 2,0 includono anche una proprietà `ValidationGroup`. Viene verificata la validità dei validator di un gruppo di convalida solo quando un postback è indotto da un pulsante con la stessa impostazione della proprietà `ValidationGroup`. Ad esempio, per consentire al pulsante di inserimento di DetailsView di attivare il `InsertValidationControls` gruppo di convalida, è necessario impostare la proprietà `ValidationGroup` di CommandField su `InsertValidationControls` (vedere la figura 14). Inoltre, impostare la proprietà CommandField's `ValidationGroup` di GridView su `EditValidationControls`.

[![impostare la proprietà ValidationGroup CommandField's di DetailsView su InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Figura 14**: impostare la proprietà CommandField's `ValidationGroup` di DetailsView su `InsertValidationControls` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))

Una volta apportate queste modifiche, TemplateFields e CommandFields di DetailsView e GridView dovrebbero avere un aspetto simile al seguente:

TemplateFields e CommandField di DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

CommandField e TemplateFields di GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

A questo punto, i controlli di convalida specifici per la modifica vengono attivati solo quando si fa clic sul pulsante Aggiorna di GridView e i controlli di convalida specifici dell'inserimento vengono attivati solo quando si fa clic sul pulsante di inserimento di DetailsView, risolvendo il problema evidenziato dalla figura 13. Con questa modifica, tuttavia, il controllo ValidationSummary non viene più visualizzato quando si immettono dati non validi. Il controllo ValidationSummary contiene inoltre una proprietà `ValidationGroup` e Mostra solo le informazioni di riepilogo per i controlli di convalida nel gruppo di convalida. In questa pagina è quindi necessario disporre di due controlli di convalida, uno per il gruppo di convalida `InsertValidationControls` e uno per `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Con questa aggiunta, l'esercitazione è stata completata.

## <a name="summary"></a>Riepilogo

Mentre i BoundField possono fornire un'interfaccia di inserimento e modifica, l'interfaccia non è personalizzabile. In genere, si desidera aggiungere controlli di convalida all'interfaccia di modifica e inserimento per assicurarsi che l'utente immetta gli input richiesti in un formato valido. A tale scopo, è necessario convertire i BoundField in TemplateFields e aggiungere i controlli di convalida ai modelli appropriati. In questa esercitazione è stato esteso l'esempio dell'esercitazione *analisi degli eventi associati a inserimento, aggiornamento ed eliminazione* , aggiunta di controlli di convalida sia all'interfaccia di inserimento di DetailsView sia all'interfaccia di modifica di GridView. È stato inoltre illustrato come visualizzare le informazioni di convalida di riepilogo tramite il controllo ValidationSummary e come partizionare i controlli di convalida nella pagina in gruppi di convalida distinti.

Come illustrato in questa esercitazione, TemplateFields consente di aumentare le interfacce di modifica e inserimento per includere i controlli di convalida. TemplateFields può anche essere esteso per includere controlli Web di input aggiuntivi, consentendo alla casella di testo di essere sostituita da un controllo Web più appropriato. Nell'esercitazione successiva verrà illustrato come sostituire il controllo TextBox con un controllo DropDownList associato a dati, ideale quando si modifica una chiave esterna (ad esempio `CategoryID` o `SupplierID` nella tabella `Products`).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Liz Shulok e Zack Jones. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [Successivo](customizing-the-data-modification-interface-cs.md)
