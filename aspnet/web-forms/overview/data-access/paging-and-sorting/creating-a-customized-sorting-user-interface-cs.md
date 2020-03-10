---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Creazione di un'interfaccia utente di ordinamento personalizzataC#() | Microsoft Docs
author: rick-anderson
description: Quando si visualizza un lungo elenco di dati ordinati, può essere molto utile raggruppare i dati correlati introducendo le righe del separatore. In questa esercitazione verrà illustrato come cre...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 93ec07a13de80e4c874ff46b5dfa626b60b632c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78619994"
---
# <a name="creating-a-customized-sorting-user-interface-c"></a>Creazione di un'interfaccia utente per l'ordinamento personalizzato (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) o [scaricare il file PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Quando si visualizza un lungo elenco di dati ordinati, può essere molto utile raggruppare i dati correlati introducendo le righe del separatore. In questa esercitazione verrà illustrato come creare un'interfaccia utente di questo tipo.

## <a name="introduction"></a>Introduzione

Quando si visualizza un lungo elenco di dati ordinati in cui sono presenti solo pochi valori diversi nella colonna ordinata, un utente finale potrebbe rivelarsi difficile individuare dove, esattamente, si verificano i limiti di differenza. Nel database, ad esempio, sono presenti 81 prodotti, ma solo nove scelte di categoria diverse (otto categorie univoche più l'opzione `NULL`). Si consideri il caso di un utente interessato a esaminare i prodotti che rientrano nella categoria Seafood. Da una pagina che elenca *tutti* i prodotti in un unico GridView, l'utente potrebbe decidere la scelta migliore per ordinare i risultati in base alla categoria, che raggruppa tutti i prodotti ittici insieme. Dopo l'ordinamento in base alla categoria, l'utente deve quindi cercare nell'elenco la posizione di inizio e fine dei prodotti raggruppati per il pesce. Poiché i risultati vengono ordinati in ordine alfabetico in base al nome di categoria, la ricerca dei prodotti a frutti di pesce non è difficile, ma richiede comunque l'analisi accurata dell'elenco di elementi nella griglia.

Per evidenziare i limiti tra gruppi ordinati, molti siti Web utilizzano un'interfaccia utente che aggiunge un separatore tra tali gruppi. I separatori come quelli mostrati nella figura 1 consentono a un utente di trovare più rapidamente un particolare gruppo e di identificarne i limiti, oltre a verificare quali gruppi distinti sono presenti nei dati.

[![ogni gruppo di categorie è chiaramente identificato](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figura 1**: ogni gruppo di categorie è chiaramente identificato ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image3.png))

In questa esercitazione verrà illustrato come creare un'interfaccia utente di questo tipo.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Passaggio 1: creazione di un GridView standard e ordinabile

Prima di esaminare come aumentare il controllo GridView per fornire l'interfaccia di ordinamento avanzata, è necessario creare prima un GridView standard e ordinabile che elenca i prodotti. Per iniziare, aprire la pagina `CustomSortingUI.aspx` nella cartella `PagingAndSorting`. Aggiungere un controllo GridView alla pagina, impostare la relativa proprietà `ID` su `ProductList`e associarla a un nuovo ObjectDataSource. Configurare ObjectDataSource per utilizzare il metodo di `GetProducts()` della classe `ProductsBLL` per selezionare i record.

Configurare quindi GridView in modo che contenga solo i BoundField `ProductName`, `CategoryName`, `SupplierName`e `UnitPrice` e l'oggetto CheckBoxField sospeso. Infine, configurare GridView per supportare l'ordinamento selezionando la casella di controllo Abilita ordinamento nello smart tag di GridView o impostando la relativa proprietà `AllowSorting` su `true`). Dopo aver apportato queste aggiunte alla pagina `CustomSortingUI.aspx`, il markup dichiarativo avrà un aspetto simile al seguente:

[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

È possibile esaminare lo stato di avanzamento fino a questo punto in un browser. Nella figura 2 viene illustrato il GridView ordinabile quando i dati vengono ordinati in ordine alfabetico in base alla categoria.

[![i dati di GridView ordinabili sono ordinati in base alla categoria](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figura 2**: i dati di GridView ordinabili sono ordinati in base alla categoria ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Passaggio 2: esplorazione delle tecniche per l'aggiunta delle righe dei separatori

Con l'oggetto GridView generico e ordinabile completo, rimane solo la possibilità di aggiungere le righe del separatore in GridView prima di ogni gruppo ordinato univoco. In che modo è possibile inserire tali righe in GridView? Essenzialmente, è necessario eseguire l'iterazione delle righe di GridView, determinare dove si verificano le differenze tra i valori nella colonna ordinata, quindi aggiungere la riga del separatore appropriata. Quando si pensa a questo problema, sembra naturale che la soluzione si trovi in un punto del gestore dell'evento GridView s `RowDataBound`. Come illustrato nell'esercitazione [sulla formattazione personalizzata basata sui dati](../custom-formatting/custom-formatting-based-upon-data-cs.md) , questo gestore eventi viene comunemente usato quando si applica la formattazione a livello di riga in base ai dati della riga. Tuttavia, il gestore dell'evento `RowDataBound` non è la soluzione in questo caso, in quanto le righe non possono essere aggiunte a GridView a livello di codice da questo gestore eventi. La raccolta di `Rows` GridView s, di fatto, è di sola lettura.

Per aggiungere altre righe al GridView sono disponibili tre opzioni:

- Aggiungere queste righe del separatore di metadati ai dati effettivi associati al controllo GridView
- Dopo che GridView è stato associato ai dati, aggiungere altre istanze di `TableRow` alla raccolta di controlli GridView s
- Creare un controllo server personalizzato che estende il controllo GridView ed esegue l'override dei metodi responsabili della creazione della struttura di GridView.

La creazione di un controllo server personalizzato è l'approccio migliore se questa funzionalità era necessaria in molte pagine Web o in più siti Web. Tuttavia, comporterebbe un po' di codice e un'esplorazione approfondita delle attività interne di GridView. Pertanto, questa opzione non verrà considerata in questa esercitazione.

Le altre due opzioni che aggiungono le righe dei separatori ai dati effettivi associati al GridView e la modifica della raccolta di controlli di GridView dopo la relativa associazione, attaccano il problema in modo diverso e meritano una discussione.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Aggiunta di righe ai dati associati a GridView

Quando GridView è associato a un'origine dati, viene creata una `GridViewRow` per ogni record restituito dall'origine dati. Pertanto, è possibile inserire le righe del separatore necessarie aggiungendo record separatori all'origine dati prima di associarlo a GridView. Nella figura 3 viene illustrato questo concetto.

![Una tecnica prevede l'aggiunta di righe di separatore all'origine dati](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figura 3**: una tecnica prevede l'aggiunta di righe di separatore all'origine dati

Utilizzo il termine record separatore tra virgolette perché non esiste alcun record separatore speciale; è piuttosto necessario contrassegnare che un particolare record nell'origine dati funge da separatore anziché da una normale riga di dati. Per gli esempi, viene rilegata un'istanza di `ProductsDataTable` al GridView, che è costituita da `ProductRows`. È possibile contrassegnare un record come una riga di separazione impostando la relativa proprietà `CategoryID` su `-1` (poiché tale valore non può esistere normalmente).

Per usare questa tecnica, è necessario eseguire i passaggi seguenti:

1. Recuperare a livello di codice i dati da associare a GridView (un'istanza di `ProductsDataTable`)
2. Ordinare i dati in base alle proprietà `SortExpression` e `SortDirection` di GridView
3. Scorrere il `ProductsRows` nel `ProductsDataTable`, cercando il punto in cui si trovano le differenze nella colonna ordinata.
4. Al limite di ogni gruppo, inserire un record separatore `ProductsRow` istanza nella DataTable, una `CategoryID` impostata su `-1` (o qualsiasi designazione che sia stata decisa per contrassegnare un record come record separatore)
5. Dopo aver inserito le righe del separatore, associare i dati a livello di codice a GridView

Oltre a questi cinque passaggi, è necessario specificare anche un gestore eventi per l'evento GridView s `RowDataBound`. In questo caso, si controlla ogni `DataRow` e si determina se si tratta di una riga di separatore, una con `CategoryID` impostazione `-1`ta. In tal caso, è probabile che si desideri modificare la formattazione o il testo visualizzato nelle celle.

L'uso di questa tecnica per inserire i limiti del gruppo di ordinamento richiede un po' più di lavoro rispetto a quanto descritto in precedenza, in quanto è necessario fornire anche un gestore eventi per l'evento `Sorting` GridView e tenere traccia dei valori `SortExpression` e `SortDirection`.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Modifica della raccolta di controlli di GridView dopo che è stato associato a un DataBinding

Anziché eseguire la messaggistica dei dati prima di associarli a GridView, è possibile aggiungere le righe del separatore *dopo che* i dati sono stati associati a GridView. Il processo di data binding crea la gerarchia dei controlli GridView, che in realtà è semplicemente un'istanza di `Table` composta da una raccolta di righe, ciascuna delle quali è costituita da una raccolta di celle. In particolare, la raccolta di controlli GridView s contiene un oggetto `Table` alla radice, un `GridViewRow` (derivato dalla classe `TableRow`) per ogni record del `DataSource` associato a GridView e un oggetto `TableCell` in ogni istanza di `GridViewRow` per ogni campo dati nella `DataSource`.

Per aggiungere righe separatore tra ogni gruppo di ordinamento, è possibile modificare direttamente questa gerarchia di controlli dopo che è stata creata. È possibile essere certi che la gerarchia dei controlli di GridView sia stata creata per l'ultima volta nel momento in cui viene eseguito il rendering della pagina. Pertanto, questo approccio esegue l'override del metodo di `Render` della classe `Page`, a quel punto la gerarchia dei controlli finali di GridView viene aggiornata per includere le righe del separatore necessarie. Nella figura 4 viene illustrato questo processo.

[![una tecnica alternativa manipola la gerarchia dei controlli di GridView](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figura 4**: una tecnica alternativa modifica la gerarchia dei controlli di GridView ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image10.png))

Per questa esercitazione verrà usato questo secondo approccio per personalizzare l'esperienza utente di ordinamento.

> [!NOTE]
> Il codice i m presentato in questa esercitazione si basa sull'esempio fornito nel post del Blog di [Keiski](http://aspadvice.com/blogs/joteke/default.aspx) , che [gioca un po' con il raggruppamento di ordinamento di GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Passaggio 3: aggiunta delle righe dei separatori alla gerarchia dei controlli GridView s

Poiché si desidera solo aggiungere le righe del separatore alla gerarchia dei controlli di GridView dopo che la gerarchia dei controlli è stata creata e creata per l'ultima volta nella visita della pagina, è necessario eseguire questa aggiunta alla fine del ciclo di vita della pagina, ma prima dell'effettivo GridView c il rendering della gerarchia ontrollo è stato eseguito in HTML. Il punto più recente in cui è possibile eseguire questa operazione è l'evento `Page` Class s `Render`, che è possibile sostituire nella classe code-behind usando la firma del metodo seguente:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Quando viene richiamato il metodo `Render` originale della classe `Page` `base.Render(writer)` viene eseguito il rendering di ogni controllo della pagina, generando il markup in base alla gerarchia dei controlli. È pertanto fondamentale che entrambi chiamino `base.Render(writer)`, in modo che venga eseguito il rendering della pagina e che si modifichi la gerarchia dei controlli di GridView prima di chiamare `base.Render(writer)`, in modo che le righe del separatore siano state aggiunte alla gerarchia dei controlli di GridView prima che ne venga eseguito il rendering.

Per inserire le intestazioni del gruppo di ordinamento, è necessario innanzitutto verificare che l'utente abbia richiesto l'ordinamento dei dati. Per impostazione predefinita, il contenuto di GridView non è ordinato e pertanto non è necessario immettere intestazioni di ordinamento del gruppo.

> [!NOTE]
> Se si desidera che GridView venga ordinato in base a una determinata colonna quando la pagina viene caricata per la prima volta, chiamare il Metodo GridView s `Sort` nella prima pagina visita (ma non nei postback successivi). A tale scopo, aggiungere la chiamata nel gestore eventi `Page_Load` all'interno di un `if (!Page.IsPostBack)` condizionale. Per ulteriori informazioni sul metodo `Sort`, vedere le informazioni sull'esercitazione [paging e ordinamento dei dati del report](paging-and-sorting-report-data-cs.md) .

Supponendo che i dati siano stati ordinati, l'attività successiva consiste nel determinare la colonna in base alla quale sono stati ordinati i dati e quindi analizzare le righe che cercano le differenze nei valori della colonna. Il codice seguente garantisce che i dati siano stati ordinati e trovi la colonna in base alla quale sono stati ordinati i dati:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Se GridView è ancora stato ordinato, la proprietà `SortExpression` di GridView non sarà impostata. Pertanto, si desidera aggiungere solo le righe del separatore se questa proprietà ha un valore. In tal caso, è necessario determinare l'indice della colonna in base alla quale sono stati ordinati i dati. Questa operazione viene eseguita scorrendo il ciclo della raccolta `Columns` di GridView, cercando la colonna la cui proprietà `SortExpression` è uguale alla proprietà `SortExpression` di GridView. Oltre all'indice della colonna, si acquisisce anche la proprietà `HeaderText`, che viene usata quando si visualizzano le righe del separatore.

Con l'indice della colonna in base al quale vengono ordinati i dati, il passaggio finale consiste nell'enumerare le righe di GridView. Per ogni riga è necessario determinare se il valore della colonna ordinata è diverso dal valore della colonna ordinata della riga precedente. In tal caso, è necessario inserire una nuova istanza di `GridViewRow` nella gerarchia dei controlli. Questa operazione viene eseguita con il codice seguente:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Questo codice inizia a livello di codice facendo riferimento all'oggetto `Table` trovato alla radice della gerarchia dei controlli di GridView e creando una variabile di stringa denominata `lastValue`. `lastValue` viene utilizzato per confrontare il valore della colonna ordinata della riga corrente con il valore della riga precedente. Successivamente, l'insieme di `Rows` GridView s viene enumerato e per ogni riga il valore della colonna ordinata viene archiviato nella variabile `currentValue`.

> [!NOTE]
> Per determinare il valore della colonna ordinata della riga specifica, utilizzo la proprietà `Text` della cella. Questa soluzione funziona bene per i BoundField, ma non per TemplateFields, CheckBoxFields e così via. A breve verrà illustrato come tenere conto dei campi GridView alternativi.

Vengono quindi confrontate le variabili `currentValue` e `lastValue`. Se sono diversi, è necessario aggiungere una nuova riga di separazione alla gerarchia dei controlli. Questa operazione viene eseguita determinando l'indice della `GridViewRow` nella raccolta `Rows` dell'oggetto `Table`, creando nuove istanze `GridViewRow` e `TableCell` e quindi aggiungendo le `TableCell` e `GridViewRow` alla gerarchia dei controlli.

Si noti che il separatore della riga s `TableCell` è formattato in modo che si estenda sull'intera larghezza del GridView, viene formattato usando la classe CSS `SortHeaderRowStyle` e ha la relativa proprietà `Text` in modo che mostri sia il nome del gruppo di ordinamento (ad esempio Category) che il valore del gruppo (ad esempio bevande). Infine, `lastValue` viene aggiornato al valore di `currentValue`.

La classe CSS utilizzata per formattare la riga di intestazione del gruppo di ordinamento `SortHeaderRowStyle` deve essere specificata nel file di `Styles.css`. È possibile usare le impostazioni di stile che si rivolgono all'utente; Ho utilizzato gli elementi seguenti:

[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Con il codice corrente, l'interfaccia di ordinamento aggiunge le intestazioni del gruppo di ordinamento durante l'ordinamento in base a qualsiasi BoundField (vedere la figura 5, che mostra una schermata durante l'ordinamento in base al fornitore). Tuttavia, quando si esegue l'ordinamento in base a qualsiasi altro tipo di campo, ad esempio CheckBoxField o TemplateField, le intestazioni del gruppo di ordinamento non sono disponibili (vedere la figura 6).

[![l'interfaccia di ordinamento include le intestazioni dei gruppi di ordinamento durante l'ordinamento in base a BoundField](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figura 5**: l'interfaccia di ordinamento include le intestazioni dei gruppi di ordinamento durante l'ordinamento in base a BoundField ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image13.png))

[![le intestazioni del gruppo di ordinamento mancanti durante l'ordinamento di un oggetto CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figura 6**: intestazioni del gruppo di ordinamento mancanti durante l'ordinamento di un oggetto CheckBoxField ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image16.png))

Il motivo per cui le intestazioni del gruppo di ordinamento risultano mancanti durante l'ordinamento in base a un oggetto CheckBoxField è dato dal fatto che il codice utilizza attualmente solo la proprietà `Text` `TableCell` s per determinare il valore della colonna ordinata per ogni riga. Per CheckBoxFields, la proprietà `Text` `TableCell` s è una stringa vuota. il valore è invece disponibile tramite un controllo Web CheckBox che risiede all'interno della raccolta di `Controls` `TableCell` s.

Per gestire tipi di campo diversi da BoundField, è necessario aumentare il codice a cui è assegnata la variabile `currentValue` per verificare l'esistenza di una casella di controllo nella raccolta di `Controls` di `TableCell`. Anziché utilizzare `currentValue = gvr.Cells[sortColumnIndex].Text`, sostituire il codice con il codice seguente:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Questo codice esamina la colonna ordinata `TableCell` per la riga corrente per determinare se sono presenti controlli nella raccolta di `Controls`. Se è presente e il primo controllo è una casella di controllo, la variabile `currentValue` è impostata su Yes o no, a seconda della proprietà `Checked` della casella di controllo. In caso contrario, il valore viene ricavato dalla proprietà `Text` di `TableCell`. Questa logica può essere replicata per gestire l'ordinamento per qualsiasi TemplateFields che può esistere in GridView.

Con l'aggiunta del codice precedente, le intestazioni del gruppo di ordinamento sono ora presenti durante l'ordinamento in base a CheckBoxField Discontinued (vedere la figura 7).

[![le intestazioni del gruppo di ordinamento sono ora presenti durante l'ordinamento di un oggetto CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figura 7**: le intestazioni del gruppo di ordinamento sono ora presenti durante l'ordinamento di un oggetto CheckBoxField ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-customized-sorting-user-interface-cs/_static/image19.png))

> [!NOTE]
> Se si dispone di prodotti con `NULL` valori di database per i campi `CategoryID`, `SupplierID`o `UnitPrice`, tali valori verranno visualizzati come stringhe vuote in GridView per impostazione predefinita, vale a dire che il testo della riga separatore per quei prodotti con `NULL` valori verrà letto come Category: (ovvero, non esiste alcun nome dopo category: like con category: beverages). Se si vuole che venga visualizzato un valore, è possibile impostare la proprietà BoundFields [`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) sul testo da visualizzare oppure è possibile aggiungere un'istruzione condizionale nel metodo Render quando si assegnano i `currentValue` alla proprietà `Text` della riga del separatore.

## <a name="summary"></a>Riepilogo

GridView non include molte opzioni predefinite per la personalizzazione dell'interfaccia di ordinamento. Tuttavia, con un po' di codice di basso livello, è possibile modificare la gerarchia dei controlli di GridView per creare un'interfaccia più personalizzata. In questa esercitazione è stato illustrato come aggiungere una riga di separatore del gruppo di ordinamento per un GridView ordinabile, che identifica più facilmente i limiti dei gruppi e dei gruppi distinti. Per altri esempi di interfacce di ordinamento personalizzate, vedere il post di Blog relativo a [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [alcuni suggerimenti per l'ordinamento di ASP.NET 2,0 GridView](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) .

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](sorting-custom-paged-data-cs.md)
> [Successivo](paging-and-sorting-report-data-vb.md)
