---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementazione della concorrenza ottimistica con SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione vengono esaminati gli elementi di base del controllo della concorrenza ottimistica e viene quindi illustrato come implementarlo tramite il controllo SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 431734b5245c20ac840147cf0827fa7f8d1e4d17
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598052"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementazione della concorrenza ottimistica con SqlDataSource (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) o [scaricare il file PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> In questa esercitazione vengono esaminati gli elementi di base del controllo della concorrenza ottimistica e viene quindi illustrato come implementarlo tramite il controllo SqlDataSource.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato esaminato come aggiungere le funzionalità di inserimento, aggiornamento ed eliminazione al controllo SqlDataSource. In breve, per fornire queste funzionalità è necessario specificare l'istruzione SQL `INSERT`, `UPDATE`o `DELETE` corrispondente nelle proprietà `InsertCommand`, `UpdateCommand`o `DeleteCommand` del controllo, insieme ai parametri appropriati nelle raccolte `InsertParameters`, `UpdateParameters`e `DeleteParameters`. Anche se queste proprietà e raccolte possono essere specificate manualmente, il pulsante Avanzate della procedura guidata Configura origine dati offre una casella di controllo genera `INSERT`, `UPDATE`e `DELETE` che creerà automaticamente tali istruzioni in base all'istruzione `SELECT`.

Insieme alla casella di controllo genera `INSERT`, `UPDATE`e `DELETE`, la finestra di dialogo Opzioni di generazione SQL avanzate include l'opzione Usa concorrenza ottimistica (vedere la figura 1). Se questa opzione è selezionata, le clausole `WHERE` nelle istruzioni `UPDATE` e `DELETE` generate automaticamente vengono modificate in modo da eseguire l'aggiornamento o l'eliminazione solo se i dati del database sottostanti non sono stati modificati dopo l'ultima volta che l'utente ha caricato i dati nella griglia.

![È possibile aggiungere il supporto della concorrenza ottimistica dalla finestra di dialogo Opzioni avanzate di generazione SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figura 1**: è possibile aggiungere il supporto della concorrenza ottimistica dalla finestra di dialogo Opzioni avanzate di generazione SQL

Nell'esercitazione sull' [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) sono state esaminate le nozioni di base del controllo della concorrenza ottimistica e viene illustrato come aggiungerlo a ObjectDataSource. In questa esercitazione verranno ritoccate le nozioni di base del controllo della concorrenza ottimistica e quindi verrà illustrato come implementarlo tramite SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Riepilogo della concorrenza ottimistica

Per le applicazioni Web che consentono a più utenti simultanei di modificare o eliminare gli stessi dati, esiste la possibilità che un utente possa sovrascrivere accidentalmente le modifiche apportate da un altro utente. Nell'esercitazione sull' [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) è stato fornito l'esempio seguente:

Si supponga che due utenti, Jisun e Sam, stiano visitando una pagina di un'applicazione che consentiva ai visitatori di aggiornare ed eliminare prodotti tramite un controllo GridView. Entrambi fanno clic sul pulsante modifica per Chai intorno allo stesso tempo. Jisun modifica il nome del prodotto in Chai Tea e fa clic sul pulsante Aggiorna. Il risultato finale è un'istruzione `UPDATE` inviata al database, che imposta *tutti* i campi aggiornabili del prodotto (anche se Jisun ha aggiornato solo un campo, `ProductName`). A questo punto, nel database sono presenti i valori Chai Tea, Category Beverage, Suppliers Exotic Liquids e così via per questo prodotto specifico. Tuttavia, la schermata GridView in Sam s Mostra ancora il nome del prodotto nella riga di GridView modificabile come Chai. Pochi secondi dopo il commit delle modifiche apportate a Jisun, Sam aggiorna la categoria ai condimenti e fa clic su Aggiorna. In questo modo si ottiene un'istruzione `UPDATE` inviata al database che imposta il nome del prodotto su Chai, il `CategoryID` all'ID della categoria dei condimenti corrispondente e così via. Jisun modifiche apportate al nome del prodotto sono state sovrascritte.

Nella figura 2 viene illustrata questa interazione.

[![quando due utenti aggiornano contemporaneamente un record, è possibile che un utente modifichi le modifiche per sovrascrivere le altre](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figura 2**: quando due utenti aggiornano contemporaneamente un record, è possibile che un utente modifichi le modifiche per sovrascrivere gli altri ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))

Per evitare che questo scenario venga aperto, è necessario implementare un form di [controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) . La [concorrenza ottimistica](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) in questa esercitazione si basa sul presupposto che anche se possono verificarsi conflitti di concorrenza ogni ora e quindi, la maggior parte del tempo di tali conflitti non si verifica. Se pertanto si verifica un conflitto, il controllo della concorrenza ottimistica informa semplicemente l'utente che è possibile salvare le modifiche perché un altro utente ha modificato gli stessi dati.

> [!NOTE]
> Per le applicazioni in cui si presuppone che vi siano molti conflitti di concorrenza o se tali conflitti non sono tollerabili, è possibile utilizzare il controllo della concorrenza pessimistica. Per una discussione più approfondita sul controllo della concorrenza pessimistica, vedere l'esercitazione sull' [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) .

Il controllo della concorrenza ottimistica funziona assicurandosi che il record aggiornato o eliminato abbia gli stessi valori di quando è stato avviato il processo di aggiornamento o eliminazione. Ad esempio, quando si fa clic sul pulsante modifica in un GridView modificabile, i valori dei record vengono letti dal database e visualizzati in caselle di testo e altri controlli Web. Questi valori originali vengono salvati dal controllo GridView. Successivamente, dopo che l'utente ha apportato le modifiche e fa clic sul pulsante Aggiorna, l'istruzione `UPDATE` utilizzata deve prendere in considerazione i valori originali più i nuovi valori e aggiornare solo il record del database sottostante se i valori originali che l'utente ha iniziato a modificare sono identici ai valori ancora presenti nel database. Nella figura 3 viene illustrata questa sequenza di eventi.

[![per la riuscita dell'operazione di aggiornamento o eliminazione, i valori originali devono essere uguali ai valori del database corrente](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: affinché l'aggiornamento o l'eliminazione abbia esito positivo, i valori originali devono essere uguali ai valori di database correnti ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))

Esistono diversi approcci per l'implementazione della concorrenza ottimistica (vedere la logica di aggiornamento della [concorrenza ottimistica](http://www.eggheadcafe.com/articles/20050719.asp) di [Peter a. Bromberg](http://peterbromberg.net/)per una breve panoramica di alcune opzioni). La tecnica utilizzata dal SqlDataSource (e dai set di dati tipizzati ADO.NET utilizzati nel livello di accesso ai dati) aumenta la clausola `WHERE` per includere un confronto di tutti i valori originali. L'istruzione `UPDATE` seguente, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se i valori correnti del database sono uguali ai valori recuperati in origine durante l'aggiornamento del record in GridView. I parametri `@ProductName` e `@UnitPrice` contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori caricati originariamente in GridView quando è stato fatto clic sul pulsante modifica:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Come si vedrà in questa esercitazione, l'abilitazione del controllo della concorrenza ottimistica con il SqlDataSource è semplice come il controllo di una casella di controllo.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Passaggio 1: creazione di un SqlDataSource che supporta la concorrenza ottimistica

Per iniziare, aprire la pagina `OptimisticConcurrency.aspx` dalla cartella `SqlDataSource`. Trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione, impostarne la proprietà `ID` su `ProductsDataSourceWithOptimisticConcurrency`. Quindi, fare clic sul collegamento Configura origine dati dallo smart tag del controllo. Dalla prima schermata della procedura guidata, scegliere di utilizzare il `NORTHWINDConnectionString` e fare clic su Avanti.

[![scegliere di usare NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: scegliere di utilizzare il `NORTHWINDConnectionString` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))

Per questo esempio verrà aggiunto un controllo GridView che consente agli utenti di modificare la tabella `Products`. Quindi, dalla schermata Configura istruzione SELECT scegliere la tabella `Products` dall'elenco a discesa e selezionare le colonne `ProductID`, `ProductName`, `UnitPrice`e `Discontinued`, come illustrato nella figura 5.

[![dalla tabella Products, restituire le colonne ProductID, ProductName, PrezzoUnitario e Discontinued](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figura 5**: dalla tabella `Products`, restituire le colonne `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))

Al termine della selezione delle colonne, fare clic sul pulsante avanzate per visualizzare la finestra di dialogo Opzioni avanzate di generazione SQL. Controllare le istruzioni genera `INSERT`, `UPDATE`e `DELETE` e usare le caselle di controllo della concorrenza ottimistica e fare clic su OK. per una schermata, vedere nuovamente la figura 1. Completare la procedura guidata facendo clic su Avanti, quindi su fine.

Dopo aver completato la configurazione guidata origine dati, esaminare le proprietà `DeleteCommand` e `UpdateCommand` risultanti e le raccolte di `DeleteParameters` e `UpdateParameters`. Il modo più semplice per eseguire questa operazione consiste nel fare clic sulla scheda origine nell'angolo in basso a sinistra per visualizzare la sintassi dichiarativa della pagina. Qui sarà disponibile un valore `UpdateCommand`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Con sette parametri nella raccolta `UpdateParameters`:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Analogamente, la proprietà `DeleteCommand` e la raccolta `DeleteParameters` dovrebbero avere un aspetto simile al seguente:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Oltre ad aumentare le clausole `WHERE` delle proprietà `UpdateCommand` e `DeleteCommand` (e aggiungere i parametri aggiuntivi alle rispettive raccolte di parametri), la selezione dell'opzione Usa concorrenza ottimistica regola altre due proprietà:

- Modifica la [proprietà`ConflictDetection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) da `OverwriteChanges` (impostazione predefinita) a `CompareAllValues`
- Modifica la [proprietà`OldValuesParameterFormatString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) da {0} (impostazione predefinita) a\_originale {0}.

Quando il controllo Web dati richiama il Metodo SqlDataSource s `Update()` o `Delete()`, vengono passati i valori originali. Se la proprietà `ConflictDetection` SqlDataSource s è impostata su `CompareAllValues`, i valori originali vengono aggiunti al comando. La proprietà `OldValuesParameterFormatString` fornisce il modello di denominazione utilizzato per questi parametri di valore originale. La configurazione guidata origine dati utilizza {0} di\_originali e assegna un nome a ogni parametro originale nelle proprietà `UpdateCommand` e `DeleteCommand` e `UpdateParameters` e `DeleteParameters` di conseguenza.

> [!NOTE]
> Poiché non si utilizzano le funzionalità di inserimento del controllo SqlDataSource, è possibile rimuovere la proprietà `InsertCommand` e la relativa raccolta di `InsertParameters`.

## <a name="correctly-handlingnullvalues"></a>Gestione corretta dei valori di`NULL`

Sfortunatamente, le istruzioni `UPDATE` e `DELETE` aumentata generate automaticamente dalla configurazione guidata origine dati *quando si utilizza* la concorrenza ottimistica non funzionano con i record che contengono valori di `NULL`. Per capire perché, prendere in considerazione il `UpdateCommand`di SqlDataSource:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

La colonna `UnitPrice` della tabella `Products` può includere valori di `NULL`. Se un record specifico ha un valore `NULL` per `UnitPrice`, la parte della clausola `WHERE` `[UnitPrice] = @original_UnitPrice` restituirà *sempre* false perché `NULL = NULL` restituisce sempre false. Di conseguenza, i record che contengono `NULL` valori non possono essere modificati o eliminati perché le istruzioni `UPDATE` e `DELETE` `WHERE` le clausole non restituiranno alcuna riga da aggiornare o eliminare.

> [!NOTE]
> Questo bug è stato segnalato per la prima volta a Microsoft nel giugno 2004 in [SqlDataSource genera istruzioni SQL non corrette](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e viene pianificato per essere risolto nella versione successiva di ASP.NET.

Per risolvere questo problema, è necessario aggiornare manualmente le clausole `WHERE` nelle proprietà `UpdateCommand` e `DeleteCommand` per **tutte** le colonne che possono avere valori `NULL`. In generale, modificare `[ColumnName] = @original_ColumnName` in:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Questa modifica può essere eseguita direttamente tramite il markup dichiarativo, tramite le opzioni UpdateQuery o DeleteQuery della Finestra Proprietà o tramite le schede Aggiorna ed Elimina nell'opzione specificare un'istruzione SQL personalizzata o stored procedure nella pagina Configura dati Creazione guidata origine. Anche in questo caso, è necessario apportare questa modifica per *ogni* colonna della clausola `UpdateCommand` e `DeleteCommand` s `WHERE` che può contenere valori `NULL`.

Applicando questo risultato all'esempio, i valori `UpdateCommand` e `DeleteCommand` modificati seguenti sono i seguenti:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Passaggio 2: aggiunta di un controllo GridView con le opzioni di modifica ed eliminazione

Con il SqlDataSource configurato per supportare la concorrenza ottimistica, rimane solo l'aggiunta di un controllo Web dati alla pagina che utilizza questo controllo della concorrenza. Per questa esercitazione, verrà aggiunto un controllo GridView che fornisce funzionalità di modifica ed eliminazione. A tale scopo, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione e impostare la relativa `ID` su `Products`. Dallo smart tag GridView s, associarlo al controllo `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource aggiunto nel passaggio 1. Infine, selezionare l'opzione Abilita modifica e Abilita eliminazione dallo smart tag.

[![associare GridView al SqlDataSource e abilitare la modifica e l'eliminazione](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Figura 6**: associare GridView al SqlDataSource e abilitare la modifica e l'eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))

Dopo aver aggiunto GridView, configurarne l'aspetto rimuovendo il `ProductID` BoundField, modificando la proprietà `HeaderText` di `ProductName` BoundField in Product e aggiornando il BoundField `UnitPrice` in modo che la relativa proprietà `HeaderText` sia semplicemente price. Idealmente, è possibile migliorare l'interfaccia di modifica per includere un RequiredFieldValidator per il valore `ProductName` e un CompareValidator per il valore di `UnitPrice` (per assicurarsi che sia un valore numerico formattato correttamente). Per un'analisi più approfondita della personalizzazione dell'interfaccia di modifica di GridView, vedere l'esercitazione [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

> [!NOTE]
> È necessario abilitare lo stato di visualizzazione di GridView perché i valori originali passati da GridView al SqlDataSource vengono archiviati in stato di visualizzazione.

Dopo avere apportato queste modifiche a GridView, il markup dichiarativo GridView e SqlDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Per visualizzare il controllo della concorrenza ottimistica in azione, aprire due finestre del browser e caricare la pagina `OptimisticConcurrency.aspx` in entrambi. Fare clic sui pulsanti Edit (modifica) per il primo prodotto in entrambi i browser. In un browser modificare il nome del prodotto e fare clic su Aggiorna. Il browser effettuerà il postback e GridView tornerà alla modalità di modifica precedente, mostrando il nuovo nome del prodotto per il record appena modificato.

Nella seconda finestra del browser modificare il prezzo (lasciando il nome del prodotto come valore originale) e fare clic su Aggiorna. Durante il postback, la griglia torna alla modalità di modifica precedente, ma la modifica al prezzo non viene registrata. Il secondo browser Mostra lo stesso valore del primo nome del nuovo prodotto con il prezzo precedente. Le modifiche apportate nella seconda finestra del browser sono andate perse. Inoltre, le modifiche sono andate perse piuttosto tranquillamente, perché non era presente un'eccezione o un messaggio indicante che si è verificata una violazione della concorrenza.

[![le modifiche apportate alla seconda finestra del browser sono state perse automaticamente](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figura 7**: le modifiche apportate alla seconda finestra del browser sono state perse automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))

Il motivo per cui non è stato eseguito il commit delle modifiche apportate al secondo browser è dovuto al fatto che la clausola `UPDATE` Statement s `WHERE` ha escluso tutti i record e pertanto non ha interessato alcuna riga. Esaminiamo di nuovo l'istruzione `UPDATE`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Quando la seconda finestra del browser aggiorna il record, il nome prodotto originale specificato nella clausola `WHERE` non corrisponde al nome del prodotto esistente (poiché è stato modificato dal primo browser). Di conseguenza, l'istruzione `[ProductName] = @original_ProductName` restituisce false e il `UPDATE` non influisce su alcun record.

> [!NOTE]
> L'eliminazione funziona allo stesso modo. Con due finestre del browser aperte, iniziare modificando un determinato prodotto con uno e quindi salvando le modifiche. Dopo aver salvato le modifiche in un unico browser, fare clic sul pulsante Elimina per lo stesso prodotto nell'altro. Poiché i valori originali non corrispondono nella clausola `DELETE` Statement s `WHERE`, l'eliminazione non riesce automaticamente.

Dal punto di vista dell'utente finale nella seconda finestra del browser, dopo aver fatto clic sul pulsante Aggiorna la griglia torna alla modalità di modifica preliminare, ma le modifiche sono andate perse. Tuttavia, non c'è alcun feedback visivo che le modifiche non sono state apportate. Idealmente, se le modifiche apportate a un utente vengono perse in caso di violazione della concorrenza, verranno inviate notifiche e, forse, la griglia verrà mantenuta in modalità di modifica. Si osservi come eseguire questa operazione.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Passaggio 3: determinazione del momento in cui si è verificata una violazione di concorrenza

Poiché una violazione della concorrenza rifiuta le modifiche apportate, è utile avvisare l'utente quando si è verificata una violazione della concorrenza. Per avvisare l'utente, aggiungere un controllo Web Label alla parte superiore della pagina denominata `ConcurrencyViolationMessage` la cui proprietà `Text` Visualizza il messaggio seguente: si è tentato di aggiornare o eliminare un record che è stato aggiornato contemporaneamente da un altro utente. Verificare le modifiche apportate dall'altro utente e quindi ripetere l'aggiornamento o l'eliminazione. Impostare il controllo Label s `CssClass` proprietà su Warning, ovvero una classe CSS definita in `Styles.css` che Visualizza il testo in un tipo di carattere rosso, corsivo, grassetto e grande. Infine, impostare l'etichetta s `Visible` e `EnableViewState` proprietà su `False`. In questo modo, l'etichetta viene nascosta solo per i postback in cui la proprietà `Visible` viene impostata in modo esplicito su `True`.

[![aggiungere un controllo etichetta alla pagina per visualizzare l'avviso](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figura 8**: aggiungere un controllo etichetta alla pagina per visualizzare l'avviso ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))

Quando si esegue un aggiornamento o un'eliminazione, i gestori di eventi `RowUpdated` e `RowDeleted` di GridView vengono attivati dopo che il controllo origine dati ha eseguito l'aggiornamento o l'eliminazione richiesta. È possibile determinare il numero di righe interessate dall'operazione da questi gestori eventi. Se sono state interessate zero righe, si desidera visualizzare l'etichetta `ConcurrencyViolationMessage`.

Creare un gestore eventi per gli eventi `RowUpdated` e `RowDeleted` e aggiungere il codice seguente:

[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

In entrambi i gestori eventi si controlla la proprietà `e.AffectedRows` e, se è uguale a 0, impostare la proprietà `Visible` `ConcurrencyViolationMessage` Label su `True`. Nel gestore dell'evento `RowUpdated` viene inoltre indicato a GridView di rimanere in modalità di modifica impostando la proprietà `KeepInEditMode` su true. In questo modo, è necessario riassociare i dati alla griglia in modo che i dati degli altri utenti vengano caricati nell'interfaccia di modifica. Questa operazione viene eseguita chiamando il Metodo GridView s `DataBind()`.

Come illustrato nella figura 9, con questi due gestori eventi, viene visualizzato un messaggio molto evidente quando si verifica una violazione della concorrenza.

[![viene visualizzato un messaggio in caso di violazione della concorrenza](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figura 9**: viene visualizzato un messaggio in caso di violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni complete](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))

## <a name="summary"></a>Riepilogo

Quando si crea un'applicazione Web in cui più utenti simultanei possono modificare gli stessi dati, è importante prendere in considerazione le opzioni di controllo della concorrenza. Per impostazione predefinita, i controlli Web e i controlli origine dati di ASP.NET non utilizzano alcun controllo della concorrenza. Come illustrato in questa esercitazione, l'implementazione del controllo della concorrenza ottimistica con SqlDataSource è relativamente rapida e semplice. Il SqlDataSource gestisce la maggior parte delle operazioni preliminari per l'aggiunta delle clausole di `WHERE` aumentata alle istruzioni `UPDATE` e `DELETE` generate automaticamente, ma sono disponibili alcune sottigliezze per la gestione delle colonne di valori di `NULL`, come illustrato nella sezione corretta gestione dei valori di `NULL`.

Questa esercitazione conclude l'esame del SqlDataSource. Le esercitazioni rimanenti torneranno a lavorare con i dati usando ObjectDataSource e l'architettura a livelli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
