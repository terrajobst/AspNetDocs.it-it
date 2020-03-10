---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aggiornamento ed eliminazione di dati binari esistenti (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato illustrato il modo in cui il controllo GridView semplifica la modifica e l'eliminazione dei dati di testo. In questa esercitazione viene illustrato il modo in cui il controllo GridView crea anche...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 27ff6941008b4e7bf6d632e4c248fd1d35fb3589
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587668"
---
# <a name="updating-and-deleting-existing-binary-data-vb"></a>Aggiornamento ed eliminazione di dati binari esistenti (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) o [scaricare il file PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> Nelle esercitazioni precedenti è stato illustrato il modo in cui il controllo GridView semplifica la modifica e l'eliminazione dei dati di testo. In questa esercitazione viene illustrato il modo in cui il controllo GridView consente inoltre di modificare ed eliminare i dati binari, indipendentemente dal fatto che siano salvati nel database o archiviati nel file system.

## <a name="introduction"></a>Introduzione

Nelle ultime tre esercitazioni sono state aggiunte alcune funzionalità per l'utilizzo di dati binari. È stata avviata aggiungendo una colonna `BrochurePath` alla tabella `Categories` e l'architettura è stata aggiornata di conseguenza. Sono stati anche aggiunti i metodi livello di accesso ai dati e livello di logica di business per lavorare con la colonna della tabella delle categorie s `Picture` esistente, che include il contenuto binario di un file di immagine. Sono state create pagine Web per presentare i dati binari in un collegamento GridView a download per la brochure, con l'immagine della categoria mostrata in un elemento `<img>` e è stato aggiunto un DetailsView per consentire agli utenti di aggiungere una nuova categoria e caricare la relativa brochure e i dati immagine.

Tutto ciò che rimane da implementare è la possibilità di modificare ed eliminare le categorie esistenti, che verranno eseguite in questa esercitazione usando le funzionalità di modifica e eliminazione predefinite di GridView. Quando si modifica una categoria, l'utente potrà caricare facoltativamente una nuova immagine o fare in modo che la categoria continui a usare quella esistente. Per la brochure, è possibile scegliere di usare la brochure esistente, caricare una nuova brochure o indicare che alla categoria non è più associata una brochure. Inizia subito.

## <a name="step-1-updating-the-data-access-layer"></a>Passaggio 1: aggiornamento del livello di accesso ai dati

Il DAL ha generato automaticamente i metodi `Insert`, `Update`e `Delete`, ma questi metodi sono stati generati in base alla query principale `CategoriesTableAdapter` s, che non include la colonna `Picture`. Pertanto, i metodi `Insert` e `Update` non includono parametri per specificare i dati binari per l'immagine di categoria. Come nell' [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md), è necessario creare un nuovo metodo TableAdapter per aggiornare la tabella `Categories` quando si specificano dati binari.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic con il pulsante destro del mouse sull'intestazione `CategoriesTableAdapter` s e scegliere Aggiungi query dal menu di scelta rapida per avviare la configurazione guidata query TableAdapter. Questa procedura guidata inizia chiedendo come la query TableAdapter deve accedere al database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo richiede la generazione del tipo di query. Poiché viene ricreata una query per aggiungere un nuovo record alla tabella `Categories`, scegliere Aggiorna, quindi fare clic su Avanti.

[![selezionare l'opzione di aggiornamento](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Figura 1**: selezionare l'opzione di aggiornamento ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image3.png))

A questo punto è necessario specificare l'istruzione SQL `UPDATE`. La procedura guidata suggerisce automaticamente un'istruzione `UPDATE` corrispondente alla query principale di TableAdapter (uno che aggiorna i valori `CategoryName`, `Description`e `BrochurePath`). Modificare l'istruzione in modo che la colonna `Picture` venga inclusa insieme a un parametro di `@Picture`, come indicato di seguito:

[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Nella schermata finale della procedura guidata viene chiesto di assegnare un nome al nuovo metodo TableAdapter. Immettere `UpdateWithPicture` e fare clic su fine.

[![nome del nuovo metodo TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Figura 2**: assegnare un nome al nuovo metodo TableAdapter `UpdateWithPicture` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image6.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Passaggio 2: aggiunta dei metodi del livello di logica di business

Oltre ad aggiornare il DAL, è necessario aggiornare il livello BLL per includere i metodi per l'aggiornamento e l'eliminazione di una categoria. Questi sono i metodi che verranno richiamati dal livello di presentazione.

Per eliminare una categoria, è possibile usare il metodo di `Delete` generato automaticamente `CategoriesTableAdapter` s. Aggiungere il metodo seguente alla classe `CategoriesBLL`:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Per questa esercitazione, è possibile creare due metodi per l'aggiornamento di una categoria che prevede i dati dell'immagine binaria e richiama il metodo di `UpdateWithPicture` appena aggiunto al `CategoriesTableAdapter` e un altro che accetta solo i valori `CategoryName`, `Description`e `BrochurePath` e USA `CategoriesTableAdapter` istruzione `Update` della classe generata automaticamente. La logica alla base dell'utilizzo di due metodi è che, in alcune circostanze, un utente potrebbe voler aggiornare l'immagine della categoria insieme agli altri campi, nel qual caso l'utente dovrà caricare la nuova immagine. È quindi possibile usare i dati binari dell'immagine caricata nell'istruzione `UPDATE`. In altri casi, è possibile che l'utente sia interessato solo all'aggiornamento, ad Say, al nome e alla descrizione. Tuttavia, se l'istruzione `UPDATE` prevede anche i dati binari per la colonna `Picture`, è necessario fornire anche tali informazioni. Questa operazione richiederebbe un ulteriore viaggio al database per ripristinare i dati dell'immagine per il record in corso di modifica. Pertanto, si desiderano due metodi `UPDATE`. Il livello della logica di business determinerà quello da utilizzare a seconda che i dati dell'immagine vengano forniti quando si aggiorna la categoria.

Per semplificare questa operazione, aggiungere due metodi alla classe `CategoriesBLL`, entrambi denominati `UpdateCategory`. Il primo deve accettare tre `String` s, una matrice di `Byte` e un `Integer` come parametri di input; il secondo, solo tre `String` s e una `Integer`. I parametri di input `String` sono per il nome, la descrizione e il percorso del file della brochure della categoria, la matrice di `Byte` è per il contenuto binario dell'immagine della categoria e il `Integer` identifica il `CategoryID` del record da aggiornare. Si noti che il primo overload richiama il secondo se la matrice `Byte` passata è `Nothing`:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Passaggio 3: copia sulle funzionalità di inserimento e visualizzazione

Nell' [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md) è stata creata una pagina denominata `UploadInDetailsView.aspx` in cui sono elencate tutte le categorie di un controllo GridView e viene fornito un oggetto DetailsView per aggiungere nuove categorie al sistema. In questa esercitazione si estenderà GridView per includere la modifica e l'eliminazione del supporto. Anziché continuare a lavorare da `UploadInDetailsView.aspx`, è possibile inserire le modifiche dell'esercitazione nella pagina `UpdatingAndDeleting.aspx` dalla stessa cartella `~/BinaryData`. Copiare e incollare il markup dichiarativo e il codice da `UploadInDetailsView.aspx` a `UpdatingAndDeleting.aspx`.

Per iniziare, aprire la pagina `UploadInDetailsView.aspx`. Copiare tutta la sintassi dichiarativa all'interno dell'elemento `<asp:Content>`, come illustrato nella figura 3. Successivamente, aprire `UpdatingAndDeleting.aspx` e incollare il markup all'interno dell'elemento `<asp:Content>`. Analogamente, copiare il codice dalla classe code-behind della pagina `UploadInDetailsView.aspx` in `UpdatingAndDeleting.aspx`.

[![copiare il markup dichiarativo da UploadInDetailsView. aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Figura 3**: copiare il markup dichiarativo da `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image9.png))

Dopo aver copiato il markup dichiarativo e il codice, visitare `UpdatingAndDeleting.aspx`. Si dovrebbe visualizzare lo stesso output e avere la stessa esperienza utente di `UploadInDetailsView.aspx` pagina dell'esercitazione precedente.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Passaggio 4: aggiunta del supporto per l'eliminazione di ObjectDataSource e GridView

Come è stato illustrato nella [Panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , GridView fornisce funzionalità di eliminazione predefinite e queste funzionalità possono essere abilitate quando si esegue il segno di spunta di una casella di controllo se l'origine dati sottostante della griglia supporta l'eliminazione. Attualmente l'oggetto ObjectDataSource a cui è associato GridView (`CategoriesDataSource`) non supporta l'eliminazione.

Per risolvere il problema, fare clic sull'opzione Configura origine dati dallo smart tag di ObjectDataSource per avviare la procedura guidata. La prima schermata mostra che ObjectDataSource è configurato per funzionare con la classe `CategoriesBLL`. Fare clic su Avanti. Attualmente sono specificate solo le proprietà `InsertMethod` e `SelectMethod` di ObjectDataSource. Tuttavia, la procedura guidata ha automaticamente popolato gli elenchi a discesa nelle schede Aggiorna ed Elimina con i metodi `UpdateCategory` e `DeleteCategory`, rispettivamente. Questo è dovuto al fatto che nella classe `CategoriesBLL` abbiamo contrassegnato questi metodi usando il `DataObjectMethodAttribute` come metodi predefiniti per l'aggiornamento e l'eliminazione.

Per il momento, impostare l'elenco a discesa scheda aggiornamento su (nessuno), ma lasciare l'elenco a discesa Elimina scheda s impostato su `DeleteCategory`. Questa procedura guidata verrà visualizzata nel passaggio 6 per aggiungere il supporto per l'aggiornamento.

[![configurare ObjectDataSource per l'utilizzo del metodo DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per l'uso del metodo `DeleteCategory` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image12.png))

> [!NOTE]
> Al termine della procedura guidata, è possibile che in Visual Studio venga chiesto se si desidera aggiornare i campi e le chiavi, in modo da rigenerare i campi dei controlli Web. Scegliere No, perché se si sceglie Sì, eventuali personalizzazioni dei campi che potrebbero essere state apportate vengono sovrascritte.

In ObjectDataSource verrà ora incluso un valore per la proprietà `DeleteMethod`, oltre a una `DeleteParameter`. Tenere presente che quando si usa la procedura guidata per specificare i metodi, Visual Studio imposta la proprietà `OldValuesParameterFormatString` di ObjectDataSource su `original_{0}`, che causa problemi con le chiamate al metodo Update e DELETE. Quindi, cancellare completamente questa proprietà o reimpostarla sul valore predefinito `{0}`. Se è necessario aggiornare la memoria in questa proprietà ObjectDataSource, vedere l'esercitazione [Panoramica sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Dopo aver completato la procedura guidata e aver corretto la `OldValuesParameterFormatString`, il markup dichiarativo di ObjectDataSource sarà simile al seguente:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Dopo aver configurato ObjectDataSource, aggiungere le funzionalità di eliminazione a GridView selezionando la casella di controllo Abilita eliminazione dallo smart tag di GridView. Verrà aggiunto un oggetto CommandField al controllo GridView la cui proprietà `ShowDeleteButton` è impostata su `True`.

[![Abilita il supporto per l'eliminazione in GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Figura 5**: abilitare il supporto per l'eliminazione in GridView ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image15.png))

Esaminare la funzionalità di eliminazione. Esiste una chiave esterna tra la tabella `Products` s `CategoryID` e la `CategoryID`della tabella `Categories`, quindi si otterrà un'eccezione di violazione di vincolo di chiave esterna se si tenta di eliminare le prime otto categorie. Per testare questa funzionalità, aggiungere una nuova categoria, fornendo una brochure e un'immagine. La categoria di test, illustrata nella figura 6, include un file della brochure di test denominato `Test.pdf` e un'immagine di test. Nella figura 7 viene illustrato GridView dopo che è stata aggiunta la categoria di test.

[![aggiungere una categoria di test con una brochure e un'immagine](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Figura 6**: aggiungere una categoria di test con una brochure e un'immagine ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image18.png))

[![dopo aver inserito la categoria di test, questa viene visualizzata in GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Figura 7**: dopo aver inserito la categoria di test, questa viene visualizzata nel GridView ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image21.png))

In Visual Studio aggiornare il Esplora soluzioni. A questo punto dovrebbe essere visualizzato un nuovo file nella cartella `~/Brochures` `Test.pdf` (vedere la figura 8).

Quindi, fare clic sul collegamento Elimina nella riga categoria di test, causando il postback della pagina e il metodo di `CategoriesBLL` classe `DeleteCategory` da attivare. Verrà richiamato il metodo DAL `Delete`, causando l'invio dell'istruzione `DELETE` appropriata al database. I dati vengono quindi riassociati al GridView e il markup viene restituito al client con la categoria di test non più presente.

Mentre il flusso di lavoro DELETE ha rimosso correttamente il record della categoria di test dalla tabella `Categories`, non ha rimosso il file della brochure dal file system del server Web. Aggiornare il Esplora soluzioni e si noterà che `Test.pdf` si trova ancora nella cartella `~/Brochures`.

![Il file test. pdf non è stato eliminato dal file System del server Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Figura 8**: il file di `Test.pdf` non è stato eliminato dal file System del server Web

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Passaggio 5: rimozione del file della brochure della categoria eliminato

Uno degli svantaggi dell'archiviazione dei dati binari esterni al database consiste nel fatto che è necessario eseguire passaggi aggiuntivi per pulire questi file quando il record del database associato viene eliminato. GridView e ObjectDataSource forniscono eventi che vengono generati sia prima che dopo l'esecuzione del comando Delete. In realtà è necessario creare gestori eventi per gli eventi pre-e post-azione. Prima che il record `Categories` venga eliminato, è necessario determinare il percorso del file PDF, ma non si vuole eliminare il file PDF prima che la categoria venga eliminata in caso di eccezione e la categoria non viene eliminata.

L'evento GridView s [`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) viene attivato prima che venga richiamato il comando di eliminazione di ObjectDataSource, mentre il relativo [evento`RowDeleted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) generato dopo. Creare gestori eventi per questi due eventi usando il codice seguente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

Nel gestore dell'evento `RowDeleting` il `CategoryID` della riga da eliminare viene preso dalla raccolta di `DataKeys` di GridView, a cui è possibile accedere in questo gestore eventi tramite la raccolta `e.Keys`. Viene quindi richiamato il `GetCategoryByCategoryID(categoryID)` della classe `CategoriesBLL` per restituire informazioni sul record da eliminare. Se l'oggetto `CategoriesDataRow` restituito ha un valore non`NULL``BrochurePath`, viene archiviato nella variabile di pagina `deletedCategorysPdfPath` in modo che il file possa essere eliminato nel gestore eventi `RowDeleted`.

> [!NOTE]
> Anziché recuperare i dettagli di `BrochurePath` per il record `Categories` da eliminare nel gestore dell'evento `RowDeleting`, è possibile che sia stata aggiunta la `BrochurePath` alla proprietà GridView s `DataKeyNames` e che sia stato eseguito l'accesso al valore del record tramite la raccolta di `e.Keys`. In questo modo si aumentano leggermente le dimensioni dello stato di visualizzazione di GridView, ma si riduce la quantità di codice necessario e si salva una corsa nel database.

Dopo che è stato richiamato il comando di eliminazione sottostante di ObjectDataSource, il gestore dell'evento GridView s `RowDeleted` viene attivato. Se non si sono verificate eccezioni durante l'eliminazione dei dati ed è presente un valore per `deletedCategorysPdfPath`, il file PDF viene eliminato dall'file system. Si noti che questo codice aggiuntivo non è necessario per pulire i dati binari della categoria associati alla relativa immagine. Poiché i dati dell'immagine vengono archiviati direttamente nel database, eliminando la `Categories` riga vengono eliminati anche i dati dell'immagine della categoria.

Dopo aver aggiunto i due gestori eventi, eseguire nuovamente questo test case. Quando si elimina la categoria, viene eliminato anche il file PDF associato.

L'aggiornamento di un record esistente associato ai dati binari fornisce alcune interessanti problemi. Il resto di questa esercitazione approfondisce l'aggiunta di funzionalità di aggiornamento alla brochure e all'immagine. Nel passaggio 6 vengono esaminate le tecniche per aggiornare le informazioni della brochure mentre il passaggio 7 esamina l'aggiornamento dell'immagine.

## <a name="step-6-updating-a-category-s-brochure"></a>Passaggio 6: aggiornamento della brochure di una categoria

Come illustrato in un'esercitazione introduttiva sull'inserimento, l'aggiornamento e l'eliminazione dei dati, GridView offre un supporto predefinito per la modifica a livello di riga che può essere implementato tramite il segno di spunta di una casella di controllo se l'origine dati sottostante è configurata in modo appropriato. Attualmente, il `CategoriesDataSource` ObjectDataSource non è ancora configurato per includere il supporto per l'aggiornamento, quindi è possibile aggiungerlo in.

Fare clic sul collegamento Configura origine dati dalla procedura guidata di ObjectDataSource e procedere con il secondo passaggio. A causa della `DataObjectMethodAttribute` utilizzata in `CategoriesBLL`, l'elenco a discesa Aggiorna deve essere popolato automaticamente con l'overload `UpdateCategory` che accetta quattro parametri di input (per tutte le colonne, ma `Picture`). Modificare questa operazione in modo che usi l'overload con cinque parametri.

[![configurare ObjectDataSource per l'utilizzo del metodo UpdateCategory che include un parametro per l'immagine](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Figura 9**: configurare ObjectDataSource per l'uso del metodo `UpdateCategory` che include un parametro per `Picture` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image24.png))

In ObjectDataSource verrà ora incluso un valore per la proprietà `UpdateMethod`, oltre ai `UpdateParameter` s corrispondenti. Come indicato nel passaggio 4, Visual Studio imposta la proprietà `OldValuesParameterFormatString` di ObjectDataSource su `original_{0}` quando si utilizza la configurazione guidata origine dati. Ciò causerà problemi con le chiamate al metodo Update e DELETE. Quindi, cancellare completamente questa proprietà o reimpostarla sul valore predefinito `{0}`.

Dopo aver completato la procedura guidata e aver corretto la `OldValuesParameterFormatString`, il markup dichiarativo di ObjectDataSource sarà simile al seguente:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Per attivare le funzionalità di modifica predefinite di GridView, selezionare l'opzione Abilita modifica dallo smart tag di GridView. In questo modo verrà impostata la proprietà `ShowEditButton` di CommandField s su `True`, ottenendo l'aggiunta di un pulsante modifica (e i pulsanti Aggiorna e Annulla per la riga in corso di modifica).

[![configurare GridView per supportare la modifica](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Figura 10**: configurare GridView per supportare la modifica ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image27.png))

Visitare la pagina tramite un browser e fare clic su uno dei pulsanti Modifica riga. I BoundField `CategoryName` e `Description` vengono visualizzati come caselle di testo. Il `BrochurePath` TemplateField non dispone di un `EditItemTemplate`, quindi continua a visualizzare il `ItemTemplate` un collegamento alla brochure. Il `Picture` ImageField viene visualizzato come casella di testo la cui proprietà `Text` viene assegnata al valore del valore `DataImageUrlField` di ImageField, in questo caso `CategoryID`.

[![GridView non dispone di un'interfaccia di modifica per BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Figura 11**: GridView non dispone di un'interfaccia di modifica per `BrochurePath` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image30.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizzazione dell'interfaccia di modifica di`BrochurePath`s

È necessario creare un'interfaccia di modifica per il `BrochurePath` TemplateField, che consente all'utente di effettuare una delle operazioni seguenti:

- Lasciare invariata la brochure della categoria,
- Aggiornare la brochure della categoria caricando una nuova brochure o
- Rimuovere completamente la brochure della categoria (nel caso in cui alla categoria non sia più associato un opuscolo).

È anche necessario aggiornare l'interfaccia di modifica di `Picture` ImageField s, ma verrà illustrata nel passaggio 7.

Dallo smart tag di GridView, fare clic sul collegamento modifica modelli e selezionare il `BrochurePath` TemplateField s `EditItemTemplate` dall'elenco a discesa. Aggiungere un controllo Web RadioButtonList a questo modello, impostando la relativa proprietà `ID` su `BrochureOptions` e la relativa proprietà `AutoPostBack` su `True`. Dal Finestra Proprietà fare clic sui puntini di sospensione nella proprietà `Items`, che consente di visualizzare l'editor della raccolta `ListItem`. Aggiungere le tre opzioni seguenti con `Value` s 1, 2 e 3 rispettivamente:

- Usa la brochure corrente
- Rimuovi la brochure corrente
- Carica nuova brochure

Impostare la prima proprietà `Selected` `ListItem` s su `True`.

![Aggiungere tre ListItem a RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Figura 12**: aggiungere tre `ListItem` s a RadioButtonList

Sotto l'oggetto RadioButtonList aggiungere un controllo FileUpload denominato `BrochureUpload`. Impostarne la proprietà `Visible` su `False`.

[![aggiungere un controllo RadioButtonList e FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Figura 13**: aggiungere un controllo RadioButtonList e FileUpload al `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image33.png))

Questo RadioButtonList fornisce le tre opzioni per l'utente. Il concetto è che il controllo FileUpload verrà visualizzato solo se è selezionata l'ultima opzione, carica nuova brochure. A tale scopo, creare un gestore eventi per l'evento `SelectedIndexChanged` di RadioButtonList e aggiungere il codice seguente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Poiché i controlli RadioButtonList e FileUpload sono all'interno di un modello, è necessario scrivere un po' di codice per accedere a questi controlli a livello di codice. Al gestore dell'evento `SelectedIndexChanged` viene passato un riferimento a RadioButtonList nell'`sender` parametro di input. Per ottenere il controllo FileUpload, è necessario ottenere il controllo padre di RadioButtonList e usare il metodo `FindControl("controlID")`. Una volta ottenuto un riferimento ai controlli RadioButtonList e FileUpload, la proprietà `Visible` del controllo FileUpload è impostata su `True` solo se l'oggetto RadioButtonList s `SelectedValue` è uguale a 3, che è il `Value` per il caricamento di una nuova brochure `ListItem`.

Con questo codice, dedicare un po' di tempo a testare l'interfaccia di modifica. Fare clic sul pulsante modifica per una riga. Inizialmente, è necessario selezionare l'opzione Usa brochure corrente. La modifica dell'indice selezionato causa un postback. Se la terza opzione è selezionata, viene visualizzato il controllo FileUpload, in caso contrario è nascosto. Nella figura 14 viene illustrata l'interfaccia di modifica quando viene prima fatto clic sul pulsante modifica; Nella figura 15 viene illustrata l'interfaccia dopo aver selezionato l'opzione Carica nuova brochure.

[![inizialmente è selezionata l'opzione Usa brochure corrente](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Figura 14**: inizialmente è selezionata l'opzione Usa brochure corrente ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image36.png))

[![la scelta dell'opzione Carica nuova brochure Visualizza il controllo FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Figura 15**: se si sceglie l'opzione Carica nuova brochure, viene visualizzato il controllo FileUpload ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image39.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Salvataggio del file della brochure e aggiornamento della colonna`BrochurePath`

Quando si fa clic sul pulsante di aggiornamento di GridView, viene generato l'evento `RowUpdating`. Viene richiamato il comando ObjectDataSource s Update, quindi viene generato l'evento GridView s `RowUpdated`. Come per il flusso di lavoro di eliminazione, è necessario creare gestori eventi per entrambi gli eventi. Nel gestore dell'evento `RowUpdating` è necessario determinare l'azione da eseguire in base alla `SelectedValue` del `BrochureOptions` RadioButtonList:

- Se il `SelectedValue` è 1, è necessario usare la stessa impostazione di `BrochurePath`. Pertanto, è necessario impostare il parametro ObjectDataSource s `brochurePath` sul valore `BrochurePath` esistente del record da aggiornare. È possibile impostare il parametro `brochurePath` di ObjectDataSource utilizzando `e.NewValues["brochurePath"] = value`.
- Se il `SelectedValue` è 2, è necessario impostare il valore di `BrochurePath` del record su `NULL`. Questa operazione può essere eseguita impostando il parametro ObjectDataSource s `brochurePath` su `Nothing`, il che comporta l'utilizzo di un database `NULL` nell'istruzione `UPDATE`. Se è presente un file di brochure esistente da rimuovere, è necessario eliminare il file esistente. Tuttavia, si desidera eseguire questa operazione solo se l'aggiornamento viene completato senza generare un'eccezione.
- Se il `SelectedValue` è 3, è necessario assicurarsi che l'utente abbia caricato un file PDF e quindi salvarlo nella file system e aggiornare il valore della colonna del record s `BrochurePath`. Inoltre, se è presente un file di opuscolo esistente che viene sostituito, è necessario eliminare il file precedente. Tuttavia, si desidera eseguire questa operazione solo se l'aggiornamento viene completato senza generare un'eccezione.

I passaggi necessari per il completamento quando il `SelectedValue` di RadioButtonList è 3 sono praticamente identici a quelli utilizzati dal gestore dell'evento DetailsView s `ItemInserting`. Questo gestore eventi viene eseguito quando viene aggiunto un nuovo record di categoria dal controllo DetailsView aggiunto nell' [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md). Pertanto, è necessario eseguire il refactoring di questa funzionalità in metodi distinti. In particolare, ho spostato le funzionalità comuni in due metodi:

- `ProcessBrochureUpload(FileUpload, out bool)` accetta come input un'istanza del controllo FileUpload e un valore booleano di output che specifica se l'operazione di eliminazione o modifica deve continuare o se deve essere annullata a causa di un errore di convalida. Questo metodo restituisce il percorso del file salvato o `null` se non è stato salvato alcun file.
- `DeleteRememberedBrochurePath` Elimina il file specificato dal percorso nella variabile di pagina `deletedCategorysPdfPath` se `deletedCategorysPdfPath` non è `null`.

Di seguito è riportato il codice per questi due metodi. Si noti la somiglianza tra `ProcessBrochureUpload` e il gestore dell'evento DetailsView s `ItemInserting` dell'esercitazione precedente. In questa esercitazione sono stati aggiornati i gestori di eventi DetailsView s per usare questi nuovi metodi. Scaricare il codice associato a questa esercitazione per visualizzare le modifiche ai gestori di eventi di DetailsView.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

I gestori di eventi `RowUpdating` e `RowUpdated` di GridView utilizzano i metodi `ProcessBrochureUpload` e `DeleteRememberedBrochurePath`, come illustrato nel codice seguente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Si noti il modo in cui il gestore dell'evento `RowUpdating` usa una serie di istruzioni condizionali per eseguire l'azione appropriata in base al valore della proprietà `SelectedValue` della `BrochureOptions` RadioButtonList.

Con questo codice, è possibile modificare una categoria e fare in uso la brochure corrente, non usare brochure o caricarne una nuova. Procediamo e proviamo. Impostare i punti di interruzione nei gestori eventi `RowUpdating` e `RowUpdated` per ottenere un'idea del flusso di lavoro.

## <a name="step-7-uploading-a-new-picture"></a>Passaggio 7: caricamento di una nuova immagine

L'interfaccia di modifica di `Picture` ImageField s viene visualizzata come casella di testo popolata con il valore della relativa proprietà `DataImageUrlField`. Durante il flusso di lavoro di modifica, GridView passa un parametro a ObjectDataSource con il nome del parametro, il valore della proprietà `DataImageUrlField` ImageField e il valore del parametro del valore immesso nella casella di testo nell'interfaccia di modifica. Questo comportamento è adatto quando l'immagine viene salvata come file nella file system e il `DataImageUrlField` contiene l'URL completo dell'immagine. In tali circostanze, l'interfaccia di modifica Visualizza l'URL dell'immagine nella casella di testo, che può essere modificata dall'utente e salvato nuovamente nel database. Questa interfaccia predefinita non consente all'utente di caricare una nuova immagine, ma consente loro di modificare l'URL dell'immagine dal valore corrente a un altro. Per questa esercitazione, tuttavia, l'interfaccia di modifica predefinita ImageField non è sufficiente perché i dati binari `Picture` vengono archiviati direttamente nel database e la proprietà `DataImageUrlField` include solo il `CategoryID`.

Per comprendere meglio ciò che accade nell'esercitazione quando un utente modifica una riga con un ImageField, si consideri l'esempio seguente: un utente modifica una riga con `CategoryID` 10, causando il rendering del `Picture` ImageField come casella di testo con il valore 10. Si supponga che l'utente modifichi il valore in questa casella di testo su 50 e fa clic sul pulsante Aggiorna. Si verifica un postback e GridView crea inizialmente un parametro denominato `CategoryID` con il valore 50. Tuttavia, prima che GridView invii questo parametro (e i parametri `CategoryName` e `Description`), aggiunge i valori della raccolta `DataKeys`. Pertanto, sovrascrive il parametro `CategoryID` con il valore `CategoryID` sottostante della riga corrente, 10. In breve, l'interfaccia di modifica di ImageField non ha alcun effetto sul flusso di lavoro di modifica per questa esercitazione, perché i nomi della proprietà `DataImageUrlField` ImageField e il valore `DataKey` della griglia sono uno nello stesso.

Sebbene il ImageField renda più semplice la visualizzazione di un'immagine in base ai dati del database, non è necessario fornire una casella di testo nell'interfaccia di modifica. Si vuole invece offrire un controllo FileUpload che l'utente finale può usare per modificare l'immagine di categoria. A differenza del valore `BrochurePath`, per queste esercitazioni si è deciso di richiedere che ogni categoria disponga di un'immagine. Non è quindi necessario consentire all'utente di indicare che non sono presenti immagini associate. l'utente può caricare una nuova immagine o lasciare l'immagine corrente così com'è.

Per personalizzare l'interfaccia di modifica di ImageField, è necessario convertirla in un TemplateField. Dallo smart tag di GridView, fare clic sul collegamento Modifica colonne, selezionare il ImageField, quindi fare clic sul collegamento Converti questo campo in un TemplateField.

![Convertire il ImageField in un TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Figura 16**: convertire il ImageField in un TemplateField

La conversione di ImageField in un TemplateField in questo modo genera un TemplateField con due modelli. Come illustrato nella sintassi dichiarativa seguente, il `ItemTemplate` contiene un controllo Web Image la cui proprietà `ImageUrl` viene assegnata usando la sintassi DataBinding basata sulle proprietà `DataImageUrlField` e `DataImageUrlFormatString` di ImageField. Il `EditItemTemplate` contiene una casella di testo la cui proprietà `Text` è associata al valore specificato dalla proprietà `DataImageUrlField`.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

È necessario aggiornare il `EditItemTemplate` per usare un controllo FileUpload. Dallo smart tag GridView s fare clic sul collegamento modifica modelli, quindi selezionare l'`Picture` TemplateField s `EditItemTemplate` dall'elenco a discesa. Nel modello dovrebbe essere visualizzata una casella di testo Remove this. Trascinare quindi un controllo FileUpload dalla casella degli strumenti nel modello, impostando la relativa `ID` su `PictureUpload`. Aggiungere anche il testo per modificare l'immagine della categoria, quindi specificare una nuova immagine. Per mantenere la stessa immagine della categoria, lasciare il campo vuoto anche per il modello.

[![aggiungere un controllo FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Figura 17**: aggiungere un controllo FileUpload al `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image42.png))

Dopo la personalizzazione dell'interfaccia di modifica, visualizzare lo stato di avanzamento in un browser. Quando si visualizza una riga in modalità di sola lettura, l'immagine della categoria viene visualizzata come prima, ma facendo clic sul pulsante modifica viene eseguito il rendering della colonna immagine come testo con un controllo FileUpload.

[![l'interfaccia di modifica include un controllo FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Figura 18**: l'interfaccia di modifica include un controllo FileUpload ([fare clic per visualizzare l'immagine con dimensioni complete](updating-and-deleting-existing-binary-data-vb/_static/image45.png))

Tenere presente che ObjectDataSource è configurato per chiamare il metodo `CategoriesBLL` Class s `UpdateCategory` che accetta come input i dati binari per l'immagine come matrice `Byte`. Se questa matrice è `Nothing`, tuttavia, viene chiamato l'overload `UpdateCategory` alternativo, che rilascia l'istruzione SQL `UPDATE` che non modifica la colonna `Picture`, lasciando intatta l'immagine corrente della categoria. Pertanto, nel gestore eventi `RowUpdating` GridView è necessario fare riferimento a livello di codice al controllo `PictureUpload` FileUpload e determinare se un file è stato caricato. Se non è stato caricato un valore, *non* è necessario specificare un valore per il parametro `picture`. D'altra parte, se un file è stato caricato nel controllo `PictureUpload` FileUpload, è necessario assicurarsi che sia un file JPG. In tal caso, è possibile inviare il contenuto binario a ObjectDataSource tramite il parametro `picture`.

Come nel caso del codice usato nel passaggio 6, gran parte del codice necessario qui esiste già nel gestore dell'evento DetailsView s `ItemInserting`. Quindi, ho sottoposto a refactoring le funzionalità comuni in un nuovo metodo, `ValidPictureUpload`e aggiornato il gestore dell'evento `ItemInserting` per usare questo metodo.

Aggiungere il codice seguente all'inizio del gestore dell'evento GridView s `RowUpdating`. È importante che il codice venga prima del codice che salva il file della brochure, dal momento che non si vuole salvare la brochure nel server Web file system se viene caricato un file di immagine non valido.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Il metodo `ValidPictureUpload(FileUpload)` accetta un controllo FileUpload come unico parametro di input e controlla l'estensione del file caricato per verificare che il file caricato sia un file JPG; viene chiamato solo se viene caricato un file di immagine. Se non viene caricato alcun file, il parametro Picture non è impostato e pertanto usa il valore predefinito `Nothing`. Se un'immagine è stata caricata e `ValidPictureUpload` restituisce `True`, al parametro `picture` vengono assegnati i dati binari dell'immagine caricata. Se il metodo restituisce `False`, il flusso di lavoro di aggiornamento viene annullato e il gestore eventi è stato terminato.

Il codice del metodo `ValidPictureUpload(FileUpload)`, che è stato sottoposto a refactoring dal gestore dell'evento DetailsView s `ItemInserting`, segue:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Passaggio 8: sostituzione delle immagini delle categorie originali con jpg

Si ricordi che le immagini originali di otto categorie sono file bitmap racchiusi in un'intestazione OLE. Ora che è stata aggiunta la funzionalità per modificare un'immagine di record esistente, è necessario sostituire le bitmap con jpg. Se si desidera continuare a utilizzare le immagini della categoria corrente, è possibile convertirle in jpg attenendosi alla procedura seguente:

1. Salvare le immagini bitmap sul disco rigido. Visitare la pagina `UpdatingAndDeleting.aspx` nel browser e per ognuna delle prime otto categorie, fare clic con il pulsante destro del mouse sull'immagine e scegliere di salvare l'immagine.
2. Aprire l'immagine nell'editor di immagini desiderato. È possibile usare Microsoft Paint, ad esempio.
3. Salvare la bitmap come immagine JPG.
4. Aggiornare l'immagine della categoria tramite l'interfaccia di modifica, usando il file JPG.

Dopo la modifica di una categoria e il caricamento dell'immagine JPG, non verrà eseguito il rendering dell'immagine nel browser perché la pagina `DisplayCategoryPicture.aspx` estrae i primi 78 byte dalle immagini delle prime otto categorie. Per risolvere il problema, rimuovere il codice che esegue la rimozione dell'intestazione OLE. Al termine di questa operazione, il gestore dell'evento `DisplayCategoryPicture.aspx``Page_Load` deve avere solo il codice seguente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Le interfacce di inserimento e modifica della pagina `UpdatingAndDeleting.aspx` possono usare un po' più di lavoro. I BoundField `CategoryName` e `Description` in DetailsView e GridView devono essere convertiti in TemplateFields. Poiché `CategoryName` non consente valori `NULL`, è necessario aggiungere un RequiredFieldValidator. E la casella di testo `Description` deve probabilmente essere convertita in una casella di testo a più righe. Lascio questi ultimi ritocchi come esercizio.

## <a name="summary"></a>Riepilogo

Questa esercitazione completa l'uso di dati binari. In questa esercitazione e nelle tre precedenti è stato illustrato il modo in cui i dati binari possono essere archiviati nel file system o direttamente nel database. Un utente fornisce dati binari al sistema selezionando un file dal relativo disco rigido e caricando il file nel server Web, dove può essere archiviato nel file system o inserito nel database. ASP.NET 2,0 include un controllo FileUpload che consente di fornire un'interfaccia di questo tipo semplice come il trascinamento della selezione. Tuttavia, come indicato nell'esercitazione sul [caricamento di file](uploading-files-vb.md) , il controllo FileUpload è particolarmente adatto per i caricamenti di file relativamente piccoli, idealmente senza superare un megabyte. Si è inoltre esplorato come associare dati caricati al modello di dati sottostante, nonché come modificare ed eliminare i dati binari da record esistenti.

Il prossimo set di esercitazioni Esplora varie tecniche di Caching. La memorizzazione nella cache consente di migliorare le prestazioni complessive di un'applicazione eseguendo i risultati delle operazioni dispendiose e archiviarle in una posizione a cui è possibile accedere più rapidamente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md)
