---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Utilizzo di stored procedure esistenti per gli oggetti TableAdapter del set di dati tipizzato (VB) | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente si è appreso come utilizzare la procedura guidata TableAdapter per generare nuove stored procedure. In questa esercitazione si apprenderà come lo stesso TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74613859"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Uso di stored procedure esistenti per i TableAdapter del set di dati tipizzato (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) o [Scarica PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Nell'esercitazione precedente si è appreso come utilizzare la procedura guidata TableAdapter per generare nuove stored procedure. In questa esercitazione viene illustrato come è possibile utilizzare la stessa procedura guidata TableAdapter con le stored procedure esistenti. Viene inoltre illustrato come aggiungere manualmente nuove stored procedure al database.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) è stato illustrato come è possibile configurare gli oggetti TableAdapter del set di dati tipizzati per l'utilizzo di stored procedure per accedere ai dati invece che a istruzioni SQL ad hoc. In particolare, è stato esaminato come fare in modo che la procedura guidata TableAdapter crei automaticamente queste stored procedure. Quando si esegue il porting di un'applicazione legacy a ASP.NET 2,0 o quando si crea un sito Web ASP.NET 2,0 per un modello di dati esistente, è probabile che il database contenga già le stored procedure necessarie. In alternativa, è possibile che si preferisca creare le stored procedure manualmente o tramite uno strumento diverso dalla procedura guidata TableAdapter che genera automaticamente le stored procedure.

In questa esercitazione verrà illustrato come configurare il TableAdapter per l'utilizzo di stored procedure esistenti. Poiché il database Northwind dispone solo di un piccolo set di stored procedure predefinite, si esamineranno anche i passaggi necessari per aggiungere manualmente nuove stored procedure al database tramite l'ambiente di Visual Studio. Inizia subito.

> [!NOTE]
> Nell'esercitazione [wrapping delle modifiche al database all'interno di una transazione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) sono stati aggiunti metodi al TableAdapter per supportare le transazioni (`BeginTransaction`, `CommitTransaction`e così via). In alternativa, le transazioni possono essere gestite interamente all'interno di una stored procedure, che non richiede alcuna modifica al codice del livello di accesso ai dati. In questa esercitazione verranno esaminati i comandi T-SQL utilizzati per eseguire le istruzioni di stored procedure s nell'ambito di una transazione.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Passaggio 1: aggiunta di stored procedure al database Northwind

Visual Studio semplifica l'aggiunta di nuove stored procedure a un database. Consente di aggiungere un nuovo stored procedure al database Northwind che restituisce tutte le colonne della tabella `Products` per quelle che hanno un valore `CategoryID` specifico. Dalla finestra Esplora server espandere il database Northwind in modo che vengano visualizzate le relative cartelle, ovvero diagrammi di database, tabelle, viste e così via. Come illustrato nell'esercitazione precedente, la cartella stored procedure contiene le stored procedure esistenti del database. Per aggiungere una nuova stored procedure, è sufficiente fare clic con il pulsante destro del mouse sulla cartella stored procedure e scegliere l'opzione Aggiungi nuova stored procedure dal menu di scelta rapida.

[![fare clic con il pulsante destro del mouse sulla cartella stored procedure e aggiungere una nuova stored procedure](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: fare clic con il pulsante destro del mouse sulla cartella stored procedure e aggiungere una nuova stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))

Come illustrato nella figura 1, la selezione dell'opzione Aggiungi nuova stored procedure consente di aprire una finestra di script in Visual Studio con il contorno dello script SQL necessario per creare il stored procedure. Il nostro lavoro consiste nel creare questo script ed eseguirlo. a questo punto, il stored procedure verrà aggiunto al database.

Immettere lo script seguente:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Questo script, quando viene eseguito, aggiungerà un nuovo stored procedure al database Northwind denominato `Products_SelectByCategoryID`. Questo stored procedure accetta un solo parametro di input (`@CategoryID`, di tipo `int`) e restituisce tutti i campi per quei prodotti con un valore di `CategoryID` corrispondente.

Per eseguire questo script `CREATE PROCEDURE` e aggiungere il stored procedure al database, fare clic sull'icona Salva sulla barra degli strumenti o premere CTRL + S. Dopo questa operazione, la cartella stored procedure viene aggiornata, mostrando il stored procedure appena creato. Inoltre, lo script nella finestra cambierà sottigliezza da `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` a `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` aggiunge un nuovo stored procedure al database, mentre `ALTER PROCEDURE` ne aggiorna uno esistente. Poiché l'inizio dello script è stato modificato in `ALTER PROCEDURE`, la modifica dei parametri di input delle stored procedure o delle istruzioni SQL e la selezione dell'icona Salva aggiorneranno il stored procedure con queste modifiche.

La figura 2 Mostra Visual Studio dopo il salvataggio del stored procedure di `Products_SelectByCategoryID`.

[![il Products_SelectByCategoryID della stored procedure è stato aggiunto al database](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: la stored procedure `Products_SelectByCategoryID` è stata aggiunta al database ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Passaggio 2: configurazione del TableAdapter per l'utilizzo di una stored procedure esistente

Ora che il `Products_SelectByCategoryID` stored procedure è stato aggiunto al database, è possibile configurare il livello di accesso ai dati per usare questa stored procedure quando viene richiamato uno dei metodi. In particolare, si aggiungerà un metodo di `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->` al `ProductsTableAdapter` nel set di dati `NorthwindWithSprocs` tipizzato che chiama il `Products_SelectByCategoryID` stored procedure appena creato.

Per iniziare, aprire il set di dati `NorthwindWithSprocs`. Fare clic con il pulsante destro del mouse sul `ProductsTableAdapter` e scegliere Aggiungi query per avviare la configurazione guidata query TableAdapter. Nell' [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) si è scelto di fare in modo che TableAdapter crei un nuovo stored procedure per noi. Per questa esercitazione, tuttavia, si vuole collegare il nuovo metodo TableAdapter al stored procedure di `Products_SelectByCategoryID` esistente. Quindi, scegliere l'opzione Usa stored procedure esistente dal primo passaggio della procedura guidata e quindi fare clic su Avanti.

[![scegliere l'opzione Usa stored procedure esistente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: scegliere l'opzione Usa stored procedure esistente ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

Nella schermata seguente è disponibile un elenco a discesa popolato con le stored procedure del database. Selezionando una stored procedure vengono elencati i parametri di input a sinistra e i campi dati restituiti (se presenti) a destra. Scegliere il stored procedure `Products_SelectByCategoryID` dall'elenco e fare clic su Avanti.

[![selezionare la stored procedure Products_SelectByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: selezionare la stored procedure `Products_SelectByCategoryID` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

Nella schermata successiva viene richiesto il tipo di dati restituito dal stored procedure e la risposta in questo articolo determina il tipo restituito dal metodo TableAdapter. Se, ad esempio, si indica che vengono restituiti dati tabulari, il metodo restituirà un'istanza di `ProductsDataTable` popolata con i record restituiti dall'stored procedure. Al contrario, se si indica che questo stored procedure restituisce un valore singolo, TableAdapter restituirà un `Object` a cui viene assegnato il valore nella prima colonna del primo record restituito dal stored procedure.

Poiché il `Products_SelectByCategoryID` stored procedure restituisce tutti i prodotti appartenenti a una determinata categoria, scegliere la prima dati tabulare di risposta, quindi fare clic su Avanti.

[![indicare che la stored procedure restituisce dati tabulari](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicare che la stored procedure restituisce dati tabulari ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Ciò che rimane è quello di indicare i modelli di metodo da usare seguiti dai nomi per questi metodi. Lasciare selezionata l'opzione Compila un DataTable e restituire le opzioni DataTable, ma rinominare i metodi `FillByCategoryID` e `GetProductsByCategoryID`. Quindi fare clic su Avanti per esaminare un riepilogo delle attività che verranno eseguite dalla procedura guidata. Se tutto sembra corretto, fare clic su fine.

[![il nome dei metodi FillByCategoryID e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: assegnare un nome ai metodi `FillByCategoryID` e `GetProductsByCategoryID` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> I metodi TableAdapter appena creati, `FillByCategoryID` e `GetProductsByCategoryID`, prevedono un parametro di input di tipo `Integer`. Il valore del parametro di input viene passato nel stored procedure tramite il parametro di `@CategoryID`. Se si modificano i parametri `Products_SelectByCategory` stored procedure s, sarà necessario aggiornare anche i parametri per questi metodi TableAdapter. Come illustrato nell' [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), questa operazione può essere eseguita in uno dei due modi seguenti: aggiungendo o rimuovendo manualmente i parametri dalla raccolta Parameters o rieseguendo la procedura guidata TableAdapter.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Passaggio 3: aggiunta di un metodo di`GetProductsByCategoryID(categoryID)`a BLL

Una volta completato il `GetProductsByCategoryID` DAL metodo, il passaggio successivo consiste nel fornire l'accesso a questo metodo nel livello della logica di business. Aprire il file di classe `ProductsBLLWithSprocs` e aggiungere il metodo seguente:

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Questo metodo BLL restituisce semplicemente il `ProductsDataTable` restituito dal metodo `ProductsTableAdapter` s `GetProductsByCategoryID`. L'attributo `DataObjectMethodAttribute` fornisce i metadati utilizzati dalla configurazione guidata origine dati di ObjectDataSource. In particolare, questo metodo verrà visualizzato nell'elenco a discesa Seleziona scheda.

## <a name="step-4-displaying-products-by-category"></a>Passaggio 4: visualizzazione dei prodotti per categoria

Per testare la stored procedure `Products_SelectByCategoryID` appena aggiunta e i metodi DAL e BLL corrispondenti, è possibile creare una pagina ASP.NET contenente un oggetto DropDownList e GridView. Il controllo DropDownList elenca tutte le categorie nel database mentre GridView visualizzerà i prodotti appartenenti alla categoria selezionata.

> [!NOTE]
> Nelle esercitazioni precedenti sono state create interfacce Master/Details con i controlli DropDownList. Per un'analisi più approfondita dell'implementazione di un report master/dettagli, vedere l'esercitazione relativa al [filtro master/dettagli con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) .

Aprire la pagina `ExistingSprocs.aspx` nella cartella `AdvancedDAL` e trascinare un oggetto DropDownList dalla casella degli strumenti nella finestra di progettazione. Impostare la proprietà `ID` di DropDownList su `Categories` e la relativa proprietà `AutoPostBack` su `True`. Quindi, dallo smart tag, associare il DropDownList a un nuovo ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource in modo che recuperi i dati dal metodo `GetCategories` della classe `CategoriesBLL`. Impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno).

[![recuperare i dati dal metodo CategoriesBLL della classe s GetCategories](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperare dati dal metodo `GetCategories` della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Dopo aver completato la procedura guidata ObjectDataSource, configurare DropDownList per visualizzare il campo dati `CategoryName` e per usare il campo `CategoryID` come `Value` per ogni `ListItem`.

A questo punto, il markup dichiarativo DropDownList e ObjectDataSource è simile al seguente:

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Trascinare quindi un controllo GridView nella finestra di progettazione, inserendolo sotto il controllo DropDownList. Impostare il `ID` di GridView su `ProductsByCategory` e, dal relativo smart tag, associarlo a un nuovo ObjectDataSource denominato `ProductsByCategoryDataSource`. Configurare il `ProductsByCategoryDataSource` ObjectDataSource per utilizzare la classe `ProductsBLLWithSprocs`, facendo in modo che recuperi i dati utilizzando il metodo `GetProductsByCategoryID(categoryID)`. Poiché questo controllo GridView verrà usato solo per visualizzare i dati, impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) e fare clic su Avanti.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurare ObjectDataSource per l'uso della classe `ProductsBLLWithSprocs` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![recuperare dati dal metodo GetProductsByCategoryID (categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperare dati dal metodo `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

Il metodo scelto nella scheda SELECT prevede un parametro, quindi il passaggio finale della procedura guidata richiede l'origine del parametro. Impostare l'elenco a discesa parametro source (origine parametro) per controllare e scegliere il controllo `Categories` dall'elenco a discesa ControlID. Fare clic su Fine per completare la procedura guidata.

[![utilizzare il DropDownList categorie come origine del parametro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: usare il `Categories` DropDownList come origine del parametro `categoryID` ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Al termine della procedura guidata ObjectDataSource, Visual Studio aggiungerà i BoundField e un CheckBoxField per ognuno dei campi dati del prodotto. È possibile personalizzare questi campi in base alle esigenze.

Visita la pagina tramite un browser. Quando si visita la pagina, viene selezionata la categoria bevande e i prodotti corrispondenti elencati nella griglia. Se si modifica l'elenco a discesa in una categoria alternativa, come illustrato nella figura 12, viene generato un postback e viene ricaricata la griglia con i prodotti della categoria appena selezionata.

[![vengono visualizzati i prodotti nella categoria produce](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: vengono visualizzati i prodotti nella categoria produce ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Passaggio 5: wrapping di istruzioni di una stored procedure nell'ambito di una transazione

Nelle [modifiche di wrapping del database all'interno di un'](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) esercitazione sulle transazioni sono state illustrate le tecniche per l'esecuzione di una serie di istruzioni di modifica del database nell'ambito di una transazione. Tenere presente che le modifiche apportate con l'ombrello di una transazione hanno esito positivo o negativo, garantendo l'atomicità. Le tecniche per l'utilizzo delle transazioni includono:

- Utilizzando le classi nello spazio dei nomi `System.Transactions`,
- Il livello di accesso ai dati usa classi ADO.NET come `SqlTransaction`e
- Aggiunta dei comandi di transazione T-SQL direttamente all'interno dell'stored procedure

Il *wrapping delle modifiche al database all'interno di un'* esercitazione sulle transazioni ha utilizzato le classi ADO.NET in dal. Nella parte restante di questa esercitazione viene esaminato come gestire una transazione utilizzando i comandi T-SQL dall'interno di una stored procedure.

I tre comandi SQL principali per l'avvio manuale, il commit e il rollback di una transazione sono rispettivamente `BEGIN TRANSACTION`, `COMMIT TRANSACTION`e `ROLLBACK TRANSACTION`. Analogamente all'approccio ADO.NET, quando si usano le transazioni all'interno di un stored procedure è necessario applicare il modello seguente:

1. Indica l'inizio di una transazione.
2. Eseguire le istruzioni SQL che compongono la transazione.
3. Se si verifica un errore in una delle istruzioni del passaggio 2, eseguire il rollback della transazione.
4. Se tutte le istruzioni del passaggio 2 sono state completate senza errori, eseguire il commit della transazione.

Questo modello può essere implementato nella sintassi T-SQL usando il modello seguente:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Il modello inizia definendo un blocco di `TRY...CATCH`, un costrutto nuovo per SQL Server 2005. Analogamente a quanto avviene con `Try...Catch` blocchi in Visual Basic, il blocco SQL `TRY...CATCH` esegue le istruzioni nel blocco `TRY`. Se un'istruzione genera un errore, il controllo viene immediatamente trasferito al blocco `CATCH`.

Se non sono presenti errori durante l'esecuzione delle istruzioni SQL che comportano la composizione della transazione, l'istruzione `COMMIT TRANSACTION` esegue il commit delle modifiche e completa la transazione. Se, tuttavia, una delle istruzioni genera un errore, il `ROLLBACK TRANSACTION` nel blocco `CATCH` restituisce lo stato del database prima dell'avvio della transazione. Il stored procedure genera inoltre un errore utilizzando il [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), che determina la generazione di un `SqlException` nell'applicazione.

> [!NOTE]
> Poiché il blocco `TRY...CATCH` è nuovo per SQL Server 2005, il modello precedente non funzionerà se si usano versioni precedenti di Microsoft SQL Server. Se non si utilizza SQL Server 2005, consultare la pagina relativa alla [gestione delle transazioni in SQL Server stored procedure](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per un modello che funzionerà con altre versioni di SQL Server.

Esaminiamo un esempio concreto. Esiste un vincolo di chiave esterna tra le tabelle `Categories` e `Products`, vale a dire che ogni campo `CategoryID` della tabella `Products` deve eseguire il mapping a un valore `CategoryID` nella tabella `Categories`. Qualsiasi azione che violerebbe questo vincolo, ad esempio il tentativo di eliminare una categoria con prodotti associati, comporta una violazione del vincolo di chiave esterna. Per verificarlo, vedere l'esempio relativo all'aggiornamento e all'eliminazione di dati binari esistenti nella sezione Utilizzo di dati binari (`~/BinaryData/UpdatingAndDeleting.aspx`). In questa pagina viene elencata ogni categoria del sistema insieme ai pulsanti modifica ed Elimina (vedere la figura 13), ma se si tenta di eliminare una categoria con prodotti associati, ad esempio bibite, l'eliminazione non riesce a causa di una violazione del vincolo di chiave esterna (vedere la figura 14).

[![ogni categoria viene visualizzata in un GridView con i pulsanti modifica ed Elimina](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: ogni categoria viene visualizzata in un GridView con i pulsanti modifica ed Elimina ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![non è possibile eliminare una categoria con prodotti esistenti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: non è possibile eliminare una categoria con prodotti esistenti ([fare clic per visualizzare l'immagine con dimensioni complete](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

Si supponga, tuttavia, che si desideri consentire l'eliminazione delle categorie, indipendentemente dal fatto che siano associati a prodotti. Se una categoria con prodotti deve essere eliminata, si supponga di voler eliminare anche i prodotti esistenti (anche se un'altra opzione consiste nel impostare i propri prodotti `CategoryID` valori `NULL`). Questa funzionalità può essere implementata tramite le regole Cascade del vincolo FOREIGN KEY. In alternativa, è possibile creare un stored procedure che accetti un parametro di input `@CategoryID` e, quando viene richiamato, elimini in modo esplicito tutti i prodotti associati e quindi la categoria specificata.

Il primo tentativo in questo stored procedure potrebbe essere simile al seguente:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Anche se questa operazione eliminerà definitivamente i prodotti e la categoria associati, questa operazione non viene eseguita nell'ambito di una transazione. Si supponga che esistano altri vincoli di chiave esterna per `Categories` che impediscono l'eliminazione di un determinato valore di `@CategoryID`. Il problema è che, in tal caso, tutti i prodotti verranno eliminati prima di provare a eliminare la categoria. Il risultato finale è che per tale categoria, questo stored procedure rimuoverà tutti i prodotti mentre la categoria è rimasta perché contiene ancora record correlati in un'altra tabella.

Se la stored procedure è stata racchiusa nell'ambito di una transazione, tuttavia, le eliminazioni nella tabella `Products` verrebbero sottoposte a rollback in caso di eliminazione non riuscita sul `Categories`. Nello script di stored procedure seguente viene utilizzata una transazione per garantire l'atomicità tra le due istruzioni `DELETE`:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Aggiungere il `Categories_Delete` stored procedure al database Northwind. Per istruzioni sull'aggiunta di stored procedure a un database, fare riferimento al passaggio 1.

## <a name="step-6-updating-thecategoriestableadapter"></a>Passaggio 6: aggiornamento del`CategoriesTableAdapter`

Sebbene sia stata aggiunta la `Categories_Delete` stored procedure al database, il DAL è attualmente configurato per l'uso di istruzioni SQL ad hoc per l'esecuzione dell'eliminazione. È necessario aggiornare il `CategoriesTableAdapter` e indicare all'utente di usare invece il stored procedure `Categories_Delete`.

> [!NOTE]
> In precedenza in questa esercitazione abbiamo lavorato con il set di dati `NorthwindWithSprocs`. Ma il set di dati ha solo una singola entità, `ProductsDataTable`, ed è necessario lavorare con le categorie. Quindi, nella parte restante di questa esercitazione, quando parlerò del livello di accesso ai dati, mi riferisco al set di dati `Northwind`, quello creato per la prima volta nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) .

Aprire il set di dati Northwind, selezionare il `CategoriesTableAdapter`e passare al Finestra Proprietà. Il Finestra Proprietà elenca i `InsertCommand`, `UpdateCommand`, `DeleteCommand`e `SelectCommand` utilizzati dal TableAdapter, nonché le informazioni relative al nome e alla connessione. Espandere la proprietà `DeleteCommand` per visualizzarne i dettagli. Come illustrato nella figura 15, la proprietà `CommandType` `DeleteCommand` s è impostata su text, che indica all'utente di inviare il testo nella proprietà `CommandText` come query SQL ad hoc.

![Selezionare il CategoriesTableAdapter nella finestra di progettazione per visualizzarne le proprietà nella finestra Proprietà](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: selezionare il `CategoriesTableAdapter` nella finestra di progettazione per visualizzarne le proprietà nella finestra Proprietà

Per modificare queste impostazioni, selezionare il testo (DeleteCommand) nell'Finestra Proprietà e scegliere (nuovo) nell'elenco a discesa. Questa operazione eliminerà le impostazioni per le proprietà `CommandText`, `CommandType`e `Parameters`. Impostare quindi la proprietà `CommandType` su `StoredProcedure`, quindi digitare il nome del stored procedure per l'`CommandText` (`dbo.Categories_Delete`). Se si ha la certezza di immettere le proprietà in questo ordine, innanzitutto il `CommandType` e quindi il `CommandText`-Visual Studio compilerà automaticamente la raccolta di parametri. Se non si immettono queste proprietà in questo ordine, sarà necessario aggiungere manualmente i parametri tramite l'editor della raccolta Parameters. In entrambi i casi, è prudente fare clic sui puntini di sospensione nella proprietà Parameters per visualizzare l'editor della raccolta Parameters per verificare che siano state apportate le modifiche corrette alle impostazioni dei parametri (vedere la figura 16). Se nella finestra di dialogo non vengono visualizzati parametri, aggiungere manualmente il parametro `@CategoryID` (non è necessario aggiungere il parametro `@RETURN_VALUE`).

![Verificare che le impostazioni dei parametri siano corrette](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: assicurarsi che le impostazioni dei parametri siano corrette

Una volta che il DAL è stato aggiornato, l'eliminazione di una categoria eliminerà automaticamente tutti i prodotti associati ed eseguirà questa operazione sotto l'ombrello di una transazione. Per verificarlo, tornare alla pagina aggiornamento ed eliminazione di dati binari esistenti e fare clic sul pulsante Elimina per una delle categorie. Con un solo clic del mouse, la categoria e tutti i prodotti associati verranno eliminati.

> [!NOTE]
> Prima di testare l'stored procedure `Categories_Delete`, che eliminerà una serie di prodotti insieme alla categoria selezionata, potrebbe essere opportuno eseguire una copia di backup del database. Se si usa il database di `NORTHWND.MDF` in `App_Data`, è sufficiente chiudere Visual Studio e copiare i file MDF e LDF in `App_Data` in un'altra cartella. Dopo aver testato la funzionalità, è possibile ripristinare il database chiudendo Visual Studio e sostituendo i file MDF e LDF correnti in `App_Data` con le copie di backup.

## <a name="summary"></a>Riepilogo

Mentre la procedura guidata di TableAdapter genera automaticamente le stored procedure, in alcuni casi è possibile che tali stored procedure siano già state create o che si voglia crearle manualmente o con altri strumenti. Per gestire tali scenari, è anche possibile configurare TableAdapter in modo che punti a un stored procedure esistente. In questa esercitazione è stato illustrato come aggiungere manualmente stored procedure a un database tramite l'ambiente Visual Studio e come collegare i metodi di TableAdapter a tali stored procedure. Sono stati anche esaminati i comandi T-SQL e il modello di script usati per avviare, eseguire il commit e il rollback delle transazioni dall'interno di una stored procedure.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Hilton Geisenow, S Ren Jacob Lauritsen e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Successivo](updating-the-tableadapter-to-use-joins-vb.md)
