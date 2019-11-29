---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Esecuzione di query sui dati con il controllo SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato usato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questo tutor...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 199ddb46e877c3a0937672d33241a240660684da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606098"
---
# <a name="querying-data-with-the-sqldatasource-control-vb"></a>Esecuzione di query sui dati con il controllo SqlDataSource (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) o [scaricare il file PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> Nelle esercitazioni precedenti è stato usato il controllo ObjectDataSource per separare completamente il livello di presentazione dal livello di accesso ai dati. A partire da questa esercitazione si apprenderà come il controllo SqlDataSource possa essere usato per applicazioni semplici che non richiedono una separazione restrittiva di presentazione e accesso ai dati.

## <a name="introduction"></a>Introduzione

Tutte le esercitazioni esaminate finora hanno usato un'architettura a più livelli costituita dai livelli di presentazione, logica di business e accesso ai dati. Il livello di accesso ai dati (DAL) è stato creato nella prima esercitazione ([creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md)) e nel secondo livello della logica di business ([creazione di un livello di logica di business](../introduction/creating-a-business-logic-layer-vb.md)). A partire dall'esercitazione [visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) , è stato illustrato come utilizzare ASP.NET 2,0 s nuovo controllo ObjectDataSource per interfacciarsi in modo dichiarativo con l'architettura del livello di presentazione.

Mentre tutte le esercitazioni finora hanno usato l'architettura per lavorare con i dati, è anche possibile accedere, inserire, aggiornare ed eliminare i dati del database direttamente da una pagina di ASP.NET, ignorando l'architettura. In questo modo, le query di database e la logica di business specifiche vengono inserite direttamente nella pagina Web. Per applicazioni sufficientemente grandi o complesse, la progettazione, l'implementazione e l'uso di un'architettura a più livelli è estremamente importante per il successo, la aggiornabilità e la gestibilità dell'applicazione. Tuttavia, lo sviluppo di un'architettura affidabile può non essere necessario quando si creano applicazioni semplici e estremamente semplici.

ASP.NET 2,0 fornisce cinque controlli origine dati incorporati [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Il SqlDataSource può essere usato per accedere ai dati e modificarli direttamente da un database relazionale, tra cui Microsoft SQL Server, Microsoft Access, Oracle, MySQL e altri. In questa esercitazione e nei prossimi tre verrà illustrato come utilizzare il controllo SqlDataSource, viene illustrato come eseguire una query e filtrare i dati del database, nonché come utilizzare SqlDataSource per inserire, aggiornare ed eliminare dati.

![ASP.NET 2,0 include cinque controlli origine dati incorporati](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Figura 1**: ASP.NET 2,0 include cinque controlli dell'origine dati incorporati

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Confronto tra ObjectDataSource e SqlDataSource

Concettualmente, i controlli ObjectDataSource e SqlDataSource sono semplicemente proxy ai dati. Come illustrato nell'esercitazione [visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) , ObjectDataSource dispone di proprietà che indicano il tipo di oggetto che fornisce i dati e i metodi da richiamare per selezionare, inserire, aggiornare ed eliminare dati dal tipo di oggetto sottostante. Una volta configurate le proprietà di ObjectDataSource, un controllo Web dei dati come GridView, DetailsView o DataList può essere associato al controllo, usando i metodi `Select()`, `Insert()`, `Delete()`e `Update()` di ObjectDataSource per interagire con l'architettura sottostante.

Il SqlDataSource fornisce la stessa funzionalità, ma opera su un database relazionale anziché una libreria di oggetti. Con il SqlDataSource è necessario specificare la stringa di connessione al database e le query SQL ad hoc o le stored procedure da eseguire per inserire, aggiornare, eliminare e recuperare i dati. I metodi SqlDataSource s `Select()`, `Insert()`, `Update()`e `Delete()`, quando vengono richiamati, si connettono al database specificato ed emettono la query SQL appropriata. Come illustrato nel diagramma seguente, questi metodi eseguono le operazioni di connessione a un database, l'emissione di una query e la restituzione dei risultati.

![Il SqlDataSource funge da proxy per il database](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Figura 2**: SqlDataSource funge da proxy per il database

> [!NOTE]
> In questa esercitazione si concentrerà sul recupero dei dati dal database. Nell'esercitazione [inserimento, aggiornamento ed eliminazione di dati con il controllo SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) verrà illustrato come configurare SqlDataSource per supportare l'inserimento, l'aggiornamento e l'eliminazione.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Controlli SqlDataSource e AccessDataSource

Oltre al controllo SqlDataSource, ASP.NET 2,0 include anche un controllo AccessDataSource. Questi due diversi controlli portano molti sviluppatori nuovi a ASP.NET 2,0 per sospettare che il controllo AccessDataSource sia progettato per funzionare esclusivamente con Microsoft Access con il controllo SqlDataSource progettato per funzionare esclusivamente con Microsoft SQL Server. Mentre l'oggetto AccessDataSource è progettato per funzionare in modo specifico con Microsoft Access, il controllo SqlDataSource funziona con *qualsiasi* database relazionale a cui è possibile accedere tramite .NET. Sono inclusi tutti gli archivi dati conformi a OLE DB o ODBC, ad esempio Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL e PostgreSQL, tra le molte altre.

L'unica differenza tra i controlli AccessDataSource e SqlDataSource è il modo in cui vengono specificate le informazioni di connessione al database. Per il controllo AccessDataSource è necessario solo il percorso del file del database di Access. Il SqlDataSource, d'altra parte, richiede una stringa di connessione completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Passaggio 1: creazione delle pagine Web di SqlDataSource

Prima di iniziare a esplorare il modo in cui lavorare direttamente con i dati del database usando il controllo SqlDataSource, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le tre successive. Per iniziare, aggiungere una nuova cartella denominata `SqlDataSource`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Figura 3**: aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource

Analogamente alle altre cartelle, `Default.aspx` nella cartella `SqlDataSource` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Figura 4**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))

Infine, aggiungere le quattro pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo avere aggiunto i pulsanti personalizzati a DataList e Repeater `<siteMapNode>`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica, inserimento ed eliminazione.

![La mappa del sito include ora le voci per le esercitazioni di SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Figura 5**: la mappa del sito include ora le voci per le esercitazioni di SqlDataSource

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Passaggio 2: aggiunta e configurazione del controllo SqlDataSource

Per iniziare, aprire la pagina `Querying.aspx` nella cartella `SqlDataSource` e passare alla visualizzazione progettazione. Trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione e impostare la relativa `ID` su `ProductsDataSource`. Come per ObjectDataSource, il SqlDataSource non produce alcun output sottoposto a rendering e pertanto viene visualizzato come una casella grigia nell'area di progettazione. Per configurare il SqlDataSource, fare clic sul collegamento Configura origine dati dallo smart tag SqlDataSource s.

![Fare clic sul collegamento Configura origine dati dallo smart tag SqlDataSource s](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Figura 6**: fare clic sul collegamento Configura origine dati dallo smart tag SqlDataSource s

Viene visualizzata la configurazione guidata origine dati del controllo SqlDataSource. Mentre i passaggi della procedura guidata sono diversi da quelli del controllo ObjectDataSource, l'obiettivo finale è lo stesso per fornire i dettagli su come recuperare, inserire, aggiornare ed eliminare dati tramite l'origine dati. Per il SqlDataSource questo comporta l'impostazione del database sottostante da usare e la fornitura di istruzioni SQL ad hoc o stored procedure.

Il primo passaggio della procedura guidata richiede il database. Nell'elenco a discesa sono inclusi i database disponibili nella cartella `App_Data` dell'applicazione Web e quelli che sono stati aggiunti al nodo Connessioni dati nel Esplora server. Poiché è già stata aggiunta una stringa di connessione per il database `NORTHWIND.MDF` nella cartella `App_Data` al file `Web.config` del progetto, l'elenco a discesa include un riferimento a tale stringa di connessione, `NORTHWINDConnectionString`. Scegliere questo elemento dall'elenco a discesa e fare clic su Avanti.

![Scegliere NORTHWINDConnectionString dall'elenco a discesa](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Figura 7**: scegliere il `NORTHWINDConnectionString` dall'elenco a discesa

Dopo aver scelto il database, la procedura guidata chiede alla query di restituire i dati. È possibile specificare le colonne di una tabella o di una vista da restituire oppure è possibile immettere un'istruzione SQL personalizzata o specificare un stored procedure. È possibile passare da una scelta all'altra tramite il specificare un'istruzione SQL personalizzata o stored procedure e specificare le colonne da una tabella o da una vista pulsanti di opzione.

> [!NOTE]
> Per questo primo esempio, consente di utilizzare l'opzione specificare le colonne da una tabella o una vista. Si tornerà alla procedura guidata più avanti in questa esercitazione e si esplorerà l'opzione specificare un'istruzione SQL personalizzata o stored procedure.

Nella figura 8 viene illustrata la schermata Configura istruzione SELECT quando si seleziona il pulsante di opzione specificare le colonne da una tabella o una vista. Nell'elenco a discesa è contenuto il set di tabelle e viste del database Northwind, con le colonne della tabella o della vista selezionata visualizzate nell'elenco di caselle di controllo riportato di seguito. Per questo esempio, Let s restituisce le colonne `ProductID`, `ProductName`e `UnitPrice` della tabella `Products`. Come illustrato nella figura 8, dopo aver eseguito queste selezioni, la procedura guidata mostra l'istruzione SQL risultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.

![Restituisce i dati dalla tabella Products](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Figura 8**: restituire dati dalla tabella `Products`

Una volta configurata la procedura guidata per la restituzione delle colonne `ProductID`, `ProductName`e `UnitPrice` della tabella `Products`, fare clic sul pulsante Avanti. Questa schermata finale offre un'opportunità per esaminare i risultati della query configurata nel passaggio precedente. Facendo clic sul pulsante Test query viene eseguita l'istruzione `SELECT` configurata e i risultati vengono visualizzati in una griglia.

![Fare clic sul pulsante Test query per esaminare la query SELECT](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Figura 9**: fare clic sul pulsante Test query per esaminare la query `SELECT`

Per completare la procedura guidata, fare clic su Fine.

Analogamente a ObjectDataSource, la procedura guidata SqlDataSource s assegna semplicemente i valori alle proprietà del controllo, ovvero le proprietà [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) . Al termine della procedura guidata, il markup dichiarativo del controllo SqlDataSource avrà un aspetto simile al seguente:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

La proprietà `ConnectionString` fornisce informazioni su come connettersi al database. A questa proprietà può essere assegnato un valore della stringa di connessione hardcoded completo oppure può puntare a una stringa di connessione in `Web.config`. Per fare riferimento a un valore della stringa di connessione in Web. config, utilizzare la sintassi `<%$ expressionPrefix:expressionValue %>`. In genere, *ExpressionPrefix* è connectionStrings e *expressionValue* è il nome della stringa di connessione nella [sezione `Web.config``<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx). Tuttavia, la sintassi può essere usata per fare riferimento a `<appSettings>` elementi o contenuto dai file di risorse. Per ulteriori informazioni su questa sintassi, vedere [Cenni preliminari sulle espressioni ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) .

La proprietà `SelectCommand` specifica l'istruzione SQL ad hoc o stored procedure da eseguire per restituire i dati.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Passaggio 3: aggiunta di un controllo Web di dati e relativa associazione al SqlDataSource

Una volta configurato, il SqlDataSource può essere associato a un controllo Web dei dati, ad esempio GridView o DetailsView. Per questa esercitazione, consente di visualizzare i dati in un controllo GridView. Dalla casella degli strumenti trascinare un controllo GridView nella pagina e associarlo alla `ProductsDataSource` SqlDataSource scegliendo l'origine dati dall'elenco a discesa nello smart tag di GridView.

[![aggiungere GridView e associarlo al controllo SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Figura 10**: aggiungere GridView e associarlo al controllo SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))

Dopo aver selezionato il controllo SqlDataSource dall'elenco a discesa nello smart tag di GridView, Visual Studio aggiungerà automaticamente un BoundField o un oggetto CheckBoxField a GridView per ogni colonna restituita dal controllo origine dati. Poiché SqlDataSource restituisce tre colonne di database `ProductID`, `ProductName`e `UnitPrice` sono presenti tre campi in GridView.

Dedicare un po' di tempo alla configurazione dei tre BoundField di GridView. Modificare la proprietà `ProductName` Field s `HeaderText` in Product Name e il campo `UnitPrice` s in price. Formattare anche il campo `UnitPrice` come valuta. Dopo aver apportato queste modifiche, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Visitare questa pagina tramite un browser. Come illustrato nella figura 11, GridView elenca i valori `ProductID`, `ProductName`e `UnitPrice` di ogni prodotto.

[![GridView Visualizza i valori ProductID, ProductName e PrezzoUnitario di ogni prodotto.](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Figura 11**: GridView Visualizza i valori `ProductID`, `ProductName`e `UnitPrice` di ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))

Quando la pagina viene visitata, GridView richiama il metodo del controllo origine dati s `Select()`. Quando si utilizza il controllo ObjectDataSource, viene chiamato il metodo `ProductsBLL` Class s `GetProducts()`. Con il SqlDataSource, tuttavia, il metodo `Select()` stabilisce una connessione al database specificato e genera l'`SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, in questo esempio). Il SqlDataSource restituisce i risultati che GridView enumera, creando una riga in GridView per ogni record di database restituito.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funzionalità di controllo Web dei dati predefinite e controllo SqlDataSource

In generale, le funzionalità inerenti ai controlli Web dei dati il paging, l'ordinamento, la modifica, l'eliminazione, l'inserimento e così via sono specifiche del controllo Web dati e non dipendono dal controllo origine dati utilizzato. Ovvero GridView può utilizzare il paging, l'ordinamento, la modifica e l'eliminazione predefiniti se è associato a un ObjectDataSource o a un SqlDataSource. Alcune funzionalità di controllo Web dei dati, tuttavia, sono sensibili al controllo origine dati utilizzato o alla configurazione del controllo origine dati.

Ad esempio, in un'esercitazione di [paging efficiente con grandi quantità di dati](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) è stato illustrato come, per impostazione predefinita, la logica di paging per i controlli Web dati restituisce in modo ingenuo *tutti* i record dell'origine dati sottostante e quindi Visualizza solo il subset appropriato di record in base all'indice della pagina corrente e al numero di record da visualizzare per ogni pagina. Questo modello è estremamente inefficiente durante il paging di set di risultati sufficientemente grandi. Fortunatamente, ObjectDataSource può essere configurato per supportare il paging personalizzato, che restituisce solo il subset preciso di record da visualizzare. Il controllo SqlDataSource, tuttavia, non dispone delle proprietà per l'implementazione del paging personalizzato.

Un'altra sottigliezza con il paging e l'ordinamento si verifica con il SqlDataSource. Per impostazione predefinita, i dati restituiti da un SqlDataSource possono essere sottoposto a paging o ordinati tramite GridView. Per illustrare questo problema, selezionare le opzioni Abilita paging e Abilita ordinamento nello smart tag di GridView in `Querying.aspx` e verificare che funzioni come previsto.

L'ordinamento e il paging funzionano perché SqlDataSource recupera i dati del database in un set di dati debolmente tipizzato. Il numero totale di record restituiti dalla query un aspetto essenziale per l'implementazione del paging può essere verificato dal set di dati. Inoltre, i risultati del set di dati possono essere ordinati tramite un DataView. Queste funzionalità vengono usate automaticamente da SqlDataSource quando GridView richiede dati di paging o ordinati.

Il SqlDataSource può essere configurato per restituire un DataReader anziché un set di dati modificandone la [proprietà`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) da `DataSet` (impostazione predefinita) a `DataReader`. L'utilizzo di un DataReader potrebbe essere preferibile in situazioni in cui si passano i risultati di SqlDataSource al codice esistente che prevede un DataReader. Inoltre, poiché i DataReaders sono oggetti molto più semplici rispetto ai set di impostazioni, offrono prestazioni migliori. Se si apportano queste modifiche, tuttavia, il controllo Web dei dati non può né ordinare né pagina perché il SqlDataSource non è in grado di determinare il numero di record restituiti dalla query, né l'oggetto DataReader offre tecniche per l'ordinamento dei dati restituiti.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Passaggio 4: utilizzo di un'istruzione SQL personalizzata o di una stored procedure

Quando si configura il controllo SqlDataSource, la query utilizzata per restituire dati può essere specificata in uno dei due approcci come istruzione SQL personalizzata o stored procedure o come colonne da una tabella o vista esistente. Nel passaggio 2 è stata esaminata la selezione delle colonne dalla tabella `Products`. Viene ora esaminato l'utilizzo di un'istruzione SQL personalizzata.

Aggiungere un altro controllo GridView alla pagina `Querying.aspx` e scegliere di creare una nuova origine dati dall'elenco a discesa nello smart tag. Successivamente, indicare che i dati verranno estratti da un database. verrà creato un nuovo controllo SqlDataSource. Denominare il controllo `ProductsWithCategoryInfoDataSource`.

![Creare un nuovo controllo SqlDataSource denominato ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Figura 12**: creare un nuovo controllo SqlDataSource denominato `ProductsWithCategoryInfoDataSource`

Nella schermata successiva viene richiesto di specificare il database. Come illustrato nella figura 7, selezionare il `NORTHWINDConnectionString` dall'elenco a discesa e fare clic su Avanti. Nella schermata Configura istruzione SELECT scegliere il pulsante di opzione specificare un'istruzione SQL personalizzata o stored procedure e fare clic su Avanti. Verrà visualizzata la schermata Definisci istruzioni personalizzate o stored procedure, che offre schede con etichetta SELECT, UPDATE, INSERT e DELETE. In ogni scheda è possibile immettere un'istruzione SQL personalizzata nella casella di testo o scegliere una stored procedure dall'elenco a discesa. In questa esercitazione verrà esaminato l'immissione di un'istruzione SQL personalizzata. nell'esercitazione successiva è incluso un esempio in cui viene utilizzato un stored procedure.

![Immettere un'istruzione SQL personalizzata oppure selezionare una stored procedure](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Figura 13**: immettere un'istruzione SQL personalizzata o selezionare una stored procedure

L'istruzione SQL personalizzata può essere immessa manualmente nella casella di testo oppure può essere costruita graficamente facendo clic sul pulsante Generatore di query. Dal Generatore di query o dalla casella di testo, usare la query seguente per restituire i campi `ProductID` e `ProductName` della tabella `Products` usando un `JOIN` per recuperare il `CategoryName` del prodotto dalla tabella `Categories`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]

![È possibile creare graficamente la query usando il Generatore di query](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Figura 14**: è possibile creare graficamente la query usando il generatore di query

Dopo aver specificato la query, fare clic su Avanti per passare alla schermata Test query. Fare clic su fine per completare la procedura guidata SqlDataSource.

Al termine della procedura guidata, GridView disporrà di tre BoundField che visualizzano le colonne `ProductID`, `ProductName`e `CategoryName` restituite dalla query, ottenendo il markup dichiarativo seguente:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]

[![GridView Mostra l'ID, il nome e il nome di categoria associato di ogni prodotto](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Figura 15**: il controllo GridView Mostra ogni ID, nome e nome di categoria associato ([fare clic per visualizzare l'immagine con dimensioni complete](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come eseguire una query e visualizzare i dati usando il controllo SqlDataSource. Come ObjectDataSource, il SqlDataSource funge da proxy, fornendo un approccio dichiarativo per accedere ai dati. Le proprietà specificano il database a cui connettersi e la query SQL `SELECT` per l'esecuzione. possono essere specificati tramite il Finestra Proprietà o tramite la procedura guidata Configura origine dati.

Gli esempi di query `SELECT` esaminati in questa esercitazione hanno restituito tutti i record dalla query specificata. Il controllo SqlDataSource, tuttavia, può includere una clausola `WHERE` con parametri i cui valori vengono assegnati a livello di codice o estratti automaticamente da un'origine specificata. Si esaminerà come creare e usare query con parametri nell'esercitazione successiva.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Accesso ai dati di database relazionali](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Cenni preliminari sul controllo SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Esercitazione introduttiva di ASP.NET: controllo SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Elemento `<connectionStrings>` Web. config](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Riferimento alla stringa di connessione del database](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Susan Connery, Bernadette Leigh e David Suru. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [Successivo](using-parameterized-queries-with-the-sqldatasource-vb.md)
