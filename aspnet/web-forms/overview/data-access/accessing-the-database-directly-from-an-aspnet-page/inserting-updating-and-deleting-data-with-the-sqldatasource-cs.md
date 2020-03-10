---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Inserimento, aggiornamento ed eliminazione di dati con SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato illustrato il modo in cui il controllo ObjectDataSource consentiva di inserire, aggiornare ed eliminare dati. Il controllo SqlDataSource supporta t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 495e0e81a67e6926e1c4fa92e29ebbda747cd418
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78552675"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Inserimento, aggiornamento ed eliminazione dei dati con SqlDataSource (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) o [scaricare il file PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Nelle esercitazioni precedenti è stato illustrato il modo in cui il controllo ObjectDataSource consentiva di inserire, aggiornare ed eliminare dati. Il controllo SqlDataSource supporta le stesse operazioni, ma l'approccio è diverso e in questa esercitazione viene illustrato come configurare SqlDataSource per inserire, aggiornare ed eliminare i dati.

## <a name="introduction"></a>Introduzione

Come descritto in [una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), il controllo GridView fornisce funzionalità predefinite di aggiornamento ed eliminazione, mentre i controlli DetailsView e FormView includono il supporto per l'inserimento e la modifica e l'eliminazione delle funzionalità. Queste funzionalità di modifica dei dati possono essere inserite direttamente in un controllo origine dati senza che sia necessario scrivere una riga di codice. [Panoramica dell'inserimento, dell'aggiornamento e dell'eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esaminati tramite ObjectDataSource per facilitare l'inserimento, l'aggiornamento e l'eliminazione con i controlli GridView, DetailsView e FormView. In alternativa, è possibile utilizzare SqlDataSource al posto di ObjectDataSource.

Tenere presente che per supportare l'inserimento, l'aggiornamento e l'eliminazione, con ObjectDataSource è necessario specificare i metodi a livello di oggetto da richiamare per eseguire l'azione di inserimento, aggiornamento o eliminazione. Con il SqlDataSource è necessario fornire `INSERT`, `UPDATE`e `DELETE` istruzioni SQL (o stored procedure) da eseguire. Come si vedrà in questa esercitazione, queste istruzioni possono essere create manualmente oppure possono essere generate automaticamente dalla configurazione guidata origine dati di SqlDataSource.

> [!NOTE]
> Poiché sono già state illustrate le funzionalità di inserimento, modifica ed eliminazione dei controlli GridView, DetailsView e FormView, questa esercitazione si concentrerà sulla configurazione del controllo SqlDataSource per supportare queste operazioni. Se è necessario eseguire il pennello sull'implementazione di queste funzionalità all'interno di GridView, DetailsView e FormView, tornare alle esercitazioni relative a modifica, inserimento ed eliminazione dei dati, a partire da [una panoramica dell'inserimento, dell'aggiornamento e dell'eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Passaggio 1: specifica di istruzioni`INSERT`,`UPDATE`e`DELETE`

Come illustrato nelle ultime due esercitazioni, per recuperare i dati da un controllo SqlDataSource è necessario impostare due proprietà:

1. `ConnectionString`, che specifica il database a cui inviare la query e
2. `SelectCommand`, che specifica l'istruzione SQL ad hoc o il nome stored procedure da eseguire per restituire i risultati.

Per `SelectCommand` valori con parametri, i valori dei parametri vengono specificati tramite la raccolta di `SelectParameters` SqlDataSource e possono includere valori hardcoded, valori di origine di parametri comuni (campi QueryString, variabili di sessione, valori di controlli Web e così via) oppure possono essere assegnati a livello di codice. Quando il controllo SqlDataSource s `Select()` metodo viene richiamato a livello di codice o automaticamente da un controllo Web dati, viene stabilita una connessione al database, i valori dei parametri vengono assegnati alla query e il comando viene collegato al database. I risultati vengono quindi restituiti come DataSet o DataReader, a seconda del valore della proprietà `DataSourceMode` del controllo.

Insieme alla selezione dei dati, è possibile utilizzare il controllo SqlDataSource per inserire, aggiornare ed eliminare dati fornendo `INSERT`, `UPDATE`e `DELETE` istruzioni SQL nello stesso modo. È sufficiente assegnare le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` le istruzioni SQL `INSERT`, `UPDATE`e `DELETE` da eseguire. Se le istruzioni hanno parametri (come sempre), includerle nelle raccolte `InsertParameters`, `UpdateParameters`e `DeleteParameters`.

Dopo che è stato specificato un valore `InsertCommand`, `UpdateCommand`o `DeleteCommand`, l'opzione Abilita inserimento, Abilita modifica o Abilita eliminazione nello smart tag del controllo Web dei dati corrispondente diventerà disponibile. Per illustrare questo problema, è possibile prendere un esempio dalla pagina `Querying.aspx` creata nell'esercitazione sull' [esecuzione di query sui dati con il controllo SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) e potenziarla per includere le funzionalità di eliminazione.

Per iniziare, aprire il `InsertUpdateDelete.aspx` e `Querying.aspx` pagine dalla cartella `SqlDataSource`. Nella pagina `Querying.aspx` della finestra di progettazione selezionare SqlDataSource e GridView dal primo esempio (controlli `ProductsDataSource` e `GridView1`). Dopo aver selezionato i due controlli, scegliere Copia dal menu modifica oppure premere CTRL + C. Passare quindi alla finestra di progettazione di `InsertUpdateDelete.aspx` e incollare i controlli. Dopo aver spostato i due controlli su `InsertUpdateDelete.aspx`, testare la pagina in un browser. Verranno visualizzati i valori delle colonne `ProductID`, `ProductName`e `UnitPrice` per tutti i record nella tabella di database `Products`.

[![tutti i prodotti sono elencati, ordinati in base a ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: tutti i prodotti sono elencati, ordinati per `ProductID` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Aggiunta delle proprietà`DeleteCommand`e`DeleteParameters`di SqlDataSource

A questo punto è presente un SqlDataSource che restituisce semplicemente tutti i record della tabella `Products` e GridView che esegue il rendering dei dati. L'obiettivo consiste nell'estendere questo esempio per consentire all'utente di eliminare i prodotti tramite GridView. A tale scopo, è necessario specificare i valori per le proprietà del controllo SqlDataSource `DeleteCommand` e `DeleteParameters` e quindi configurare GridView per supportare l'eliminazione.

È possibile specificare le proprietà `DeleteCommand` e `DeleteParameters` in diversi modi:

- Tramite la sintassi dichiarativa
- Dal Finestra Proprietà nella finestra di progettazione
- Dalla schermata specificare un'istruzione SQL personalizzata o stored procedure nella configurazione guidata origine dati
- Tramite il pulsante Avanzate nella finestra di dialogo impostazione colonne da una tabella di visualizzazione della configurazione guidata origine dati, che genererà automaticamente il `DELETE` istruzione SQL e la raccolta di parametri utilizzati nelle proprietà `DeleteCommand` e `DeleteParameters`

Si esaminerà come avere automaticamente l'istruzione `DELETE` creata nel passaggio 2. Per il momento, è possibile utilizzare il Finestra Proprietà nella finestra di progettazione, anche se la configurazione guidata origine dati o la sintassi dichiarativa funzionerebbe anche.

Nella finestra di progettazione `InsertUpdateDelete.aspx`fare clic sul `ProductsDataSource` SqlDataSource, quindi visualizzare il Finestra Proprietà (dal menu Visualizza scegliere Finestra Proprietà o semplicemente premere F4). Selezionare la proprietà DeleteQuery, che consente di visualizzare un set di ellissi.

![Selezionare la proprietà DeleteQuery nella finestra Proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figura 2**: selezionare la proprietà DeleteQuery nella finestra Proprietà

> [!NOTE]
> Il SqlDataSource doesn t ha una proprietà DeleteQuery. DeleteQuery è invece una combinazione delle proprietà `DeleteCommand` e `DeleteParameters` e viene elencata solo nel Finestra Proprietà quando si visualizza la finestra tramite la finestra di progettazione. Se si esaminano le Finestra Proprietà nella visualizzazione origine, si troverà invece la proprietà `DeleteCommand`.

Fare clic sui puntini di sospensione nella proprietà DeleteQuery per visualizzare la finestra di dialogo Editor comandi e parametri (vedere la figura 3). Da questa finestra di dialogo è possibile specificare l'istruzione SQL `DELETE` e specificare i parametri. Immettere la query seguente nella casella di testo Command `DELETE` (manualmente o usando il Generatore di query, se si preferisce):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Fare quindi clic sul pulsante Aggiorna parametri per aggiungere il parametro `@ProductID` all'elenco dei parametri riportati di seguito.

[![selezionare la proprietà DeleteQuery nella finestra Proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: selezionare la proprietà DeleteQuery nella finestra Proprietà ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))

*Non* specificare un valore per questo parametro (lasciare l'origine del parametro su None). Quando si aggiunge il supporto per l'eliminazione a GridView, GridView fornirà automaticamente questo valore di parametro, usando il valore della raccolta di `DataKeys` per la riga di cui è stato fatto clic sul pulsante Elimina.

> [!NOTE]
> Il nome del parametro utilizzato nella query di `DELETE` *deve* corrispondere al nome del valore `DataKeyNames` in GridView, DetailsView o FormView. Ciò significa che il parametro nell'istruzione `DELETE` viene denominato espressamente `@ProductID` (anziché, Say, `@ID`), perché il nome della colonna chiave primaria nella tabella Products (e pertanto il valore al DataKeyNames in GridView) viene `ProductID`.

Se il nome del parametro e il valore di `DataKeyNames` non corrispondono, GridView non può assegnare automaticamente il parametro al valore della raccolta di `DataKeys`.

Dopo aver immesso le informazioni relative all'eliminazione nella finestra di dialogo Editor comandi e parametri, fare clic su OK e passare alla visualizzazione origine per esaminare il markup dichiarativo risultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Si noti l'aggiunta della proprietà `DeleteCommand`, nonché la sezione `<DeleteParameters>` e l'oggetto Parameter denominato `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurazione di GridView per l'eliminazione

Con la proprietà `DeleteCommand` aggiunta, lo smart tag GridView s contiene ora l'opzione Abilita eliminazione. Procedere e selezionare questa casella di controllo. Come descritto in [una panoramica dell'inserimento, dell'aggiornamento e dell'eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), in GridView viene aggiunto un oggetto CommandField con la relativa proprietà `ShowDeleteButton` impostata su `true`. Come illustrato nella figura 4, quando la pagina viene visitata tramite un browser viene incluso un pulsante Elimina. Testare questa pagina eliminando alcuni prodotti.

[![ogni riga GridView include ora un pulsante Elimina](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: ogni riga GridView include ora un pulsante Elimina ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))

Quando si fa clic su un pulsante Elimina, viene eseguito un postback, GridView assegna il parametro `ProductID` il valore del valore della raccolta `DataKeys` per la riga il cui pulsante Elimina è stato selezionato e richiama il Metodo SqlDataSource s `Delete()`. Il controllo SqlDataSource si connette quindi al database ed esegue l'istruzione `DELETE`. Il controllo GridView viene quindi riassociato al SqlDataSource, ottenendo nuovamente e visualizzando il set corrente di prodotti (che non include più il record appena eliminato).

> [!NOTE]
> Poiché GridView usa la raccolta `DataKeys` per popolare i parametri SqlDataSource, è fondamentale che la proprietà `DataKeyNames` di GridView sia impostata sulle colonne che costituiscono la chiave primaria e che il `SelectCommand` di SqlDataSource restituisca tali colonne. Inoltre, è importante che il nome del parametro in SqlDataSource s `DeleteCommand` sia impostato su `@ProductID`. Se la proprietà `DataKeyNames` non è impostata o il parametro non è denominato `@ProductsID`, facendo clic sul pulsante Elimina verrà generato un postback, ma non verrà eliminato alcun record.

Nella figura 5 viene illustrata graficamente questa interazione. Per una discussione più dettagliata sulla catena di eventi associati all'inserimento, all'aggiornamento e all'eliminazione da un controllo Web dati, vedere l'esercitazione [esaminare gli eventi associati all'inserimento, l'aggiornamento e l'eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) .

![Se si fa clic sul pulsante Elimina in GridView, viene chiamato il Metodo SqlDataSource s Delete ()](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figura 5**: facendo clic sul pulsante Elimina in GridView viene richiamato il Metodo SqlDataSource s `Delete()`

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Passaggio 2: generazione automatica delle istruzioni`INSERT`,`UPDATE`e`DELETE`

Come indicato nel passaggio 1, è possibile specificare le istruzioni SQL `INSERT`, `UPDATE`e `DELETE` tramite la sintassi dichiarativa Finestra Proprietà o Control. Tuttavia, questo approccio richiede la scrittura manuale manuale delle istruzioni SQL, che possono essere monotone e soggette a errori. Fortunatamente, la configurazione guidata origine dati offre un'opzione che consente di generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE` quando si utilizza la schermata specificare le colonne da una tabella di visualizzazione.

Consente di esplorare questa opzione di generazione automatica. Aggiungere un oggetto DetailsView alla finestra di progettazione in `InsertUpdateDelete.aspx` e impostare la relativa proprietà `ID` su `ManageProducts`. Quindi, dallo smart tag di DetailsView, scegliere di creare una nuova origine dati e creare un SqlDataSource denominato `ManageProductsDataSource`.

[![creare un nuovo SqlDataSource denominato ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figura 6**: creare un nuovo SqlDataSource denominato `ManageProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))

Nella configurazione guidata origine dati scegliere di utilizzare la stringa di connessione `NORTHWINDConnectionString` e fare clic su Avanti. Dalla schermata Configura istruzione SELECT, lasciare selezionato il pulsante di opzione specificare le colonne da una tabella o una vista e selezionare la tabella `Products` dall'elenco a discesa. Selezionare le colonne `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` nell'elenco di caselle di controllo.

[![utilizzando la tabella Products, restituire le colonne ProductID, ProductName, PrezzoUnitario e Discontinued](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figura 7**: utilizzo della tabella `Products`, restituire le colonne `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))

Per generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE` in base alla tabella e alle colonne selezionate, fare clic sul pulsante Avanzate e selezionare la casella di controllo genera `INSERT`, `UPDATE`e `DELETE` istruzioni.

![Selezionare la casella di controllo genera istruzioni INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figura 8**: selezionare la casella di controllo genera `INSERT`, `UPDATE`e istruzioni `DELETE`

La casella di controllo genera `INSERT`, `UPDATE`e `DELETE` sarà controllabile solo se la tabella selezionata include una chiave primaria e la colonna o le colonne di chiave primaria sono incluse nell'elenco delle colonne restituite. La casella di controllo Usa concorrenza ottimistica, che diventa selezionabile dopo che è stata selezionata la casella di controllo genera `INSERT`, `UPDATE`e `DELETE` istruzioni, aumenterà le clausole `WHERE` nelle istruzioni `UPDATE` e `DELETE` risultanti per fornire il controllo della concorrenza ottimistica. Per il momento, lasciare deselezionata questa casella di controllo; si esaminerà la concorrenza ottimistica con il controllo SqlDataSource nell'esercitazione successiva.

Dopo aver selezionato la casella di controllo genera `INSERT`, `UPDATE`e `DELETE`, fare clic su OK per tornare alla schermata Configura istruzione SELECT, quindi fare clic su Avanti, quindi su fine per completare la procedura guidata Configura origine dati. Al termine della procedura guidata, Visual Studio aggiungerà i BoundField a DetailsView per le colonne `ProductID`, `ProductName`e `UnitPrice` e un CheckBoxField per la colonna `Discontinued`. Dallo smart tag DetailsView s selezionare l'opzione Abilita paging in modo che l'utente che visita questa pagina possa esaminare i prodotti. Deselezionare anche le proprietà `Width` e `Height` di DetailsView.

Si noti che per lo smart tag sono disponibili le opzioni Consenti inserimento, Abilita modifica e Abilita eliminazione. Questo è dovuto al fatto che il SqlDataSource contiene valori per la `InsertCommand`, `UpdateCommand`e `DeleteCommand`, come illustrato nella sintassi dichiarativa seguente:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Si noti che per il controllo SqlDataSource sono stati impostati valori automaticamente per le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand`. Il set di colonne a cui si fa riferimento nelle proprietà `InsertCommand` e `UpdateCommand` si basa su quelle nell'istruzione `SELECT`. Ovvero, anziché avere *ogni* colonna Products nel `InsertCommand` e `UpdateCommand`, sono presenti solo le colonne specificate nella `SelectCommand` (less `ProductID`, che viene omesso perché è una [colonna`IDENTITY`](http://www.sqlteam.com/item.asp?ItemID=102), il cui valore non può essere modificato quando modificato e che viene assegnato automaticamente durante l'inserimento). Inoltre, per ogni parametro nelle proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` sono presenti parametri corrispondenti nelle raccolte `InsertParameters`, `UpdateParameters`e `DeleteParameters`.

Per attivare le funzionalità di modifica dei dati di DetailsView, selezionare le opzioni Consenti inserimento, Abilita modifica e Abilita eliminazione nello smart tag. Viene aggiunto un oggetto CommandField con le proprietà `ShowInsertButton`, `ShowEditButton`e `ShowDeleteButton` impostate su `true`.

Visitare la pagina in un browser e prendere nota dei pulsanti modifica, Elimina e nuovo inclusi in DetailsView. Facendo clic sul pulsante modifica, il controllo DetailsView viene trasformato in modalità di modifica, che consente di visualizzare ogni BoundField la cui proprietà `ReadOnly` è impostata su `false` (impostazione predefinita) come casella di testo e l'oggetto CheckBoxField come casella di controllo.

[![l'interfaccia di modifica predefinita di DetailsView s](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figura 9**: interfaccia di modifica predefinita di DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))

Analogamente, è possibile eliminare il prodotto attualmente selezionato o aggiungere un nuovo prodotto al sistema. Poiché l'istruzione `InsertCommand` funziona solo con le colonne `ProductName`, `UnitPrice`e `Discontinued`, le altre colonne hanno `NULL` o il relativo valore predefinito assegnato dal database al momento dell'inserimento. Proprio come con ObjectDataSource, se mancano le colonne `InsertCommand` della tabella di database che non consentono `NULL` s e Don t hanno un valore predefinito, si verificherà un errore SQL quando si tenta di eseguire l'istruzione `INSERT`.

> [!NOTE]
> Le interfacce di inserimento e modifica di DetailsView non dispongono di alcuna personalizzazione o convalida. Per aggiungere controlli di convalida o personalizzare le interfacce, è necessario convertire i BoundField in TemplateFields. Per ulteriori informazioni, vedere l'articolo relativo all' [aggiunta di controlli di convalida alle interfacce di modifica e inserimento](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [alla personalizzazione delle esercitazioni sull'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

Tenere inoltre presente che, per l'aggiornamento e l'eliminazione, DetailsView utilizza il valore `DataKey` del prodotto corrente, presente solo se la proprietà `DataKeyNames` è configurata. Se la modifica o l'eliminazione sembra non avere alcun effetto, verificare che sia impostata la proprietà `DataKeyNames`.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitazioni della generazione automatica di istruzioni SQL

Poiché l'opzione genera `INSERT`, `UPDATE`e `DELETE` Statements è disponibile solo quando si selezionano le colonne di una tabella, per le query più complesse sarà necessario scrivere le proprie istruzioni `INSERT`, `UPDATE`e `DELETE` come nel passaggio 1. Le istruzioni SQL `SELECT` usano in genere `JOIN` s per ripristinare i dati da una o più tabelle di ricerca a scopo di visualizzazione, ad esempio riportando il campo `CategoryName` della tabella `Categories` quando si visualizzano le informazioni sul prodotto. Allo stesso tempo, potrebbe essere necessario consentire all'utente di modificare, aggiornare o inserire dati nella tabella principale, in questo caso`Products`.

Sebbene le istruzioni `INSERT`, `UPDATE`e `DELETE` possano essere immesse manualmente, tenere presente il suggerimento relativo al risparmio di tempo seguente. Configurare inizialmente il SqlDataSource in modo da estrarre i dati solo dalla tabella `Products`. Utilizzare la configurazione guidata origine dati s specificare le colonne da una schermata della tabella o della vista in modo che sia possibile generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE`. Quindi, dopo aver completato la procedura guidata, scegliere di configurare SelectQuery dalla Finestra Proprietà (oppure, in alternativa, tornare alla configurazione guidata origine dati, ma usare l'opzione specificare un'istruzione SQL personalizzata o stored procedure). Aggiornare quindi l'istruzione `SELECT` per includere la sintassi del `JOIN`. Questa tecnica offre i vantaggi per il risparmio di tempo delle istruzioni SQL generate automaticamente e consente un'istruzione `SELECT` più personalizzata.

Un altro limite di generazione automatica delle istruzioni `INSERT`, `UPDATE`e `DELETE` è che le colonne nelle istruzioni `INSERT` e `UPDATE` sono basate sulle colonne restituite dall'istruzione `SELECT`. Potrebbe tuttavia essere necessario aggiornare o inserire più campi o meno. Ad esempio, nell'esempio del passaggio 2, potrebbe essere necessario che la `UnitPrice` BoundField sia di sola lettura. In tal caso, non dovrebbe essere visualizzato nella `UpdateCommand`. In alternativa, potrebbe essere necessario impostare il valore di un campo di tabella che non viene visualizzato in GridView. Ad esempio, quando si aggiunge un nuovo record, potrebbe essere necessario impostare il valore di `QuantityPerUnit` su TODO.

Se tali personalizzazioni sono necessarie, è necessario impostarle manualmente, tramite la Finestra Proprietà, l'opzione specificare un'istruzione SQL personalizzata o stored procedure nella procedura guidata oppure tramite la sintassi dichiarativa.

> [!NOTE]
> Quando si aggiungono parametri che non hanno campi corrispondenti nel controllo Web dei dati, tenere presente che i valori dei parametri devono essere assegnati in qualche modo. Questi valori possono essere: hardcoded direttamente nel `InsertCommand` o `UpdateCommand`; può provenire da un'origine predefinita (queryString, stato sessione, controlli Web nella pagina e così via); oppure possono essere assegnati a livello di codice, come illustrato nell'esercitazione precedente.

## <a name="summary"></a>Riepilogo

Per consentire ai controlli Web dei dati di utilizzare le funzionalità di inserimento, modifica ed eliminazione predefinite, il controllo origine dati a cui sono associate deve offrire tale funzionalità. Per il SqlDataSource, ciò significa che le istruzioni SQL `INSERT`, `UPDATE`e `DELETE` devono essere assegnate alle proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand`. Queste proprietà e le raccolte di parametri corrispondenti possono essere aggiunte manualmente o generate automaticamente tramite la configurazione guidata origine dati. In questa esercitazione sono state esaminate entrambe le tecniche.

È stata esaminata l'uso della concorrenza ottimistica con ObjectDataSource nell'esercitazione sull' [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) . Il controllo SqlDataSource fornisce inoltre supporto per la concorrenza ottimistica. Come indicato nel passaggio 2, quando si generano automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE`, la procedura guidata offre un'opzione Usa concorrenza ottimistica. Come si vedrà nell'esercitazione successiva, l'uso della concorrenza ottimistica con SqlDataSource modifica le clausole `WHERE` nelle istruzioni `UPDATE` e `DELETE` per assicurarsi che i valori per le altre colonne non siano stati modificati dopo l'ultima visualizzazione dei dati nella pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Successivo](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
