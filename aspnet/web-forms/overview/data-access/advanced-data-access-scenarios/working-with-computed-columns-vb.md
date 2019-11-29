---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Uso di colonne calcolate (VB) | Microsoft Docs
author: rick-anderson
description: Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che in genere fa riferimento a...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: e425d7363c2cdea6efb0ba51f3fc2b6a5330bf2a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74603120"
---
# <a name="working-with-computed-columns-vb"></a>Uso di colonne calcolate (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) o [Scarica PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Quando si crea una tabella di database, Microsoft SQL Server consente di definire una colonna calcolata il cui valore viene calcolato da un'espressione che in genere fa riferimento ad altri valori nello stesso record del database. Tali valori sono di sola lettura nel database, che richiede considerazioni speciali quando si utilizzano TableAdapter. In questa esercitazione si apprenderà come soddisfare le sfide poste dalle colonne calcolate.

## <a name="introduction"></a>Introduzione

Microsoft SQL Server consente le *[colonne calcolate](https://msdn.microsoft.com/library/ms191250.aspx)* , ovvero colonne i cui valori vengono calcolati da un'espressione che in genere fa riferimento ai valori di altre colonne nella stessa tabella. Un modello di dati di rilevamento ora può ad esempio avere una tabella denominata `ServiceLog` con colonne, tra cui `ServicePerformed`, `EmployeeID`, `Rate`e `Duration`. Sebbene la quantità dovuta all'elemento di servizio (la percentuale moltiplicata per la durata) venga calcolata tramite una pagina Web o un'altra interfaccia a livello di codice, potrebbe essere utile includere una colonna nella tabella `ServiceLog` denominata `AmountDue` che ha segnalato queste informazioni. Questa colonna può essere creata come colonna normale, ma deve essere aggiornata ogni volta che i valori della colonna `Rate` o `Duration` sono stati modificati. Un approccio migliore consiste nel rendere la colonna `AmountDue` una colonna calcolata usando l'espressione `Rate * Duration`. In questo modo SQL Server calcolare automaticamente il valore della colonna `AmountDue` ogni volta che viene fatto riferimento in una query.

Poiché un valore della colonna calcolata è determinato da un'espressione, tali colonne sono di sola lettura e pertanto non possono avere valori assegnati nelle istruzioni `INSERT` o `UPDATE`. Tuttavia, quando le colonne calcolate fanno parte della query principale per un TableAdapter che utilizza istruzioni SQL ad hoc, vengono incluse automaticamente nelle istruzioni `INSERT` e `UPDATE` generate automaticamente. Di conseguenza, è necessario aggiornare le proprietà `INSERT` e `UPDATE` di TableAdapter e `InsertCommand` e `UpdateCommand` per rimuovere i riferimenti a tutte le colonne calcolate.

Una sfida nell'uso di colonne calcolate con un TableAdapter che usa istruzioni SQL ad hoc è che le query di TableAdapter `INSERT` e `UPDATE` vengono rigenerate automaticamente ogni volta che viene completata la configurazione guidata TableAdapter. Pertanto, le colonne calcolate rimosse manualmente dal `INSERT` e `UPDATE` query verranno visualizzate di nuovo se la procedura guidata viene rieseguita. Sebbene gli oggetti TableAdapter che usano stored procedure non soffrano di questa fragilità, hanno le proprie peculiarità che verranno affrontate nel passaggio 3.

In questa esercitazione si aggiungerà una colonna calcolata alla tabella `Suppliers` nel database Northwind e quindi si creerà un TableAdapter corrispondente per lavorare con questa tabella e la relativa colonna calcolata. Il TableAdapter utilizzerà stored procedure anziché istruzioni SQL ad hoc, in modo che le personalizzazioni non vadano perse quando viene utilizzata la configurazione guidata TableAdapter.

Inizia subito.

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Passaggio 1: aggiunta di una colonna calcolata alla tabella`Suppliers`

Il database Northwind non include colonne calcolate, quindi è necessario aggiungerne una. Per questa esercitazione è possibile aggiungere una colonna calcolata alla tabella `Suppliers` denominata `FullContactName` che restituisce il nome del contatto, il titolo e la società per cui lavorano nel formato seguente: `ContactName` (`ContactTitle`, `CompanyName`). Questa colonna calcolata può essere utilizzata nei report quando si visualizzano informazioni sui fornitori.

Per iniziare, aprire la definizione della tabella `Suppliers` facendo clic con il pulsante destro del mouse sulla tabella `Suppliers` nel Esplora server e scegliendo Apri definizione tabella dal menu di scelta rapida. Verranno visualizzate le colonne della tabella e le relative proprietà, ad esempio il tipo di dati, se consentono `NULL` s e così via. Per aggiungere una colonna calcolata, iniziare digitando il nome della colonna nella definizione della tabella. Immettere quindi l'espressione nella casella di testo (formula) nella sezione specifica della colonna calcolata nella colonna Finestra Proprietà (vedere la figura 1). Denominare la colonna calcolata `FullContactName` e utilizzare l'espressione seguente:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Si noti che le stringhe possono essere concatenate in SQL utilizzando l'operatore `+`. L'istruzione `CASE` può essere usata come un condizionale in un linguaggio di programmazione tradizionale. Nell'espressione precedente l'istruzione `CASE` può essere letta come: se `ContactTitle` non è `NULL`, restituire il valore di `ContactTitle` concatenato a una virgola, in caso contrario, non emettere alcun elemento. Per ulteriori informazioni sull'utilità dell'istruzione `CASE`, vedere [le istruzioni relative alla potenza di SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Invece di usare un'istruzione `CASE`, avremmo potuto usare `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) restituisce *checkExpression* se è diverso da null. in caso contrario, restituisce *replacementValue*. Mentre `ISNULL` o `CASE` funzioneranno in questa istanza, esistono scenari più complessi in cui la flessibilità dell'istruzione `CASE` non può essere confrontata con `ISNULL`.

Dopo l'aggiunta di questa colonna calcolata, la schermata dovrebbe essere simile a quella illustrata nella figura 1.

[![aggiungere una colonna calcolata denominata FullContactName alla tabella Suppliers](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Figura 1**: aggiungere una colonna calcolata denominata `FullContactName` alla tabella `Suppliers` ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image3.png))

Dopo aver denominato la colonna calcolata e immettendo l'espressione, salvare le modifiche apportate alla tabella facendo clic sull'icona Salva sulla barra degli strumenti, premendo CTRL + S oppure scegliendo Salva `Suppliers`dal menu file.

Il salvataggio della tabella dovrebbe aggiornare il Esplora server, inclusa la colonna appena aggiunta nell'elenco di colonne della tabella `Suppliers`. Inoltre, l'espressione immessa nella casella di testo (formula) verrà automaticamente modificata in un'espressione equivalente che rimuove gli spazi vuoti superflui, racchiude i nomi di colonna tra parentesi quadre (`[]`) e include le parentesi per visualizzare in modo più esplicito l'ordine delle operazioni:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Per ulteriori informazioni sulle colonne calcolate in Microsoft SQL Server, consultare la [documentazione tecnica](https://msdn.microsoft.com/library/ms191250.aspx). Vedere anche procedura [: specificare le colonne calcolate](https://msdn.microsoft.com/library/ms188300.aspx) per una procedura dettagliata per la creazione di colonne calcolate.

> [!NOTE]
> Per impostazione predefinita, le colonne calcolate non vengono archiviate fisicamente nella tabella, ma vengono ricalcolate ogni volta che viene fatto riferimento in una query. Selezionando la casella di controllo è permanente, tuttavia, è possibile indicare SQL Server di archiviare fisicamente la colonna calcolata nella tabella. In questo modo è possibile creare un indice sulla colonna calcolata, in modo da migliorare le prestazioni delle query che utilizzano il valore della colonna calcolata nelle clausole `WHERE`. Per ulteriori informazioni, vedere [creazione di indici su colonne calcolate](https://msdn.microsoft.com/library/ms189292.aspx) .

## <a name="step-2-viewing-the-computed-column-s-values"></a>Passaggio 2: visualizzazione dei valori della colonna calcolata

Prima di iniziare a lavorare sul livello di accesso ai dati, è necessario un minuto per visualizzare i valori `FullContactName`. Dal Esplora server fare clic con il pulsante destro del mouse sul nome della tabella `Suppliers` e scegliere nuova query dal menu di scelta rapida. Verrà mostrata una finestra di query che richiede di scegliere le tabelle da includere nella query. Aggiungere la tabella `Suppliers` e fare clic su Chiudi. Controllare quindi le colonne `CompanyName`, `ContactName`, `ContactTitle`e `FullContactName` dalla tabella Suppliers. Infine, fare clic sull'icona del punto esclamativo rosso sulla barra degli strumenti per eseguire la query e visualizzare i risultati.

Come illustrato nella figura 2, i risultati includono `FullContactName`, che elenca le colonne `CompanyName`, `ContactName`e `ContactTitle` usando il formato `ContactName` (`ContactTitle`, `CompanyName`).

[![FullContactName usa il formato ContactName (ContactTitle, CompanyName)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Figura 2**: il `FullContactName` usa il formato `ContactName` (`ContactTitle`, `CompanyName`) ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Passaggio 3: aggiunta del`SuppliersTableAdapter`al livello di accesso ai dati

Per lavorare con le informazioni sul fornitore nell'applicazione, è necessario creare prima un TableAdapter e un DataTable nel DAL. Idealmente, questa operazione viene eseguita usando gli stessi passaggi semplici esaminati nelle esercitazioni precedenti. Tuttavia, l'uso di colonne calcolate introduce alcune grinze che meritano una discussione.

Se si usa un TableAdapter che usa istruzioni SQL ad hoc, è possibile includere semplicemente la colonna calcolata nella query principale di TableAdapter tramite la configurazione guidata TableAdapter. Questo, tuttavia, genererà automaticamente `INSERT` e `UPDATE` istruzioni che includono la colonna calcolata. Se si tenta di eseguire uno di questi metodi, una `SqlException` con il messaggio non è possibile modificare la colonna *ColumnName* perché è una colonna calcolata o il risultato di un operatore Union verrà generato. Sebbene sia possibile modificare manualmente l'istruzione `INSERT` e la `UPDATE` tramite le proprietà `InsertCommand` e `UpdateCommand` di TableAdapter, le personalizzazioni andranno perse ogni volta che viene eseguita di nuovo la configurazione guidata TableAdapter.

A causa della fragilità degli oggetti TableAdapter che utilizzano istruzioni SQL ad hoc, è consigliabile utilizzare stored procedure quando si utilizzano colonne calcolate. Se si utilizzano stored procedure esistenti, è sufficiente configurare il TableAdapter come descritto nell'esercitazione [utilizzo di stored procedure esistenti per i TableAdapter del set di dati tipizzati](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Se la procedura guidata TableAdapter crea le stored procedure per l'utente, tuttavia, è importante omettere inizialmente le colonne calcolate dalla query principale. Se si include una colonna calcolata nella query principale, la configurazione guidata TableAdapter indicherà, al completamento, che non è possibile creare le stored procedure corrispondenti. In breve, è necessario configurare inizialmente il TableAdapter usando una query principale senza colonna calcolata e quindi aggiornare manualmente il stored procedure corrispondente e il `SelectCommand` TableAdapter per includere la colonna calcolata. Questo approccio è simile a quello usato nell'esercitazione [aggiornamento del TableAdapter per l'uso di](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* .

Per questa esercitazione, è possibile aggiungere un nuovo TableAdapter e fare in modo che crei automaticamente le stored procedure. Di conseguenza, è necessario omettere inizialmente la colonna calcolata `FullContactName` dalla query principale.

Per iniziare, aprire il set di dati `NorthwindWithSprocs` nella cartella `~/App_Code/DAL`. Fare clic con il pulsante destro del mouse nella finestra di progettazione e scegliere Aggiungi nuovo TableAdapter dal menu di scelta rapida. Verrà avviata la configurazione guidata TableAdapter. Consente di specificare il database da cui eseguire query sui dati (`NORTHWNDConnectionString` da `Web.config`) e fare clic su Avanti. Poiché non sono state ancora create stored procedure per l'esecuzione di query o la modifica della tabella `Suppliers`, selezionare l'opzione crea nuove stored procedure in modo che vengano create automaticamente dalla procedura guidata e fare clic su Avanti.

[![scegliere l'opzione crea nuove stored procedure](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Figura 3**: scegliere l'opzione crea nuove stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image9.png))

Il passaggio successivo richiede la query principale. Immettere la query seguente, che restituisce le colonne `SupplierID`, `CompanyName`, `ContactName`e `ContactTitle` per ogni fornitore. Si noti che questa query omette intenzionalmente la colonna calcolata (`FullContactName`); il stored procedure corrispondente verrà aggiornato in modo da includere questa colonna nel passaggio 4.

[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Dopo aver immesso la query principale e aver fatto clic su Avanti, la procedura guidata consente di denominare le quattro stored procedure che verranno generate. Denominare le stored procedure `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`e `Suppliers_Delete`, come illustrato nella figura 4.

[![personalizzare i nomi delle stored procedure generate automaticamente](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Figura 4**: personalizzare i nomi delle stored procedure generate automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image12.png))

Il passaggio successivo della procedura guidata consente di assegnare un nome ai metodi TableAdapter e di specificare i modelli usati per accedere ai dati e aggiornarli. Lasciare selezionate tutte e tre le caselle di controllo, ma rinominare il metodo `GetData` per `GetSuppliers`. Fare clic su Fine per completare la procedura guidata.

[![rinominare il metodo GetData in getsuppliers](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Figura 5**: rinominare il metodo `GetData` in `GetSuppliers` ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image15.png))

Quando si fa clic su fine, la procedura guidata creerà le quattro stored procedure e aggiungerà il TableAdapter e la DataTable corrispondente al set di dati tipizzato.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Passaggio 4: includere la colonna calcolata nella query principale di TableAdapter

È ora necessario aggiornare TableAdapter e DataTable creati nel passaggio 3 per includere la colonna calcolata `FullContactName`. Questa operazione include due passaggi:

1. Aggiornamento della stored procedure `Suppliers_Select` per restituire la colonna calcolata `FullContactName` e
2. Aggiornamento del DataTable per includere una colonna di `FullContactName` corrispondente.

Per iniziare, passare al Esplora server ed eseguire il drill-down nella cartella stored procedure. Aprire il stored procedure `Suppliers_Select` e aggiornare la query `SELECT` in modo da includere la colonna calcolata `FullContactName`:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Salvare le modifiche apportate al stored procedure facendo clic sull'icona Salva sulla barra degli strumenti, premendo CTRL + S oppure scegliendo l'opzione Salva `Suppliers_Select` dal menu file.

Tornare quindi a Progettazione DataSet, fare clic con il pulsante destro del mouse sul `SuppliersTableAdapter`e scegliere Configura dal menu di scelta rapida. Si noti che la colonna `Suppliers_Select` include ora la colonna `FullContactName` nella relativa raccolta di colonne di dati.

[![eseguire la configurazione guidata di TableAdapter per aggiornare le colonne di DataTable](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Figura 6**: eseguire la configurazione guidata di TableAdapter per aggiornare le colonne di DataTable ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image18.png))

Fare clic su Fine per completare la procedura guidata. Verrà aggiunta automaticamente una colonna corrispondente al `SuppliersDataTable`. La procedura guidata TableAdapter è sufficientemente intelligente da rilevare che la colonna `FullContactName` è una colonna calcolata e quindi di sola lettura. Di conseguenza, imposta la proprietà `ReadOnly` della colonna su `true`. Per verificarlo, selezionare la colonna dalla `SuppliersDataTable`, quindi passare al Finestra Proprietà (vedere la figura 7). Si noti che anche le proprietà `DataType` e `MaxLength` della colonna `FullContactName` vengono impostate di conseguenza.

[![la colonna FullContactName è contrassegnata come di sola lettura](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Figura 7**: la colonna `FullContactName` è contrassegnata come di sola lettura ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Passaggio 5: aggiunta di un metodo di`GetSupplierBySupplierID`al TableAdapter

Per questa esercitazione verrà creata una pagina ASP.NET che consente di visualizzare i fornitori in una griglia aggiornabile. Nelle esercitazioni precedenti è stato aggiornato un singolo record dal livello della logica di business recuperando il record particolare dal DAL come DataTable fortemente tipizzato, aggiornando le proprietà e quindi inviando di nuovo la DataTable aggiornata al DAL per propagare le modifiche a database. Per eseguire questo primo passaggio: recupero del record aggiornato dal dal-è necessario innanzitutto aggiungere un metodo di `GetSupplierBySupplierID(supplierID)` al DAL.

Fare clic con il pulsante destro del mouse sul `SuppliersTableAdapter` nella progettazione del set di dati e scegliere l'opzione Aggiungi query dal menu di scelta rapida. Come illustrato nel passaggio 3, consentire alla procedura guidata di generare un nuovo stored procedure per Microsoft selezionando l'opzione Crea nuovo stored procedure (per una schermata di questo passaggio della procedura guidata, fare riferimento alla figura 3). Poiché questo metodo restituirà un record con più colonne, indicare che si desidera utilizzare una query SQL che è un oggetto SELECT che restituisce righe e fare clic su Avanti.

[![scegliere l'opzione per la restituzione delle righe](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Figura 8**: scegliere l'opzione per la restituzione delle righe ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image24.png))

Il passaggio successivo richiede la query da usare per questo metodo. Immettere quanto segue, che restituisce gli stessi campi dati della query principale, ma per un fornitore specifico.

[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Nella schermata successiva viene chiesto di assegnare un nome alla stored procedure che verrà generata automaticamente. Denominare questo stored procedure `Suppliers_SelectBySupplierID` e fare clic su Avanti.

[![nome della stored procedure Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Figura 9**: assegnare un nome alla stored procedure `Suppliers_SelectBySupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image27.png))

Infine, la procedura guidata richiede i modelli di accesso ai dati e i nomi dei metodi da usare per l'oggetto TableAdapter. Lasciare selezionate entrambe le caselle di controllo, ma rinominare i metodi `FillBy` e `GetDataBy` rispettivamente per `FillBySupplierID` e `GetSupplierBySupplierID`.

[![il nome dei metodi TableAdapter FillBySupplierID e GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Figura 10**: assegnare un nome ai metodi TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image30.png))

Fare clic su Fine per completare la procedura guidata.

## <a name="step-6-creating-the-business-logic-layer"></a>Passaggio 6: creazione del livello di logica di business

Prima di creare una pagina ASP.NET che usa la colonna calcolata creata nel passaggio 1, è prima di tutto necessario aggiungere i metodi corrispondenti in BLL. La pagina ASP.NET, che viene creata nel passaggio 7, consentirà agli utenti di visualizzare e modificare i fornitori. Per questo motivo, è necessario che il livello BLL fornisca, come minimo, un metodo per ottenere tutti i fornitori e un altro per aggiornare un particolare fornitore.

Creare un nuovo file di classe denominato `SuppliersBLLWithSprocs` nella cartella `~/App_Code/BLL` e aggiungere il codice seguente:

[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Analogamente alle altre classi BLL, `SuppliersBLLWithSprocs` dispone di una proprietà `Protected` `Adapter` che restituisce un'istanza della classe `SuppliersTableAdapter` insieme a due metodi di `Public`: `GetSuppliers` e `UpdateSupplier`. Il metodo `GetSuppliers` chiama e restituisce i `SuppliersDataTable` restituiti dal metodo `GetSupplier` corrispondente nel livello di accesso ai dati. Il metodo `UpdateSupplier` recupera le informazioni sul fornitore in corso di aggiornamento tramite una chiamata al metodo DAL `GetSupplierBySupplierID(supplierID)`. Aggiorna quindi le proprietà `CategoryName`, `ContactName`e `ContactTitle` ed effettua il commit di queste modifiche nel database chiamando il metodo `Update` del livello di accesso ai dati, passando l'oggetto `SuppliersRow` modificato.

> [!NOTE]
> Ad eccezione di `SupplierID` e `CompanyName`, tutte le colonne nella tabella Suppliers consentono `NULL` valori. Di conseguenza, se i parametri `contactName` o `contactTitle` passati sono `Nothing` è necessario impostare le proprietà `ContactName` e `ContactTitle` corrispondenti su un valore di `NULL` database usando rispettivamente i metodi `SetContactNameNull` e `SetContactTitleNull`.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Passaggio 7: utilizzo della colonna calcolata dal livello di presentazione

Con la colonna calcolata aggiunta alla tabella `Suppliers` e il DAL e BLL aggiornati di conseguenza, è possibile creare una pagina ASP.NET che funzioni con la colonna calcolata `FullContactName`. Per iniziare, aprire la pagina `ComputedColumns.aspx` nella cartella `AdvancedDAL` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare la proprietà GridView s `ID` su `Suppliers` e, dal relativo smart tag, associarla a un nuovo ObjectDataSource denominato `SuppliersDataSource`. Configurare ObjectDataSource per usare la classe `SuppliersBLLWithSprocs` aggiunta nel passaggio 6, quindi fare clic su Avanti.

[![configurare ObjectDataSource per l'utilizzo della classe SuppliersBLLWithSprocs](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per l'uso della classe `SuppliersBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image33.png))

Esistono solo due metodi definiti nella classe `SuppliersBLLWithSprocs`: `GetSuppliers` e `UpdateSupplier`. Verificare che questi due metodi siano specificati rispettivamente nelle schede seleziona e aggiorna, quindi fare clic su fine per completare la configurazione di ObjectDataSource.

Al termine della configurazione guidata origine dati, in Visual Studio verrà aggiunto un BoundField per ognuno dei campi dati restituiti. Rimuovere il `SupplierID` BoundField e modificare le proprietà del `HeaderText` dei BoundField `CompanyName`, `ContactName`, `ContactTitle`e `FullContactName` in società, nome contatto, titolo e nome completo del contatto, rispettivamente. Dallo smart tag selezionare la casella di controllo Abilita modifica per attivare le funzionalità di modifica predefinite di GridView.

Oltre ad aggiungere i BoundField al GridView, il completamento della creazione guidata origine dati fa sì che Visual Studio imposti anche la proprietà `OldValuesParameterFormatString` di ObjectDataSource su\_originale {0}. Ripristinare il valore predefinito di questa impostazione {0}.

Dopo aver apportato queste modifiche a GridView e ObjectDataSource, il relativo markup dichiarativo avrà un aspetto simile al seguente:

[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Visitare quindi questa pagina tramite un browser. Come illustrato nella figura 12, ogni fornitore è elencato in una griglia che include la colonna `FullContactName`, il cui valore è semplicemente la concatenazione delle altre tre colonne formattate come `ContactName` (`ContactTitle`, `CompanyName`).

[![ogni fornitore è elencato nella griglia](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Figura 12**: ogni fornitore è elencato nella griglia ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image36.png))

Se si fa clic sul pulsante modifica per un determinato fornitore, viene eseguito un postback e tale riga viene sottoposta a rendering nell'interfaccia di modifica (vedere la figura 13). Il rendering delle prime tre colonne viene eseguito nell'interfaccia di modifica predefinita, ovvero un controllo TextBox la cui proprietà `Text` è impostata sul valore del campo dati. La colonna `FullContactName`, tuttavia, rimane come testo. Quando i BoundField sono stati aggiunti al controllo GridView al termine della configurazione guidata origine dati, la proprietà `ReadOnly` di `FullContactName` BoundField è stata impostata su `True` perché la relativa colonna di `FullContactName` nel `SuppliersDataTable` ha la relativa proprietà `ReadOnly` impostata su `True`. Come indicato nel passaggio 4, la proprietà `ReadOnly` `FullContactName` s è stata impostata su `True` perché il TableAdapter ha rilevato che la colonna è una colonna calcolata.

[![la colonna FullContactName non è modificabile](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Figura 13**: la colonna `FullContactName` non è modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](working-with-computed-columns-vb/_static/image39.png))

Procedere con l'aggiornamento del valore di una o più colonne modificabili e fare clic su Aggiorna. Si noti che il valore `FullContactName` s viene aggiornato automaticamente per riflettere la modifica.

> [!NOTE]
> GridView usa attualmente i BoundField per i campi modificabili, ottenendo l'interfaccia di modifica predefinita. Poiché il campo `CompanyName` è obbligatorio, deve essere convertito in un TemplateField che include un RequiredFieldValidator. Lo lascio come esercizio per il lettore interessato. Per istruzioni dettagliate sulla conversione di un BoundField in un TemplateField e sull'aggiunta di controlli di convalida, vedere l'esercitazione [aggiunta di controlli di convalida all'interfaccia di modifica e inserimento delle interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) .

## <a name="summary"></a>Riepilogo

Quando si definisce lo schema per una tabella, Microsoft SQL Server consente l'inclusione di colonne calcolate. Si tratta di colonne i cui valori vengono calcolati da un'espressione che in genere fa riferimento ai valori di altre colonne nello stesso record. Poiché i valori per le colonne calcolate sono basati su un'espressione, sono di sola lettura e non possono essere assegnati a un valore in un'istruzione `INSERT` o `UPDATE`. In questo modo si verificano problemi durante l'utilizzo di una colonna calcolata nella query principale di un TableAdapter che tenta di generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE` corrispondenti.

In questa esercitazione sono state illustrate le tecniche per aggirare le sfide poste dalle colonne calcolate. In particolare, sono state usate stored procedure nel TableAdapter per superare la fragilità insita negli oggetti TableAdapter che usano istruzioni SQL ad hoc. Quando la procedura guidata TableAdapter crea nuove stored procedure, è importante che la query principale omette inizialmente le colonne calcolate perché la loro presenza impedisce la generazione delle stored procedure di modifica dei dati. Dopo la configurazione iniziale del TableAdapter, è possibile rieseguire il `SelectCommand` stored procedure per includere tutte le colonne calcolate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Hilton Geisenow e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-additional-datatable-columns-vb.md)
> [Successivo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
