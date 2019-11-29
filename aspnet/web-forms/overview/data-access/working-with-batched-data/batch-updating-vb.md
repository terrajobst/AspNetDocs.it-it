---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Aggiornamento batch (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come aggiornare più record di database in un'unica operazione. Nel livello dell'interfaccia utente si compila un controllo GridView in cui ogni riga è modificabile. Nei dati...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: f0bb83b17585876dd6d28a5893a223cce15da31d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74591055"
---
# <a name="batch-updating-vb"></a>Aggiornamento batch (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) o [Scarica PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Informazioni su come aggiornare più record di database in un'unica operazione. Nel livello dell'interfaccia utente si compila un controllo GridView in cui ogni riga è modificabile. Nel livello di accesso ai dati viene eseguito il wrapping di più operazioni di aggiornamento all'interno di una transazione per garantire l'esito positivo di tutti gli aggiornamenti o il rollback di tutti gli aggiornamenti.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](wrapping-database-modifications-within-a-transaction-vb.md) è stato illustrato come estendere il livello di accesso ai dati per aggiungere il supporto per le transazioni del database. Le transazioni di database garantiscono che una serie di istruzioni di modifica dei dati verrà considerata come un'unica operazione atomica, che garantisce che tutte le modifiche avranno esito negativo. Grazie a questa funzionalità di basso livello, è possibile riattivare l'attenzione alla creazione di interfacce di modifica dei dati batch.

In questa esercitazione verrà compilato un controllo GridView in cui ogni riga è modificabile (vedere la figura 1). Poiché ogni riga viene sottoposta a rendering nell'interfaccia di modifica, non è necessario disporre di una colonna di pulsanti modifica, aggiornamento e Annulla. Al contrario, nella pagina sono presenti due pulsanti Aggiorna prodotti che, quando si fa clic, enumerano le righe GridView e aggiornano il database.

[![ogni riga in GridView è modificabile](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: ogni riga in GridView è modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image2.png))

Inizia subito.

> [!NOTE]
> Nell'esercitazione [esecuzione degli aggiornamenti batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) è stata creata un'interfaccia di modifica batch con il controllo DataList. Questa esercitazione è diversa da quella precedente in che usa GridView e l'aggiornamento batch viene eseguito nell'ambito di una transazione. Al termine dell'esercitazione, si consiglia di tornare all'esercitazione precedente e di aggiornarla in modo da utilizzare la funzionalità relativa alle transazioni di database aggiunta nell'esercitazione precedente.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Esame dei passaggi per rendere modificabili tutte le righe GridView

Come illustrato nella [Panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , GridView offre supporto incorporato per la modifica dei dati sottostanti in base alle singole righe. Internamente, GridView rileva la riga modificabile tramite la relativa [proprietà`EditIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Quando GridView viene associato alla relativa origine dati, controlla ogni riga per verificare se l'indice della riga è uguale al valore di `EditIndex`. In tal caso, viene eseguito il rendering dei campi della riga usando le interfacce di modifica. Per i BoundField, l'interfaccia di modifica è un controllo TextBox la cui proprietà `Text` viene assegnato il valore del campo dati specificato dalla proprietà `DataField` di BoundField. Per TemplateFields, il `EditItemTemplate` viene usato al posto del `ItemTemplate`.

Ricordare che il flusso di lavoro di modifica viene avviato quando un utente fa clic sul pulsante di modifica di una riga. Questa operazione causa un postback, imposta la proprietà `EditIndex` di GridView sull'indice della riga su cui è stato fatto clic e riassocia i dati alla griglia. Quando si fa clic sul pulsante Annulla di una riga, al postback il `EditIndex` viene impostato sul valore `-1` prima di riassociare i dati alla griglia. Poiché le righe di GridView avviano l'indicizzazione su zero, l'impostazione di `EditIndex` su `-1` ha l'effetto di visualizzare il controllo GridView in modalità di sola lettura.

La proprietà `EditIndex` funziona correttamente per la modifica per riga, ma non è progettata per la modifica batch. Per rendere modificabile l'intero GridView, è necessario eseguire il rendering di ogni riga usando l'interfaccia di modifica. Il modo più semplice per eseguire questa operazione consiste nel creare dove ogni campo modificabile viene implementato come TemplateField con l'interfaccia di modifica definita nell'`ItemTemplate`.

Nei passaggi successivi verrà creato un controllo GridView completamente modificabile. Nel passaggio 1 si inizierà con la creazione di GridView e del relativo ObjectDataSource e la conversione dei BoundField e di CheckBoxField in TemplateFields. Nei passaggi 2 e 3 si sposteranno le interfacce di modifica dal TemplateFields `EditItemTemplate` s alla `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Passaggio 1: visualizzazione delle informazioni sul prodotto

Prima di preoccuparsi della creazione di un controllo GridView in cui le righe sono modificabili, è sufficiente visualizzare le informazioni sul prodotto. Aprire la pagina `BatchUpdate.aspx` nella cartella `BatchData` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare il `ID` di GridView su `ProductsGrid` e, dal relativo smart tag, scegliere di associarlo a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per recuperare i dati dal metodo `GetProducts` della classe `ProductsBLL`.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configurare ObjectDataSource per l'uso della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image4.png))

[![recuperare i dati del prodotto utilizzando il metodo GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recuperare i dati del prodotto utilizzando il metodo `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image6.png))

Come GridView, le funzionalità di modifica di ObjectDataSource sono progettate per funzionare in base alle singole righe. Per aggiornare un set di record, è necessario scrivere un po' di codice nella classe code-behind della pagina ASP.NET che suddivide in batch i dati e li passa al BLL. Quindi, impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione di ObjectDataSource su (nessuno). Fare clic su Fine per completare la procedura guidata.

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image8.png))

Al termine della configurazione guidata origine dati, il markup dichiarativo di ObjectDataSource sarà simile al seguente:

[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Se si completa la configurazione guidata origine dati, in Visual Studio vengono creati i BoundField e un CheckBoxField per i campi dati del prodotto in GridView. Per questa esercitazione, consente all'utente di visualizzare e modificare il nome del prodotto, la categoria, il prezzo e lo stato interrotto. Rimuovere tutti i campi tranne i `ProductName`, `CategoryName`, `UnitPrice`e `Discontinued` e rinominare rispettivamente le proprietà `HeaderText` dei primi tre campi in Product, Category e price. Infine, selezionare le caselle di controllo Abilita paging e Abilita ordinamento nello smart tag di GridView.

A questo punto GridView ha tre BoundField (`ProductName`, `CategoryName`e `UnitPrice`) e un oggetto CheckBoxField (`Discontinued`). È necessario convertire questi quattro campi in TemplateFields, quindi spostare l'interfaccia di modifica dal `EditItemTemplate` TemplateField al relativo `ItemTemplate`.

> [!NOTE]
> È stata esaminata la creazione e la personalizzazione di TemplateFields nell'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) . Verranno illustrati i passaggi per la conversione dei BoundField e di CheckBoxField in TemplateFields e la definizione delle interfacce di modifica nella `ItemTemplate` s, ma se si rimane bloccati o si necessita di un aggiornamento, non è possibile fare riferimento a questa esercitazione precedente.

Dallo smart tag GridView s fare clic sul collegamento Modifica colonne per aprire la finestra di dialogo campi. Selezionare quindi ogni campo e fare clic sul collegamento Converti questo campo in un TemplateField.

![Convertire i BoundField esistenti e CheckBoxField in TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: convertire i BoundField esistenti e CheckBoxField in TemplateFields

Ora che ogni campo è un TemplateField, è possibile spostare l'interfaccia di modifica dal `EditItemTemplate` s al `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Passaggio 2: creazione delle interfacce di modifica di`ProductName`,`UnitPrice`e`Discontinued`

La creazione di `ProductName`, `UnitPrice`e `Discontinued` le interfacce di modifica sono l'argomento di questo passaggio e sono piuttosto semplici, perché ogni interfaccia è già definita in TemplateField s `EditItemTemplate`. La creazione dell'interfaccia di modifica `CategoryName` è un po' più complessa perché è necessario creare un oggetto DropDownList delle categorie applicabili. Questa `CategoryName` interfaccia di modifica è affrontata nel passaggio 3.

Inizia con il `ProductName` TemplateField. Fare clic sul collegamento modifica modelli dallo smart tag GridView s ed eseguire il drill-down fino al `EditItemTemplate``ProductName` TemplateField s. Selezionare la casella di testo, copiarla negli Appunti e quindi incollarla nel `ProductName` TemplateField s `ItemTemplate`. Modificare la proprietà `ID` della casella di testo in `ProductName`.

Successivamente, aggiungere un RequiredFieldValidator al `ItemTemplate` per assicurarsi che l'utente fornisca un valore per ogni nome di prodotto. Impostare la proprietà `ControlToValidate` su ProductName, la proprietà `ErrorMessage` su è necessario fornire il nome del prodotto. e la proprietà `Text` \*. Dopo aver apportato queste aggiunte alla `ItemTemplate`, la schermata dovrebbe essere simile alla figura 6.

[![ProductName TemplateField include ora una casella di testo e un oggetto RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: il `ProductName` TemplateField include ora una casella di testo e un oggetto RequiredFieldValidator ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image10.png))

Per l'interfaccia di modifica `UnitPrice`, iniziare copiando la casella di testo dall'`EditItemTemplate` al `ItemTemplate`. Inserire quindi un valore $ davanti alla casella di testo e impostare la relativa proprietà `ID` su PrezzoUnitario e la relativa proprietà `Columns` su 8.

Aggiungere inoltre un oggetto CompareValidator al `ItemTemplate` `UnitPrice` s per assicurarsi che il valore immesso dall'utente sia un valore di valuta valido maggiore o uguale a $0,00. Impostare la proprietà `ControlToValidate` validator su PrezzoUnitario, la relativa proprietà `ErrorMessage` su è necessario immettere un valore di valuta valido. Omettere tutti i simboli di valuta, la relativa proprietà `Text` \*, la relativa proprietà `Type` `Currency`, la relativa proprietà `Operator` su `GreaterThanEqual`e la relativa proprietà `ValueToCompare` su 0.

[![aggiungere un oggetto CompareValidator per verificare che il prezzo immesso sia un valore di valuta non negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: aggiungere un oggetto CompareValidator per verificare che il prezzo immesso sia un valore di valuta non negativo ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image12.png))

Per il `Discontinued` TemplateField è possibile usare la casella di controllo già definita nell'`ItemTemplate`. È sufficiente impostare il `ID` su Discontinued e la relativa proprietà `Enabled` su `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Passaggio 3: creazione dell'interfaccia di modifica`CategoryName`

L'interfaccia di modifica nel `EditItemTemplate` `CategoryName` TemplateField s contiene una casella di testo che Visualizza il valore del campo `CategoryName` data. È necessario sostituirlo con un oggetto DropDownList che elenca le categorie possibili.

> [!NOTE]
> L'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) contiene una discussione più approfondita e completa sulla personalizzazione di un modello in modo da includere un oggetto DropDownList anziché una casella di testo. Mentre i passaggi sono completi, vengono presentati laconicamente. Per un'analisi più approfondita della creazione e della configurazione del DropDownList delle categorie, vedere l'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

Trascinare un oggetto DropDownList dalla casella degli strumenti nel `ItemTemplate``CategoryName` TemplateField, impostando la relativa `ID` su `Categories`. A questo punto, in genere si definisce l'origine dati di DropDownList tramite il relativo smart tag, creando un nuovo ObjectDataSource. Tuttavia, in questo modo verrà aggiunto ObjectDataSource all'interno dell'`ItemTemplate`, che comporterà l'creazione di un'istanza di ObjectDataSource per ogni riga GridView. Al contrario, è necessario creare ObjectDataSource all'esterno di GridView s TemplateFields. Terminare la modifica del modello e trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione sotto il `ProductsDataSource` ObjectDataSource. Denominare il nuovo ObjectDataSource `CategoriesDataSource` e configurarlo per l'uso del metodo `GetCategories` della classe `CategoriesBLL`.

[![configurare ObjectDataSource per l'utilizzo della classe CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configurare ObjectDataSource per l'uso della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image14.png))

[![recuperare i dati della categoria utilizzando il metodo GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recuperare i dati della categoria utilizzando il metodo `GetCategories` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image16.png))

Poiché questo ObjectDataSource viene usato semplicemente per recuperare i dati, impostare gli elenchi a discesa nelle schede Aggiorna ed Elimina su (nessuno). Fare clic su Fine per completare la procedura guidata.

[![impostare gli elenchi a discesa nelle schede Aggiorna ed Elimina su (nessuno)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: impostare gli elenchi a discesa nelle schede Aggiorna ed Elimina su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image18.png))

Al termine della procedura guidata, il markup dichiarativo `CategoriesDataSource` s dovrebbe essere simile al seguente:

[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Con la `CategoriesDataSource` creata e configurata, tornare al `CategoryName` TemplateField s `ItemTemplate` e, dallo smart tag di DropDownList, fare clic sul collegamento Scegli origine dati. Nella configurazione guidata origine dati selezionare l'opzione `CategoriesDataSource` dal primo elenco a discesa e scegliere di `CategoryName` utilizzato per la visualizzazione e `CategoryID` come valore.

[![associare il DropDownList a CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: associare l'oggetto DropDownList al `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image20.png))

A questo punto, il `Categories` DropDownList elenca tutte le categorie, ma non seleziona automaticamente la categoria appropriata per il prodotto associato alla riga di GridView. A tale scopo, è necessario impostare il `Categories` DropDownList s `SelectedValue` sul valore `CategoryID` del prodotto. Fare clic sul collegamento Modifica DataBindings dallo smart tag DropDownList e associare la proprietà `SelectedValue` al campo dati `CategoryID`, come illustrato nella figura 12.

![Associare il valore CategoryID del prodotto alla proprietà SelectedValue di DropDownList](batch-updating-vb/_static/image12.gif)

**Figura 12**: associare il valore di `CategoryID` del prodotto alla proprietà `SelectedValue` di DropDownList

Un ultimo problema rimane: se il prodotto non ha un valore `CategoryID` specificato, l'istruzione DataBinding su `SelectedValue` genererà un'eccezione. Questo perché il DropDownList contiene solo elementi per le categorie e non offre un'opzione per i prodotti con un valore `NULL` database per `CategoryID`. Per risolvere questo problema, impostare la proprietà `AppendDataBoundItems` di DropDownList su `True` e aggiungere un nuovo elemento al DropDownList, omettendo la proprietà `Value` dalla sintassi dichiarativa. Ovvero, assicurarsi che la sintassi dichiarativa `Categories` DropDownList sia simile alla seguente:

[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Si noti che il `<asp:ListItem Value="">`--Select One--ha il relativo attributo `Value` impostato in modo esplicito su una stringa vuota. Vedere l'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) per una discussione più approfondita sul motivo per cui questo elemento DropDownList aggiuntivo è necessario per gestire il `NULL` caso e perché l'assegnazione della proprietà `Value` a una stringa vuota è essenziale.

> [!NOTE]
> Qui è possibile che si verifichi un problema di prestazioni e scalabilità, vale a dire. Poiché ogni riga dispone di un oggetto DropDownList che utilizza il `CategoriesDataSource` come origine dati, il metodo della classe `CategoriesBLL` `GetCategories` verrà chiamato *n* volte per ogni visita della pagina, dove *n* è il numero di righe in GridView. Queste *n* chiamate a `GetCategories` generano *n* query nel database. Questo effetto sul database potrebbe essere ridotto memorizzando nella cache le categorie restituite in una cache per richiesta o tramite il livello di memorizzazione nella cache utilizzando una dipendenza della cache SQL o una scadenza molto breve basata sul tempo. Per ulteriori informazioni sull'opzione di memorizzazione nella cache per richiesta, vedere [`HttpContext.Items` un archivio cache per richiesta](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Passaggio 4: completamento dell'interfaccia di modifica

Sono state apportate alcune modifiche ai modelli di GridView senza sospendere per visualizzare lo stato di avanzamento. Per visualizzare lo stato di avanzamento, è possibile usare un browser. Come illustrato nella figura 13, ogni riga viene sottoposta a rendering utilizzando la relativa `ItemTemplate`, che contiene l'interfaccia di modifica della cella.

[![ogni riga GridView è modificabile](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: ogni riga di GridView è modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image22.png))

A questo punto è necessario prestare attenzione ad alcuni piccoli problemi di formattazione. Prima di tutto, si noti che il valore `UnitPrice` contiene quattro punti decimali. Per risolvere il problema, tornare al `ItemTemplate` `UnitPrice` TemplateField e, dallo smart tag della casella di testo, fare clic sul collegamento Modifica DataBindings. Specificare quindi che la proprietà `Text` deve essere formattata come numero.

![Formattare la proprietà Text come numero](batch-updating-vb/_static/image14.gif)

**Figura 14**: formattare la proprietà `Text` come numero

In secondo luogo, la casella di controllo viene configurata nella colonna `Discontinued`, anziché allineata a sinistra. Fare clic su Modifica colonne dallo smart tag di GridView e selezionare il `Discontinued` TemplateField dall'elenco di campi nell'angolo in basso a sinistra. Eseguire il drill-down in `ItemStyle` e impostare la proprietà `HorizontalAlign` su Center, come illustrato nella figura 15.

![Centrare la casella di controllo Discontinued](batch-updating-vb/_static/image15.gif)

**Figura 15**: centrare la casella di controllo `Discontinued`

Successivamente, aggiungere un controllo ValidationSummary alla pagina e impostare la relativa proprietà `ShowMessageBox` su `True` e la relativa proprietà `ShowSummary` su `False`. Aggiungere inoltre i controlli Web del pulsante che, quando si fa clic, aggiorneranno le modifiche dell'utente. In particolare, aggiungere due controlli Web Button, uno sopra il GridView e uno sotto di esso, impostando entrambi i controlli `Text` proprietà per aggiornare i prodotti.

Poiché l'interfaccia di modifica di GridView è definita nella relativa TemplateFields `ItemTemplate` s, le `EditItemTemplate` sono superflue e possono essere eliminate.

Dopo aver apportato le modifiche di formattazione sopra menzionate, aggiungendo i controlli Button e rimuovendo il `EditItemTemplate` s non necessario, la sintassi dichiarativa della pagina deve essere simile alla seguente:

[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

La figura 16 Mostra questa pagina quando viene visualizzata tramite un browser dopo l'aggiunta dei controlli Web del pulsante e la formattazione delle modifiche apportate.

[![la pagina include ora due pulsanti Aggiorna prodotti](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: la pagina include ora due pulsanti Aggiorna prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](batch-updating-vb/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Passaggio 5: aggiornamento dei prodotti

Quando un utente visita questa pagina, apporterà le modifiche e quindi farà clic su uno dei due pulsanti Aggiorna prodotti. A questo punto è necessario salvare in qualche modo i valori immessi dall'utente per ogni riga in un'istanza di `ProductsDataTable` e quindi passarli a un metodo BLL che passerà quindi l'istanza `ProductsDataTable` al metodo DAL `UpdateWithTransaction`. Il metodo `UpdateWithTransaction`, creato nell' [esercitazione precedente](wrapping-database-modifications-within-a-transaction-vb.md), assicura che il batch di modifiche venga aggiornato come operazione atomica.

Creare un metodo denominato `BatchUpdate` in `BatchUpdate.aspx.vb` e aggiungere il codice seguente:

[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Questo metodo inizia ottenendo nuovamente tutti i prodotti in una `ProductsDataTable` tramite una chiamata al metodo `GetProducts` di BLL. Viene quindi enumerata la [raccolta di`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)`ProductGrid` GridView. La raccolta di `Rows` contiene un' [istanza di`GridViewRow`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) per ogni riga visualizzata in GridView. Poiché vengono visualizzate al massimo dieci righe per pagina, la raccolta di `Rows` GridView s non avrà più di dieci elementi.

Per ogni riga il `ProductID` viene preso dalla raccolta di `DataKeys` e la `ProductsRow` appropriata viene selezionata dal `ProductsDataTable`. Ai quattro controlli input TemplateField viene fatto riferimento a livello di codice e i relativi valori assegnati alle proprietà dell'istanza di `ProductsRow`. Dopo che sono stati utilizzati i valori di ogni riga GridView per aggiornare la `ProductsDataTable`, vengono passati al metodo `UpdateWithTransaction` BLL, che, come illustrato nell'esercitazione precedente, chiama semplicemente il metodo DAL `UpdateWithTransaction`.

L'algoritmo di aggiornamento batch usato per questa esercitazione aggiorna ogni riga del `ProductsDataTable` che corrisponde a una riga in GridView, indipendentemente dal fatto che le informazioni sul prodotto siano state modificate. Sebbene questi aggiornamenti ciechi non siano in genere un problema di prestazioni, possono causare record superflui se si ricontrollano le modifiche apportate alla tabella di database. Nell'esercitazione [esecuzione degli aggiornamenti batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) è stata esaminata un'interfaccia di aggiornamento batch con DataList e il codice aggiunto che aggiornerà solo i record effettivamente modificati dall'utente. Se lo si desidera, è possibile utilizzare le tecniche di [esecuzione degli aggiornamenti batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) per aggiornare il codice in questa esercitazione.

> [!NOTE]
> Quando si associa l'origine dati a GridView tramite il relativo smart tag, Visual Studio assegna automaticamente i valori di chiave primaria dell'origine dati alla proprietà `DataKeyNames` di GridView. Se il controllo ObjectDataSource non è stato associato a GridView tramite lo smart tag di GridView, come descritto nel passaggio 1, sarà necessario impostare manualmente la proprietà `DataKeyNames` di GridView su ProductID per accedere al valore `ProductID` per ogni riga tramite la raccolta `DataKeys`.

Il codice utilizzato nel `BatchUpdate` è simile a quello utilizzato nei metodi `UpdateProduct` di BLL, la principale differenza è che nei metodi di `UpdateProduct` viene recuperata solo una singola istanza di `ProductRow` dall'architettura. Il codice che assegna le proprietà del `ProductRow` è lo stesso tra i metodi di `UpdateProducts` e il codice all'interno del ciclo di `For Each` in `BatchUpdate`, così come il modello generale.

Per completare questa esercitazione, è necessario che il metodo `BatchUpdate` venga richiamato quando si fa clic su uno dei pulsanti Aggiorna prodotti. Creare gestori eventi per gli eventi `Click` di questi due controlli Button e aggiungere il codice seguente nei gestori eventi:

[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Prima viene effettuata una chiamata a `BatchUpdate`. Successivamente, la [proprietà`ClientScript`](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) viene utilizzata per inserire JavaScript che visualizzerà un MessageBox che legge i prodotti sono stati aggiornati.

Per testare questo codice, è necessario un minuto. Visitare `BatchUpdate.aspx` tramite un browser, modificare un numero di righe e fare clic su uno dei pulsanti Aggiorna prodotti. Supponendo che non siano presenti errori di convalida di input, viene visualizzato un MessageBox che legge i prodotti sono stati aggiornati. Per verificare l'atomicità dell'aggiornamento, è consigliabile aggiungere un vincolo di `CHECK` casuale, ad esempio uno che non consente `UnitPrice` valori di 1234,56. Quindi, da `BatchUpdate.aspx`, modificare un numero di record, assicurandosi di impostare un valore `UnitPrice` del prodotto sul valore non consentito (1234,56). Questo dovrebbe causare un errore quando si fa clic su Aggiorna prodotti con le altre modifiche durante l'operazione batch di cui è stato eseguito il rollback ai valori originali.

## <a name="an-alternativebatchupdatemethod"></a>Metodo`BatchUpdate`alternativo

Il metodo `BatchUpdate` appena esaminato recupera *tutti* i prodotti dal metodo `GetProducts` BLL e quindi aggiorna solo i record visualizzati in GridView. Questo approccio è ideale se GridView non usa il paging, ma in caso contrario, potrebbero essere presenti centinaia, migliaia o decine di migliaia di prodotti, ma solo dieci righe in GridView. In tal caso, il recupero di tutti i prodotti dal database solo per la modifica di 10 è inferiore al valore ideale.

Per questi tipi di situazioni, provare a usare il metodo di `BatchUpdateAlternate` seguente:

[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` inizia creando una nuova `ProductsDataTable` vuota denominata `products`. Viene quindi descritta la raccolta `Rows` di GridView e per ogni riga vengono recuperate le informazioni sul prodotto specifiche utilizzando il metodo `GetProductByProductID(productID)` di BLL. L'istanza di `ProductsRow` recuperata ha le proprietà aggiornate allo stesso modo di `BatchUpdate`, ma dopo l'aggiornamento della riga viene importata nel `products` `ProductsDataTable` tramite il [Metodo](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx)DataTable s`ImportRow(DataRow)`.

Al termine del ciclo di `For Each`, `products` contiene un'istanza di `ProductsRow` per ogni riga in GridView. Poiché ogni istanza di `ProductsRow` è stata aggiunta al `products` (anziché aggiornata), se viene passata in modo cieco al metodo `UpdateWithTransaction` il `ProductsTableAdapter` tenterà di inserire ogni record nel database. Al contrario, è necessario specificare che ognuna di queste righe è stata modificata (non aggiunta).

Questa operazione può essere eseguita aggiungendo un nuovo metodo al BLL denominato `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, illustrato di seguito, imposta la `RowState` di ognuna delle istanze di `ProductsRow` nel `ProductsDataTable` su `Modified`, quindi passa il `ProductsDataTable` al metodo DAL `UpdateWithTransaction`.

[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Riepilogo

GridView fornisce funzionalità di modifica predefinite per riga, ma non supporta la creazione di interfacce completamente modificabili. Come illustrato in questa esercitazione, le interfacce sono possibili, ma richiedono un po' di lavoro. Per creare un controllo GridView in cui ogni riga è modificabile, è necessario convertire i campi di GridView in TemplateFields e definire l'interfaccia di modifica all'interno del `ItemTemplate`. Inoltre, è necessario aggiungere i controlli Web del pulsante Aggiorna tutti i tipi alla pagina, separati da GridView. Questi pulsanti `Click` gestori di eventi devono enumerare la raccolta di `Rows` di GridView, archiviare le modifiche in un `ProductsDataTable`e passare le informazioni aggiornate nel metodo BLL appropriato.

Nell'esercitazione successiva si vedrà come creare un'interfaccia per l'eliminazione batch. In particolare, ogni riga di GridView includerà una casella di controllo e invece dei pulsanti Aggiorna tutti i tipi, verranno eliminati i pulsanti Elimina righe selezionate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e David Suru. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](wrapping-database-modifications-within-a-transaction-vb.md)
> [Successivo](batch-deleting-vb.md)
