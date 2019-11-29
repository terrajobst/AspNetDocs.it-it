---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Aggiunta di colonne DataTable aggiuntive (VB) | Microsoft Docs
author: rick-anderson
description: Quando si utilizza la procedura guidata TableAdapter per creare un DataSet tipizzato, l'oggetto DataTable corrispondente contiene le colonne restituite dalla query del database principale. Ma...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a55f8bc4d3508387927ca81674073a001867de7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74608468"
---
# <a name="adding-additional-datatable-columns-vb"></a>Aggiunta di colonne DataTable aggiuntive (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) o [Scarica PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Quando si utilizza la procedura guidata TableAdapter per creare un DataSet tipizzato, l'oggetto DataTable corrispondente contiene le colonne restituite dalla query del database principale. In alcuni casi, tuttavia, è necessario che la DataTable includa colonne aggiuntive. In questa esercitazione viene illustrato il motivo per cui le stored procedure sono consigliate quando sono necessarie colonne DataTable aggiuntive.

## <a name="introduction"></a>Introduzione

Quando si aggiunge un TableAdapter a un DataSet tipizzato, lo schema di DataTable corrispondente è determinato dalla query principale di TableAdapter. Se, ad esempio, la query principale restituisce i campi dati *A*, *b*e *c*, la DataTable avrà tre colonne corrispondenti denominate *a*, *b*e *c*. Oltre alla query principale, un TableAdapter può includere query aggiuntive che restituiscono, forse, un subset di dati in base a un parametro. Ad esempio, oltre alla query principale `ProductsTableAdapter` s, che restituisce informazioni su tutti i prodotti, contiene anche metodi quali `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`che restituiscono informazioni specifiche sul prodotto basate su un parametro fornito.

Il modello di cui lo schema di DataTable riflette la query principale di TableAdapter è adatto se tutti i metodi di TableAdapter restituiscono lo stesso o un minor numero di campi dati rispetto a quelli specificati nella query principale. Se un metodo TableAdapter deve restituire campi dati aggiuntivi, è necessario espandere di conseguenza lo schema di DataTable. In [Master/Detail usando un elenco puntato di record master con un'esercitazione DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) è stato aggiunto un metodo al `CategoriesTableAdapter` che ha restituito i campi dati `CategoryID`, `CategoryName`e `Description` definiti nella query principale più `NumberOfProducts`, un campo dati aggiuntivo che ha riportato il numero di prodotti associati a ogni categoria. È stata aggiunta manualmente una nuova colonna al `CategoriesDataTable` per acquisire il valore del campo dati `NumberOfProducts` da questo nuovo metodo.

Come illustrato nell'esercitazione sul [caricamento di file](../working-with-binary-files/uploading-files-vb.md) , è necessario prestare particolare attenzione agli oggetti TableAdapter che usano istruzioni SQL ad hoc e che dispongono di metodi i cui campi dati non corrispondono esattamente alla query principale. Se la configurazione guidata TableAdapter viene eseguita di nuovo, aggiornerà tutti i metodi di TableAdapter in modo che l'elenco dei campi dati corrisponda alla query principale. Di conseguenza, qualsiasi metodo con elenchi di colonne personalizzati verrà ripristinato nell'elenco delle colonne della query principale e non restituirà i dati previsti. Questo problema non si verifica quando si utilizzano stored procedure.

In questa esercitazione verrà esaminato come estendere uno schema di DataTable per includere colonne aggiuntive. A causa della fragilità del TableAdapter quando si usano istruzioni SQL ad hoc, in questa esercitazione si useranno le stored procedure. Per ulteriori informazioni sulla configurazione di un TableAdapter per l'utilizzo di stored procedure, vedere la pagina relativa alla [creazione di nuove stored procedure per i TableAdapter del set di dati tipizzati](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) e all' [utilizzo di stored procedure esistenti per le esercitazioni di TableAdapter tipizzati del DataSet](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Passaggio 1: aggiunta di una colonna`PriceQuartile`al`ProductsDataTable`

Nell'esercitazione *creazione di nuove stored procedure per set di dati tipizzati s TableAdapters* è stato creato un set di dati tipizzato denominato `NorthwindWithSprocs`. Questo set di dati contiene attualmente due DataTable: `ProductsDataTable` e `EmployeesDataTable`. Il `ProductsTableAdapter` dispone dei tre metodi seguenti:

- `GetProducts`: la query principale, che restituisce tutti i record della tabella `Products`
- `GetProductsByCategoryID(categoryID)`: restituisce tutti i prodotti con il parametro *CategoryID*specificato.
- `GetProductByProductID(productID)`: restituisce il prodotto specifico con il *ProductID*specificato.

La query principale e i due metodi aggiuntivi restituiscono tutti lo stesso set di campi dati, vale a dire tutte le colonne della tabella `Products`. Non sono presenti sottoquery correlate o `JOIN` s che effettua il pull dei dati correlati dalle tabelle `Categories` o `Suppliers`. Pertanto, il `ProductsDataTable` dispone di una colonna corrispondente per ogni campo nella tabella `Products`.

Per questa esercitazione, è possibile aggiungere un metodo al `ProductsTableAdapter` denominato `GetProductsWithPriceQuartile` che restituisce tutti i prodotti. Oltre ai campi di dati di prodotto standard, `GetProductsWithPriceQuartile` includerà anche un campo dati `PriceQuartile` che indica in quale quartile il prezzo del prodotto è inferiore. Ad esempio, i prodotti il cui prezzo è il 25% più costoso avranno un valore di `PriceQuartile` pari a 1, mentre quelli i cui prezzi rientrano nel 25% in basso avranno un valore pari a 4. Prima di preoccuparsi di creare il stored procedure per restituire queste informazioni, è necessario prima di tutto aggiornare i `ProductsDataTable` per includere una colonna che contenga i risultati del `PriceQuartile` quando viene usato il metodo `GetProductsWithPriceQuartile`.

Aprire il set di dati `NorthwindWithSprocs` e fare clic con il pulsante destro del mouse sulla `ProductsDataTable`. Scegliere Aggiungi dal menu di scelta rapida e quindi selezionare colonna.

[![aggiungere una nuova colonna a ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Figura 1**: aggiungere una nuova colonna al `ProductsDataTable` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image3.png))

Verrà aggiunta una nuova colonna alla DataTable denominata Column1 di tipo `System.String`. È necessario aggiornare il nome della colonna in PriceQuartile e il tipo in `System.Int32` perché verrà usato per contenere un numero compreso tra 1 e 4. Selezionare la colonna appena aggiunta nel `ProductsDataTable` e, dal Finestra Proprietà, impostare la proprietà `Name` su PriceQuartile e la proprietà `DataType` su `System.Int32`.

[![impostare il nome e le proprietà del tipo di dati della nuova colonna](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Figura 2**: impostare la nuova colonna s `Name` e `DataType` proprietà ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image6.png))

Come illustrato nella figura 2, sono disponibili proprietà aggiuntive che possono essere impostate, ad esempio se i valori nella colonna devono essere univoci, se la colonna è una colonna a incremento automatico, indipendentemente dal fatto che i valori `NULL` database siano consentiti e così via. Lasciare questi valori impostati sulle impostazioni predefinite.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Passaggio 2: creazione del metodo`GetProductsWithPriceQuartile`

Ora che il `ProductsDataTable` è stato aggiornato in modo da includere la colonna `PriceQuartile`, è possibile creare il metodo di `GetProductsWithPriceQuartile`. Per iniziare, fare clic con il pulsante destro del mouse sul TableAdapter e scegliere Aggiungi query dal menu di scelta rapida. Viene visualizzata la configurazione guidata query TableAdapter, che richiede prima di tutto se si desidera utilizzare istruzioni SQL ad hoc o una stored procedure nuova o esistente. Poiché non è ancora presente un stored procedure che restituisce i dati dei quartili del prezzo, consentire al TableAdapter di creare questo stored procedure per Microsoft. Selezionare l'opzione Crea nuovo stored procedure e fare clic su Avanti.

[![indicare alla procedura guidata TableAdapter di creare la stored procedure per gli Stati Uniti](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Figura 3**: istruzioni per la creazione della stored procedure da parte della procedura guidata TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image9.png))

Nella schermata successiva, illustrata nella figura 4, la procedura guidata richiede il tipo di query da aggiungere. Poiché il metodo `GetProductsWithPriceQuartile` restituirà tutte le colonne e i record della tabella `Products`, selezionare l'opzione SELECT that returns Rows e fare clic su Next.

[![la query sarà un'istruzione SELECT che restituisce più righe](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Figura 4**: la query sarà un'istruzione `SELECT` che restituisce più righe ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image12.png))

Viene quindi richiesto di `SELECT` query. Immettere la query seguente nella procedura guidata:

[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

La query precedente usa SQL Server 2005 s nuova [funzione`NTILE`](https://msdn.microsoft.com/library/ms175126.aspx) per dividere i risultati in quattro gruppi in cui i gruppi sono determinati dai valori di `UnitPrice` ordinati in ordine decrescente.

Sfortunatamente, l'Generatore di query non sa come analizzare la parola chiave `OVER` e verrà visualizzato un errore durante l'analisi della query precedente. Quindi, immettere la query precedente direttamente nella casella di testo della procedura guidata senza usare il Generatore di query.

> [!NOTE]
> Per ulteriori informazioni su NTILE e SQL Server 2005 s altre funzioni di rango, vedere [restituzione di risultati classificati con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e la [sezione funzioni di rango](https://msdn.microsoft.com/library/ms189798.aspx) dalla [documentazione online di SQL Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).

Dopo aver immesso la query `SELECT` e aver fatto clic su Avanti, nella procedura guidata viene richiesto di specificare un nome per il stored procedure che verrà creato. Denominare il nuovo stored procedure `Products_SelectWithPriceQuartile` e fare clic su Avanti.

[![nome della stored procedure Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Figura 5**: assegnare un nome alla stored procedure `Products_SelectWithPriceQuartile` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image15.png))

Infine, viene richiesto di assegnare un nome ai metodi TableAdapter. Lasciare selezionate le caselle di controllo Fill a DataTable e return a DataTable e assegnare un nome ai metodi `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.

[![assegnare un nome ai metodi TableAdapter e fare clic su fine](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Figura 6**: assegnare un nome ai metodi di TableAdapter e fare clic su Finish ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image18.png))

Con la query `SELECT` specificata e i metodi stored procedure e TableAdapter denominati, fare clic su fine per completare la procedura guidata. A questo punto, è possibile che venga visualizzato un avviso o due dalla procedura guidata che informa che l'`OVER` costrutto o l'istruzione SQL non è supportata. Questi avvisi possono essere ignorati.

Al termine della procedura guidata, il TableAdapter deve includere i metodi `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` e il database deve includere un stored procedure denominato `Products_SelectWithPriceQuartile`. È necessario verificare che il TableAdapter contenga effettivamente questo nuovo metodo e che il stored procedure sia stato aggiunto correttamente al database. Quando si esegue il controllo del database, se non viene visualizzato il stored procedure provare a fare clic con il pulsante destro del mouse sulla cartella stored procedure e scegliere Aggiorna.

![Verificare che sia stato aggiunto un nuovo metodo al TableAdapter](adding-additional-datatable-columns-vb/_static/image19.png)

**Figura 7**: verificare che sia stato aggiunto un nuovo metodo al TableAdapter

[![verificare che il database contenga la stored procedure Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Figura 8**: verificare che il database contenga la stored procedure `Products_SelectWithPriceQuartile` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image22.png))

> [!NOTE]
> Uno dei vantaggi dell'utilizzo di stored procedure anziché di istruzioni SQL ad hoc è che la riesecuzione della configurazione guidata TableAdapter non modificherà gli elenchi di colonne delle stored procedure. Per verificarlo, fare clic con il pulsante destro del mouse sul TableAdapter, scegliere l'opzione Configura dal menu di scelta rapida per avviare la procedura guidata e quindi fare clic su fine per completare l'operazione. Passare quindi al database e visualizzare il `Products_SelectWithPriceQuartile` stored procedure. Si noti che il relativo elenco di colonne non è stato modificato. Se venivano utilizzate istruzioni SQL ad hoc, la nuova esecuzione della configurazione guidata TableAdapter avrebbe ripristinato l'elenco delle colonne della query in modo che corrisponda all'elenco di colonne di query principale, rimuovendo quindi l'istruzione NTILE dalla query utilizzata dal metodo `GetProductsWithPriceQuartile`.

Quando viene richiamato il metodo di accesso ai dati `GetProductsWithPriceQuartile`, il TableAdapter esegue il stored procedure di `Products_SelectWithPriceQuartile` e aggiunge una riga al `ProductsDataTable` per ogni record restituito. Viene eseguito il mapping dei campi dati restituiti dal stored procedure alle colonne `ProductsDataTable` s. Poiché è presente un `PriceQuartile` campo dati restituito dalla stored procedure, il relativo valore viene assegnato alla colonna `PriceQuartile` `ProductsDataTable` s.

Per i metodi TableAdapter le cui query non restituiscono un campo dati `PriceQuartile`, il valore della colonna `PriceQuartile` è il valore specificato dalla relativa proprietà `DefaultValue`. Come illustrato nella figura 2, questo valore è impostato su `DBNull`, il valore predefinito. Se si preferisce un valore predefinito diverso, è sufficiente impostare di conseguenza la proprietà `DefaultValue`. È sufficiente assicurarsi che il valore `DefaultValue` sia valido in base alle `DataType` della colonna (ad esempio, `System.Int32` per la colonna `PriceQuartile`).

A questo punto sono stati eseguiti i passaggi necessari per aggiungere una colonna aggiuntiva a una DataTable. Per verificare che la colonna aggiuntiva funzioni come previsto, è necessario creare una pagina ASP.NET che visualizzi il nome, il prezzo e il quartile del prezzo di ogni prodotto. Prima di procedere, tuttavia, è necessario prima aggiornare il livello della logica di business per includere un metodo che chiama il metodo DAL `GetProductsWithPriceQuartile`. Il livello BLL verrà aggiornato successivamente, nel passaggio 3, quindi si creerà la pagina ASP.NET nel passaggio 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Passaggio 3: incremento del livello della logica di business

Prima di usare il nuovo metodo `GetProductsWithPriceQuartile` dal livello di presentazione, è necessario innanzitutto aggiungere un metodo corrispondente a BLL. Aprire il file di classe `ProductsBLLWithSprocs` e aggiungere il codice seguente:

[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Analogamente agli altri metodi di recupero dei dati in `ProductsBLLWithSprocs`, il metodo `GetProductsWithPriceQuartile` chiama semplicemente il metodo del `GetProductsWithPriceQuartile` corrispondente e ne restituisce i risultati.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Passaggio 4: visualizzazione delle informazioni sui quartili del prezzo in una pagina Web di ASP.NET

Con il completamento dell'aggiunta di BLL, è possibile creare una pagina ASP.NET che mostri il prezzo quartile per ogni prodotto. Aprire la pagina `AddingColumns.aspx` nella cartella `AdvancedDAL` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa proprietà `ID` su `Products`. Dallo smart tag GridView s, associarlo a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per l'uso del metodo `ProductsBLLWithSprocs` Class s `GetProductsWithPriceQuartile`. Poiché si tratta di una griglia di sola lettura, impostare gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE su (nessuno).

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Figura 9**: configurare ObjectDataSource per l'uso della classe `ProductsBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image25.png))

[![recuperare informazioni sul prodotto dal metodo GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Figura 10**: recuperare informazioni sul prodotto dal metodo `GetProductsWithPriceQuartile` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image28.png))

Dopo aver completato la configurazione guidata origine dati, in Visual Studio verrà automaticamente aggiunto un BoundField o CheckBoxField a GridView per ognuno dei campi dati restituiti dal metodo. Uno di questi campi dati è `PriceQuartile`, ovvero la colonna aggiunta al `ProductsDataTable` nel passaggio 1.

Modificare i campi di GridView, rimuovendo tutti i BoundField tranne i `ProductName`, `UnitPrice`e `PriceQuartile`. Configurare il BoundField `UnitPrice` per formattare il valore come valuta e avere rispettivamente i BoundField `UnitPrice` e `PriceQuartile` a destra e allineati al centro. Aggiornare infine i BoundField rimanenti `HeaderText` proprietà rispettivamente a Product, Price e Price quartil. Inoltre, selezionare la casella di controllo Abilita ordinamento dallo smart tag di GridView.

Dopo queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

La figura 11 Mostra questa pagina quando viene visitata attraverso un browser. Si noti che, inizialmente, i prodotti sono ordinati in base al prezzo in ordine decrescente, a ogni prodotto viene assegnato un valore `PriceQuartile` appropriato. Naturalmente questi dati possono essere ordinati in base ad altri criteri con il valore di colonna price quartil che riflette ancora la classificazione dei prodotti rispetto al prezzo (vedere la figura 12).

[![i prodotti sono ordinati in base ai prezzi](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Figura 11**: i prodotti sono ordinati in base ai prezzi ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image31.png))

[![i prodotti sono ordinati in base ai rispettivi nomi](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Figura 12**: i prodotti sono ordinati in base ai rispettivi nomi ([fare clic per visualizzare l'immagine con dimensioni complete](adding-additional-datatable-columns-vb/_static/image34.png))

> [!NOTE]
> Con poche righe di codice è possibile aumentare il controllo GridView in modo che abbia colorato le righe del prodotto in base al valore `PriceQuartile`. Potremmo colorare questi prodotti nel primo quartile un verde chiaro, quelli nel secondo quartile un giallo chiaro e così via. Si consiglia di richiedere qualche istante per aggiungere questa funzionalità. Se è necessario un aggiornamento per la formattazione di GridView, vedere l'esercitazione [sulla formattazione personalizzata basata sui dati](../custom-formatting/custom-formatting-based-upon-data-vb.md) .

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un approccio alternativo-creazione di un altro TableAdapter

Come illustrato in questa esercitazione, quando si aggiunge un metodo a un TableAdapter che restituisce campi dati diversi da quelli digitati dalla query principale, è possibile aggiungere colonne corrispondenti alla DataTable. Questo approccio, tuttavia, funziona correttamente solo se è presente un numero ridotto di metodi nell'oggetto TableAdapter che restituiscono campi dati diversi e se i campi dati alternativi non variano troppo dalla query principale.

Anziché aggiungere colonne alla DataTable, è possibile aggiungere un altro TableAdapter al set di dati che contiene i metodi del primo TableAdapter che restituiscono campi dati diversi. Per questa esercitazione, anziché aggiungere la colonna `PriceQuartile` al `ProductsDataTable` (dove viene usata solo dal metodo di `GetProductsWithPriceQuartile`), è possibile che sia stato aggiunto un TableAdapter aggiuntivo al set di dati denominato `ProductsWithPriceQuartileTableAdapter` che usava `Products_SelectWithPriceQuartile` stored procedure come query principale. Le pagine ASP.NET per le quali è necessario ottenere informazioni sui prodotti con il prezzo quartile utilizzeranno il `ProductsWithPriceQuartileTableAdapter`, mentre quelle che non potevano continuare a usare il `ProductsTableAdapter`.

Con l'aggiunta di un nuovo TableAdapter, le DataTable rimangono inalterate e le rispettive colonne riflettono esattamente i campi dati restituiti dai relativi metodi TableAdapter. Tuttavia, gli oggetti TableAdapter aggiuntivi possono introdurre attività e funzionalità ripetitive. Se, ad esempio, le pagine di ASP.NET che hanno visualizzato la colonna `PriceQuartile` necessario fornire il supporto per l'inserimento, l'aggiornamento e l'eliminazione, è necessario che le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` del `ProductsWithPriceQuartileTableAdapter` siano configurate correttamente. Sebbene queste proprietà rispecchino il `ProductsTableAdapter` s, questa configurazione introduce un passaggio aggiuntivo. Sono inoltre disponibili due modi per aggiornare, eliminare o aggiungere un prodotto al database, tramite le classi `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter`.

Il download di questa esercitazione include una classe `ProductsWithPriceQuartileTableAdapter` nel set di dati `NorthwindWithSprocs` che illustra questo approccio alternativo.

## <a name="summary"></a>Riepilogo

Nella maggior parte degli scenari, tutti i metodi in un oggetto TableAdapter restituiranno lo stesso set di campi dati, ma in alcuni casi è possibile che un metodo o due metodi particolari debbano restituire un campo aggiuntivo. Ad esempio, nel [master/dettagli utilizzando un elenco puntato di record master con un'esercitazione DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) è stato aggiunto un metodo al `CategoriesTableAdapter` che, oltre ai campi dati della query principale, ha restituito un `NumberOfProducts` campo che riporta il numero di prodotti associati a ogni categoria. In questa esercitazione è stato esaminato l'aggiunta di un metodo nella `ProductsTableAdapter` che ha restituito un campo di `PriceQuartile` oltre ai campi dati della query principale. Per acquisire campi dati aggiuntivi restituiti dai metodi TableAdapter è necessario aggiungere le colonne corrispondenti alla DataTable.

Se si prevede di aggiungere manualmente colonne alla DataTable, è consigliabile usare le stored procedure per il TableAdapter. Se il TableAdapter usa istruzioni SQL ad hoc, ogni volta che viene eseguita la configurazione guidata TableAdapter, tutti gli elenchi di campi dei dati dei metodi ripristinano i campi dati restituiti dalla query principale. Questo problema non si estende alle stored procedure, motivo per cui sono consigliate e sono state usate in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Randy Schmidt, Jacky Goor, Bernadette Leigh e Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](updating-the-tableadapter-to-use-joins-vb.md)
> [Successivo](working-with-computed-columns-vb.md)
