---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Aggiornamento del TableAdapter per l'utilizzo di JOINC#() | Microsoft Docs
author: rick-anderson
description: Quando si utilizza un database, è comune richiedere dati distribuiti tra più tabelle. Per recuperare i dati da due tabelle diverse, è possibile usare...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: 24ff3645783dabfcdef5ac313a2d4833e4998efc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78532557"
---
# <a name="updating-the-tableadapter-to-use-joins-c"></a>Aggiornamento del TableAdapter per l'uso dei join (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) o [Scarica PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Quando si utilizza un database, è comune richiedere dati distribuiti tra più tabelle. Per recuperare dati da due tabelle diverse, è possibile utilizzare una sottoquery correlata o un'operazione di JOIN. In questa esercitazione vengono confrontate le sottoquery correlate e la sintassi di JOIN prima di esaminare come creare un TableAdapter che includa un JOIN nella relativa query principale.

## <a name="introduction"></a>Introduzione

Con i database relazionali, i dati di cui si è interessati a lavorare sono spesso distribuiti in più tabelle. Ad esempio, quando si visualizzano le informazioni sul prodotto, è probabile che si desideri elencare i nomi dei fornitori e delle categorie corrispondenti di ogni prodotto. La tabella `Products` dispone di valori `CategoryID` e `SupplierID`, ma i nomi di categoria e fornitore effettivi sono rispettivamente nelle tabelle `Categories` e `Suppliers`.

Per recuperare informazioni da un'altra tabella correlata, è possibile usare *sottoquery correlate* *o `JOIN`.* Una sottoquery correlata è una query `SELECT` nidificata che fa riferimento a colonne della query esterna. Nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) , ad esempio, sono state usate due sottoquery correlate nella query principale di `ProductsTableAdapter` s per restituire i nomi di categoria e Supplier per ogni prodotto. Un `JOIN` è un costrutto SQL che unisce le righe correlate di due tabelle diverse. Per visualizzare le informazioni sulle categorie insieme a ogni prodotto è stata usata una `JOIN` nell'esercitazione sui [dati di query con il controllo SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) .

Il motivo per cui si è astenuta l'uso di `JOIN` s con gli oggetti TableAdapter è dovuto a limitazioni nella procedura guidata di TableAdapter per generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE` corrispondenti. In particolare, se la query principale di TableAdapter contiene `JOIN` s, il TableAdapter non è in grado di creare automaticamente le istruzioni SQL ad hoc o le stored procedure per le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand`.

In questa esercitazione si confronteranno brevemente e si contrasteranno le sottoquery correlate e `JOIN` s prima di esplorare come creare un TableAdapter che includa `JOIN` nella query principale.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Confronto e contrasto tra sottoquery correlate e`JOIN` s

Tenere presente che il `ProductsTableAdapter` creato nella prima esercitazione del set di dati `Northwind` USA sottoquery correlate per ripristinare la categoria e il nome del fornitore corrispondenti di ogni prodotto. La query principale `ProductsTableAdapter` s è illustrata di seguito.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Le due sottoquery correlate: `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` sono `SELECT` query che restituiscono un singolo valore per prodotto come colonna aggiuntiva nell'elenco di colonne dell'istruzione `SELECT` esterna.

In alternativa, è possibile utilizzare un `JOIN` per restituire il fornitore e il nome della categoria di ogni prodotto. La query seguente restituisce lo stesso output di quello precedente, ma usa `JOIN` s al posto delle sottoquery:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Un `JOIN` unisce i record di una tabella con i record di un'altra tabella in base ad alcuni criteri. Nella query precedente, ad esempio, il `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica SQL Server di unire ogni record di prodotto con il record di categoria il cui valore `CategoryID` corrisponde al valore `CategoryID` del prodotto. Il risultato Unito consente di usare i campi di categoria corrispondenti per ogni prodotto, ad esempio `CategoryName`.

> [!NOTE]
> per l'esecuzione di query sui dati dei database relazionali vengono comunemente usati `JOIN` s. Se non si ha familiarità con la sintassi di `JOIN` o se è necessario riattivarne l'utilizzo, è consigliabile eseguire l' [esercitazione su SQL Join](http://www.w3schools.com/sql/sql_join.asp) in [W3 Schools](http://www.w3schools.com/). Vale la pena leggere anche le sezioni nozioni fondamentali [`JOIN`](https://msdn.microsoft.com/library/ms191517.aspx) e [sottoquery](https://msdn.microsoft.com/library/ms189575.aspx) della [documentazione online di SQL](https://msdn.microsoft.com/library/ms130214.aspx).

Poiché `JOIN` s e le sottoquery correlate possono essere utilizzate entrambi per recuperare i dati correlati da altre tabelle, molti sviluppatori lasciano la testa e chiedono quale approccio utilizzare. Tutti i guru SQL di cui ho parlato hanno detto approssimativamente la stessa cosa, che non è importante per le prestazioni, poiché SQL Server produrrà piani di esecuzione approssimativamente identici. Il loro Consiglio, quindi, consiste nell'usare la tecnica che l'utente e il team sono più comodi con. È importante notare che, dopo aver preso in considerazione questo Consiglio, questi esperti esprimono immediatamente le proprie preferenze di `JOIN` s rispetto alle sottoquery correlate.

Quando si compila un livello di accesso ai dati utilizzando DataSet tipizzati, gli strumenti funzionano meglio quando si utilizzano sottoquery. In particolare, la procedura guidata di TableAdapter non genera automaticamente le istruzioni corrispondenti `INSERT`, `UPDATE`e `DELETE` se la query principale contiene `JOIN` s, ma genera automaticamente tali istruzioni quando si utilizzano sottoquery correlate.

Per esplorare questa lacuna, creare un set di dati tipizzato temporaneo nella cartella `~/App_Code/DAL`. Durante la configurazione guidata TableAdapter, scegliere di usare istruzioni SQL ad hoc e immettere la query `SELECT` seguente (vedere la figura 1):

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]

[![immettere una query principale contenente JOIN](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Figura 1**: immettere una query principale che contiene `JOIN` s ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))

Per impostazione predefinita, il TableAdapter creerà automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE` basate sulla query principale. Se si fa clic sul pulsante Avanzate, è possibile vedere che questa funzionalità è abilitata. Nonostante questa impostazione, il TableAdapter non sarà in grado di creare le istruzioni `INSERT`, `UPDATE`e `DELETE` perché la query principale contiene una `JOIN`.

![Immettere una query principale contenente JOIN](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Figura 2**: immettere una query principale che contiene `JOIN` s

Fare clic su Fine per completare la procedura guidata. A questo punto, nella finestra di progettazione del set di dati verrà incluso un solo TableAdapter con una DataTable con colonne per ognuno dei campi restituiti nell'elenco di colonne della query `SELECT`. Sono inclusi i `CategoryName` e `SupplierName`, come illustrato nella figura 3.

![Il DataTable include una colonna per ogni campo restituito nell'elenco di colonne](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Figura 3**: la DataTable include una colonna per ogni campo restituito nell'elenco di colonne

Mentre la DataTable contiene le colonne appropriate, il TableAdapter non dispone di valori per le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand`. Per confermare questa operazione, fare clic sul TableAdapter nella finestra di progettazione e quindi passare al Finestra Proprietà. Si noterà che le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` sono impostate su (nessuno).

[![le proprietà InsertCommand, UpdateCommand e DeleteCommand sono impostate su (nessuno)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Figura 4**: le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` sono impostate su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))

Per ovviare a questo problema, è possibile fornire manualmente le istruzioni e i parametri SQL per le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` tramite il Finestra Proprietà. In alternativa, è possibile iniziare configurando la query principale di TableAdapter in modo da *non* includere alcun `JOIN`. Questo consentirà di generare automaticamente le istruzioni `INSERT`, `UPDATE`e `DELETE`. Dopo aver completato la procedura guidata, è possibile aggiornare manualmente il `SelectCommand` TableAdapter dall'Finestra Proprietà in modo che includa la sintassi `JOIN`.

Sebbene questo approccio funzioni, è molto fragile quando si usano query SQL ad hoc poiché ogni volta che la query principale di TableAdapter viene riconfigurata tramite la procedura guidata, le istruzioni `INSERT`, `UPDATE`e `DELETE` generate automaticamente vengono ricreate. Ciò significa che tutte le personalizzazioni apportate in un secondo momento andranno perse se si fa clic con il pulsante destro del mouse sul TableAdapter, si sceglie Configura dal menu di scelta rapida e si completa nuovamente la procedura guidata.

La fragilità delle istruzioni `INSERT`, `UPDATE`e `DELETE` di TableAdapter generate automaticamente è fortunatamente limitata alle istruzioni SQL ad hoc. Se il TableAdapter utilizza stored procedure, è possibile personalizzare le stored procedure `SelectCommand`, `InsertCommand`, `UpdateCommand`o `DeleteCommand` ed eseguire nuovamente la configurazione guidata TableAdapter senza dover temere che le stored procedure vengano modificate.

Nei passaggi successivi verrà creato un TableAdapter che inizialmente usa una query principale che omette qualsiasi `JOIN` s in modo che le stored procedure di inserimento, aggiornamento ed eliminazione corrispondenti vengano generate automaticamente. Si aggiornerà quindi il `SelectCommand` in modo che usi una `JOIN` che restituisce colonne aggiuntive dalle tabelle correlate. Infine, si creerà una classe del livello della logica di business corrispondente e si dimostrerà l'uso del TableAdapter in una pagina Web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Passaggio 1: creazione del TableAdapter usando una query principale semplificata

Per questa esercitazione si aggiungeranno un TableAdapter e DataTable fortemente tipizzati per la tabella `Employees` nel set di dati `NorthwindWithSprocs`. La tabella `Employees` contiene un campo di `ReportsTo` che specifica il `EmployeeID` del responsabile del dipendente. Il dipendente Anne Dodsworth, ad esempio, ha un valore `ReportTo` pari a 5, che corrisponde al `EmployeeID` di Steven Buchanan. Di conseguenza, Anne segnala a Steven, il suo responsabile. Insieme alla segnalazione di ogni valore di `ReportsTo` dei dipendenti, è possibile che si desideri recuperare anche il nome del proprio responsabile. Questa operazione può essere eseguita utilizzando un `JOIN`. Tuttavia, se si usa un `JOIN` durante la creazione iniziale del TableAdapter, impedisce alla procedura guidata di generare automaticamente le corrispondenti funzionalità di inserimento, aggiornamento ed eliminazione. Pertanto, si inizierà creando un TableAdapter la cui query principale non contiene `JOIN` s. Quindi, nel passaggio 2, verrà aggiornata la query principale stored procedure per recuperare il nome del Manager tramite una `JOIN`.

Per iniziare, aprire il set di dati `NorthwindWithSprocs` nella cartella `~/App_Code/DAL`. Fare clic con il pulsante destro del mouse sulla finestra di progettazione, selezionare l'opzione Aggiungi dal menu di scelta rapida e scegliere la voce di menu TableAdapter. Verrà avviata la configurazione guidata TableAdapter. Come illustrato nella figura 5, fare in modo che la procedura guidata crei nuove stored procedure e fare clic su Avanti. Per un aggiornamento della creazione di nuove stored procedure dalla procedura guidata di TableAdapter, vedere l'esercitazione [creazione di nuove stored procedure per il DataSet tipizzato s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) .

[![selezionare l'opzione crea nuove stored procedure](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Figura 5**: selezionare l'opzione crea nuove stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))

Usare l'istruzione `SELECT` seguente per la query principale di TableAdapter:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Poiché questa query non include `JOIN` s, la procedura guidata TableAdapter creerà automaticamente le stored procedure con le istruzioni `INSERT`, `UPDATE`e `DELETE` corrispondenti, nonché un stored procedure per l'esecuzione della query principale.

Il passaggio seguente consente di denominare le stored procedure di TableAdapter. Usare i nomi `Employees_Select`, `Employees_Insert`, `Employees_Update`e `Employees_Delete`, come illustrato nella figura 6.

[Nome ![le stored procedure di TableAdapter](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Figura 6**: assegnare un nome alle stored procedure di TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))

Nel passaggio finale viene richiesto di assegnare un nome ai metodi TableAdapter. Utilizzare `Fill` e `GetEmployees` come nomi di metodo. Assicurarsi anche di lasciare selezionata la casella di controllo Crea metodi per inviare aggiornamenti direttamente al database (GenerateDBDirectMethods).

[Nome ![i metodi di TableAdapter Fill e GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Figura 7**: assegnare un nome ai metodi TableAdapter `Fill` e `GetEmployees` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))

Dopo aver completato la procedura guidata, esaminare le stored procedure nel database. Verranno visualizzati quattro nuovi: `Employees_Select`, `Employees_Insert`, `Employees_Update`e `Employees_Delete`. Esaminare quindi i `EmployeesDataTable` e `EmployeesTableAdapter` appena creati. Il DataTable contiene una colonna per ogni campo restituito dalla query principale. Fare clic sul TableAdapter, quindi passare al Finestra Proprietà. Si noterà che le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` sono configurate correttamente per chiamare le stored procedure corrispondenti.

[![TableAdapter include funzionalità di inserimento, aggiornamento ed eliminazione](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Figura 8**: il TableAdapter include funzionalità di inserimento, aggiornamento ed eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))

Con le stored procedure INSERT, Update e Delete create automaticamente e le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` configurate correttamente, è possibile personalizzare il stored procedure `SelectCommand` s per restituire informazioni aggiuntive su ogni responsabile del dipendente. In particolare, è necessario aggiornare il `Employees_Select` stored procedure per usare un `JOIN` e restituire i valori `FirstName` e `LastName` del Manager. Al termine dell'aggiornamento del stored procedure, sarà necessario aggiornare la DataTable in modo che includa le colonne aggiuntive. Queste due attività verranno affrontate nei passaggi 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Passaggio 2: personalizzazione della stored procedure per includere un`JOIN`

Per iniziare, passare alla Esplora server, eseguire il drill-down nella cartella stored procedure del database Northwind e aprire la stored procedure `Employees_Select`. Se questo stored procedure non viene visualizzato, fare clic con il pulsante destro del mouse sulla cartella stored procedure e scegliere Aggiorna. Aggiornare il stored procedure in modo che usi un `LEFT JOIN` per restituire il nome e il cognome del Manager:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Dopo aver aggiornato l'istruzione `SELECT`, salvare le modifiche scegliendo Salva `Employees_Select`dal menu file. In alternativa, è possibile fare clic sull'icona Salva sulla barra degli strumenti o premere CTRL + S. Dopo aver salvato le modifiche, fare clic con il pulsante destro del mouse sul `Employees_Select` stored procedure nel Esplora server e scegliere Esegui. In questo modo verrà eseguito il stored procedure e i risultati vengono visualizzati nella finestra di output (vedere la figura 9).

[![i risultati delle stored procedure vengono visualizzati nell'Finestra di output](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Figura 9**: i risultati delle stored procedure vengono visualizzati nel finestra di output ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Passaggio 3: aggiornamento delle colonne di DataTable

A questo punto, il `Employees_Select` stored procedure restituisce i valori `ManagerFirstName` e `ManagerLastName`, ma mancano le colonne nel `EmployeesDataTable`. Queste colonne mancanti possono essere aggiunte alla DataTable in uno dei due modi seguenti:

- **Manualmente** , fare clic con il pulsante destro del mouse sulla DataTable in Progettazione DataSet e scegliere colonna dal menu Aggiungi. È quindi possibile assegnare un nome alla colonna e impostarne le proprietà di conseguenza.
- **Automaticamente** : la configurazione guidata TableAdapter aggiornerà le colonne di DataTable in modo da riflettere i campi restituiti dall'stored procedure di `SelectCommand`. Quando si utilizzano istruzioni SQL ad hoc, verranno rimosse anche le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` poiché il `SelectCommand` contiene ora un `JOIN`. Tuttavia, quando si utilizzano stored procedure, queste proprietà del comando rimangono intatte.

È stata esaminata l'aggiunta manuale di colonne DataTable nelle esercitazioni precedenti, tra cui [Master/Detail usando un elenco puntato di record master con un DataList di dettagli e il](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) [caricamento di file](../working-with-binary-files/uploading-files-cs.md). il processo verrà esaminato più dettagliatamente nell'esercitazione successiva. Per questa esercitazione, tuttavia, è consigliabile usare l'approccio automatico tramite la configurazione guidata TableAdapter.

Per iniziare, fare clic con il pulsante destro del mouse sul `EmployeesTableAdapter` e scegliere Configura dal menu di scelta rapida. Viene visualizzata la configurazione guidata TableAdapter, in cui sono elencate le stored procedure utilizzate per la selezione, l'inserimento, l'aggiornamento e l'eliminazione, insieme ai relativi valori restituiti e parametri (se presenti). Nella figura 10 è illustrata questa procedura guidata. Qui è possibile osservare che il `Employees_Select` stored procedure ora restituisce i campi `ManagerFirstName` e `ManagerLastName`.

[![procedura guidata mostra l'elenco di colonne aggiornato per la stored procedure Employees_Select](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Figura 10**: la procedura guidata mostra l'elenco di colonne aggiornato per la stored procedure `Employees_Select` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))

Completare la procedura guidata facendo clic su fine. Quando si torna a Progettazione DataSet, il `EmployeesDataTable` include due colonne aggiuntive: `ManagerFirstName` e `ManagerLastName`.

[![EmployeesDataTable contiene due nuove colonne](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Figura 11**: il `EmployeesDataTable` contiene due nuove colonne ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))

Per illustrare che il stored procedure di `Employees_Select` aggiornato è attivo e che le funzionalità di inserimento, aggiornamento ed eliminazione del TableAdapter sono ancora funzionali, è possibile creare una pagina Web che consenta agli utenti di visualizzare ed eliminare i dipendenti. Prima di creare una pagina di questo tipo, tuttavia, è necessario innanzitutto creare una nuova classe nel livello della logica di business per l'utilizzo dei dipendenti dal set di dati `NorthwindWithSprocs`. Nel passaggio 4 si creerà una classe `EmployeesBLLWithSprocs`. Nel passaggio 5 verrà usata questa classe da una pagina ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Passaggio 4: implementazione del livello di logica di business

Creare un nuovo file di classe nella cartella `~/App_Code/BLL` denominata `EmployeesBLLWithSprocs.cs`. Questa classe simula la semantica della classe `EmployeesBLL` esistente. solo questo nuovo fornisce un minor numero di metodi e usa il set di dati di `NorthwindWithSprocs` (anziché il set di dati `Northwind`). Aggiungere il codice seguente alla classe `EmployeesBLLWithSprocs` .

[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

La proprietà `Adapter` della classe `EmployeesBLLWithSprocs` restituisce un'istanza del `EmployeesTableAdapter`del set di dati `NorthwindWithSprocs`. Viene utilizzato dai metodi `GetEmployees` e `DeleteEmployee` della classe. Il metodo `GetEmployees` chiama il metodo di `GetEmployees` corrispondente `EmployeesTableAdapter`, che richiama il `Employees_Select` stored procedure e popola i risultati in un `EmployeeDataTable`. Il metodo `DeleteEmployee` chiama in modo analogo il metodo `EmployeesTableAdapter` s `Delete`, che richiama il stored procedure `Employees_Delete`.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Passaggio 5: uso dei dati nel livello di presentazione

Con la classe `EmployeesBLLWithSprocs` completata, si è pronti per lavorare con i dati dei dipendenti tramite una pagina ASP.NET. Aprire la pagina `JOINs.aspx` nella cartella `AdvancedDAL` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa proprietà `ID` su `Employees`. Successivamente, dallo smart tag GridView s, associare la griglia a un nuovo controllo ObjectDataSource denominato `EmployeesDataSource`.

Configurare ObjectDataSource per l'utilizzo della classe `EmployeesBLLWithSprocs` e, dalle schede SELECT ed DELETE, assicurarsi che i metodi `GetEmployees` e `DeleteEmployee` siano selezionati dagli elenchi a discesa. Fare clic su fine per completare la configurazione di ObjectDataSource.

[![configurare ObjectDataSource per l'utilizzo della classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Figura 12**: configurare ObjectDataSource per l'uso della classe `EmployeesBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))

[per ![usare ObjectDataSource, usare i metodi GetEmployees e DeleteEmployee](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Figura 13**: fare in modo che ObjectDataSource usi i metodi `GetEmployees` e `DeleteEmployee` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))

In Visual Studio viene aggiunto un BoundField al GridView per ognuna delle colonne `EmployeesDataTable`. Rimuovere tutti i BoundField, ad eccezione di `Title`, `LastName`, `FirstName`, `ManagerFirstName`e `ManagerLastName` e rinominare rispettivamente le proprietà `HeaderText` per gli ultimi quattro BoundField in cognome, nome, Manager e nome e Manager s cognome.

Per consentire agli utenti di eliminare i dipendenti da questa pagina, è necessario eseguire due operazioni. Per prima cosa, indicare a GridView di fornire funzionalità di eliminazione selezionando l'opzione Abilita eliminazione dallo smart tag. In secondo luogo, modificare la proprietà `OldValuesParameterFormatString` di ObjectDataSource dal valore impostato dalla procedura guidata ObjectDataSource (`original_{0}`) al valore predefinito (`{0}`). Dopo aver apportato queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Testare la pagina visitando il browser. Come illustrato nella figura 14, nella pagina sono elencati tutti i dipendenti e il nome del suo Manager (presupponendo che abbiano uno).

[![il JOIN nella stored procedure Employees_Select restituisce il nome del Manager](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Figura 14**: il `JOIN` nella stored procedure `Employees_Select` restituisce il nome del Manager ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))

Facendo clic sul pulsante Elimina viene avviato il flusso di lavoro di eliminazione, che culmina nell'esecuzione del `Employees_Delete` stored procedure. Tuttavia, l'istruzione `DELETE` tentata nel stored procedure ha esito negativo a causa di una violazione del vincolo di chiave esterna (vedere la figura 15). In particolare, ogni dipendente ha uno o più record nella tabella `Orders`, causando l'esito negativo dell'eliminazione.

[![l'eliminazione di un dipendente con ordini corrispondenti comporta una violazione del vincolo di chiave esterna](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Figura 15**: l'eliminazione di un dipendente con ordini corrispondenti comporta una violazione del vincolo di chiave esterna ([fare clic per visualizzare l'immagine con dimensioni complete](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))

Per consentire l'eliminazione di un dipendente, è possibile:

- Aggiornare il vincolo FOREIGN KEY alle eliminazioni a catena,
- Eliminare manualmente i record dalla tabella `Orders` per i dipendenti che si desidera eliminare.
- Aggiornare i stored procedure di `Employees_Delete` per eliminare prima di tutto i record correlati dalla tabella `Orders` prima di eliminare il record `Employees`. Questa tecnica è stata illustrata nell'esercitazione [utilizzo di stored procedure esistenti per i TableAdapter tipizzati del set di dati](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) .

Lo lascio come esercizio per il lettore.

## <a name="summary"></a>Riepilogo

Quando si utilizzano i database relazionali, è normale che le query estraggano i dati da più tabelle correlate. Le sottoquery correlate e `JOIN` s forniscono due tecniche diverse per accedere ai dati dalle tabelle correlate in una query. Nelle esercitazioni precedenti abbiamo usato più di frequente le sottoquery correlate perché TableAdapter non è in grado di generare automaticamente istruzioni `INSERT`, `UPDATE`e `DELETE` per le query che comportano `JOIN` s. Anche se questi valori possono essere forniti manualmente, quando si usano istruzioni SQL ad hoc, le personalizzazioni verranno sovrascritte al termine della configurazione guidata TableAdapter.

Fortunatamente, gli oggetti TableAdapter creati utilizzando le stored procedure non soffrono della stessa fragilità di quelli creati mediante istruzioni SQL ad hoc. Pertanto, è possibile creare un oggetto TableAdapter la cui query principale utilizza un `JOIN` quando si utilizzano stored procedure. In questa esercitazione è stato illustrato come creare un oggetto TableAdapter. È stata avviata usando una query di `SELECT` senza `JOIN`per la query principale di TableAdapter in modo da creare automaticamente le stored procedure di inserimento, aggiornamento ed eliminazione corrispondenti. Con la configurazione iniziale di TableAdapter completata, è stata aumentata la `SelectCommand` stored procedure per usare un `JOIN` ed è stata rieseguita la configurazione guidata TableAdapter per aggiornare le colonne `EmployeesDataTable`.

Se si esegue nuovamente la configurazione guidata TableAdapter, le colonne `EmployeesDataTable` verranno aggiornate in modo da riflettere i campi dati restituiti dalla stored procedure `Employees_Select`. In alternativa, è possibile che queste colonne siano state aggiunte manualmente alla DataTable. Nell'esercitazione successiva si analizzerà manualmente l'aggiunta di colonne alla DataTable.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Hilton Geisenow, David Suru e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Successivo](adding-additional-datatable-columns-cs.md)
