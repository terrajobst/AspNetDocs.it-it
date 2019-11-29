---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Inclusione di un'opzione di caricamento di file durante l'aggiunta di un nuovo record (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come creare un'interfaccia Web che consente all'utente di immettere dati di testo e caricare file binari. Per illustrare le opzioni disponibili in t...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edaf1754eddd7b03f1c323d1bee13238582fc99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596844"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Inclusione di un'opzione per il caricamento di file durante l'aggiunta di un nuovo record (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) o [scaricare il file PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Questa esercitazione illustra come creare un'interfaccia Web che consente all'utente di immettere dati di testo e caricare file binari. Per illustrare le opzioni disponibili per archiviare i dati binari, un file verrà salvato nel database mentre l'altro viene archiviato nella file system.

## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti sono state esplorate le tecniche per l'archiviazione di dati binari associati al modello di dati dell'applicazione, viene esaminato come utilizzare il controllo FileUpload per inviare i file dal client al server Web e viene illustrato come presentare questi dati binari in un data W controllo EB. Si parlerà comunque di come associare dati caricati al modello di dati.

In questa esercitazione verrà creata una pagina Web per aggiungere una nuova categoria. Oltre alle caselle di testo per il nome e la descrizione della categoria, in questa pagina è necessario includere due controlli FileUpload uno per la nuova immagine della categoria e uno per la brochure. L'immagine caricata verrà archiviata direttamente nel nuovo record s `Picture` colonna, mentre la brochure verrà salvata nella cartella `~/Brochures` con il percorso del file salvato nella colonna `BrochurePath` nuovo record.

Prima di creare la nuova pagina Web, è necessario aggiornare l'architettura. La query principale `CategoriesTableAdapter` s non recupera la colonna `Picture`. Di conseguenza, il metodo di `Insert` generato automaticamente dispone solo di input per i campi `CategoryName`, `Description`e `BrochurePath`. Pertanto, è necessario creare un metodo aggiuntivo nel TableAdapter che richiede tutti e quattro i campi `Categories`. Sarà inoltre necessario aggiornare la classe `CategoriesBLL` nel livello della logica di business.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Passaggio 1: aggiunta di un metodo di`InsertWithPicture`al`CategoriesTableAdapter`

Quando il `CategoriesTableAdapter` è stato creato nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) , è stato configurato per generare automaticamente `INSERT`, `UPDATE`e `DELETE` istruzioni basate sulla query principale. Inoltre, è stato indicato al TableAdapter di utilizzare l'approccio diretto del database, che ha creato i metodi `Insert`, `Update`e `Delete`. Questi metodi eseguono le istruzioni `INSERT`, `UPDATE`e `DELETE` generate automaticamente e, di conseguenza, accettano i parametri di input in base alle colonne restituite dalla query principale. Nell'esercitazione sul [caricamento dei file](uploading-files-vb.md) è stata aumentata la query principale `CategoriesTableAdapter` s per usare la colonna `BrochurePath`.

Poiché la query principale `CategoriesTableAdapter` s non fa riferimento alla colonna `Picture`, non è possibile aggiungere un nuovo record né aggiornare un record esistente con un valore per la colonna `Picture`. Per acquisire queste informazioni, è possibile creare un nuovo metodo nell'oggetto TableAdapter usato in modo specifico per inserire un record con dati binari oppure è possibile personalizzare l'istruzione `INSERT` generata automaticamente. Il problema della personalizzazione dell'istruzione `INSERT` generata automaticamente è il rischio che le personalizzazioni vengano sovrascritte dalla procedura guidata. Si supponga, ad esempio, di avere personalizzato l'istruzione `INSERT` per includere l'uso della colonna `Picture`. Questa operazione aggiornerà il metodo `Insert` di TableAdapter in modo da includere un parametro di input aggiuntivo per i dati binari della categoria s Picture s. È quindi possibile creare un metodo nel livello della logica di business per usare questo metodo DAL e richiamare questo metodo BLL tramite il livello di presentazione e tutto funziona benissimo. In altre termini, fino alla successiva configurazione del TableAdapter tramite la configurazione guidata TableAdapter. Al termine della procedura guidata, le personalizzazioni apportate all'istruzione `INSERT` verranno sovrascritte, il metodo `Insert` ripristina la forma precedente e il codice non verrà più compilato.

> [!NOTE]
> Questo fastidio è un problema che si verifica quando si utilizzano stored procedure anziché istruzioni SQL ad hoc. Un'esercitazione futura analizzerà l'uso di stored procedure al posto delle istruzioni SQL ad hoc nel livello di accesso ai dati.

Per evitare questa potenziale cefalea, anziché personalizzare le istruzioni SQL generate automaticamente, è necessario creare un nuovo metodo per il TableAdapter. Questo metodo, denominato `InsertWithPicture`, accetterà i valori per le colonne `CategoryName`, `Description`, `BrochurePath`e `Picture` ed eseguirà un'istruzione `INSERT` che archivia tutti e quattro i valori in un nuovo record.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic con il pulsante destro del mouse sull'intestazione `CategoriesTableAdapter` s e scegliere Aggiungi query dal menu di scelta rapida. Viene avviata la configurazione guidata query TableAdapter, che inizia chiedendo come la query TableAdapter deve accedere al database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo richiede la generazione del tipo di query. Poiché viene ricreata una query per aggiungere un nuovo record alla tabella `Categories`, scegliere Inserisci e fare clic su Avanti.

[![selezionare l'opzione Inserisci](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figura 1**: selezionare l'opzione Inserisci ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))

A questo punto è necessario specificare l'istruzione SQL `INSERT`. La procedura guidata suggerisce automaticamente un'istruzione `INSERT` corrispondente alla query principale di TableAdapter. In questo caso, è un'istruzione `INSERT` che inserisce i valori `CategoryName`, `Description`e `BrochurePath`. Aggiornare l'istruzione in modo che la colonna `Picture` venga inclusa insieme a un parametro di `@Picture`, come indicato di seguito:

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

Nella schermata finale della procedura guidata viene chiesto di assegnare un nome al nuovo metodo TableAdapter. Immettere `InsertWithPicture` e fare clic su fine.

[![nome del nuovo metodo TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figura 2**: assegnare un nome al nuovo metodo TableAdapter `InsertWithPicture` ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Passaggio 2: aggiornamento del livello della logica di business

Poiché il livello di presentazione deve interfacciarsi solo con il livello della logica di business anziché ignorarlo per passare direttamente al livello di accesso ai dati, è necessario creare un metodo BLL che richiama il metodo DAL appena creato (`InsertWithPicture`). Per questa esercitazione, creare un metodo nella classe `CategoriesBLL` denominata `InsertWithPicture` che accetta come input tre `String` s e una matrice `Byte`. I parametri di input `String` sono per il nome, la descrizione e il percorso del file della brochure della categoria, mentre la matrice di `Byte` è per il contenuto binario dell'immagine della categoria. Come illustrato nel codice seguente, questo metodo BLL richiama il metodo DAL corrispondente:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Assicurarsi di aver salvato il set di dati tipizzato prima di aggiungere il metodo di `InsertWithPicture` al BLL. Poiché il codice della classe `CategoriesTableAdapter` viene generato automaticamente in base al set di dati tipizzato, se non si salvano prima le modifiche nel DataSet tipizzato, la proprietà `Adapter` non sarà in grado di conoscere il metodo di `InsertWithPicture`.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Passaggio 3: elenco delle categorie esistenti e dei relativi dati binari

In questa esercitazione verrà creata una pagina che consente a un utente finale di aggiungere una nuova categoria al sistema, fornendo un'immagine e una brochure per la nuova categoria. Nell' [esercitazione precedente](displaying-binary-data-in-the-data-web-controls-vb.md) è stato usato GridView con TemplateField e ImageField per visualizzare il nome, la descrizione, l'immagine e il collegamento di ogni categoria per scaricare la relativa brochure. Consente di replicare tale funzionalità per questa esercitazione, creando una pagina in cui sono elencate tutte le categorie esistenti e consente la creazione di nuove.

Per iniziare, aprire la pagina `DisplayOrDownload.aspx` dalla cartella `BinaryData`. Passare alla visualizzazione origine e copiare la sintassi dichiarativa GridView e ObjectDataSource, incollando l'elemento all'interno dell'elemento `<asp:Content>` in `UploadInDetailsView.aspx`. Inoltre, Don t dimentica di copiare il metodo `GenerateBrochureLink` dalla classe code-behind di `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.

[![copiare e incollare la sintassi dichiarativa da DisplayOrDownload. aspx in UploadInDetailsView. aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figura 3**: copiare e incollare la sintassi dichiarativa da `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))

Dopo aver copiato la sintassi dichiarativa e `GenerateBrochureLink` metodo nella pagina `UploadInDetailsView.aspx`, visualizzare la pagina tramite un browser per assicurarsi che tutti gli elementi siano stati copiati correttamente. Verrà visualizzato un controllo GridView che elenca le otto categorie che includono un collegamento per scaricare la brochure e l'immagine di categoria.

[![è ora possibile visualizzare ogni categoria insieme ai relativi dati binari](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figura 4**: è ora possibile visualizzare ogni categoria insieme ai relativi dati binari ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Passaggio 4: configurazione del`CategoriesDataSource`per supportare l'inserimento

Il `CategoriesDataSource` ObjectDataSource utilizzato dal `Categories` GridView attualmente non offre la possibilità di inserire dati. Per supportare l'inserimento tramite questo controllo origine dati, è necessario eseguire il mapping del relativo `Insert` metodo a un metodo nell'oggetto sottostante, `CategoriesBLL`. In particolare, è necessario eseguirne il mapping al metodo `CategoriesBLL` aggiunto nel passaggio 2 `InsertWithPicture`.

Per iniziare, fare clic sul collegamento Configura origine dati dallo smart tag ObjectDataSource s. La prima schermata mostra l'oggetto con cui è configurata l'origine dati, `CategoriesBLL`. Lasciare invariata questa impostazione e fare clic su Avanti per passare alla schermata Definisci metodi dati. Passare alla scheda Inserisci e selezionare il metodo `InsertWithPicture` dall'elenco a discesa. Fare clic su Fine per completare la procedura guidata.

[![configurare ObjectDataSource per l'utilizzo del metodo InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per l'uso del metodo `InsertWithPicture` ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))

> [!NOTE]
> Al termine della procedura guidata, è possibile che in Visual Studio venga chiesto se si desidera aggiornare i campi e le chiavi, in modo da rigenerare i campi dei controlli Web. Scegliere No, perché se si sceglie Sì, eventuali personalizzazioni dei campi che potrebbero essere state apportate vengono sovrascritte.

Al termine della procedura guidata, in ObjectDataSource verrà ora incluso un valore per la proprietà `InsertMethod` e `InsertParameters` per le quattro colonne della categoria, come illustrato nel markup dichiarativo seguente:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Passaggio 5: creazione dell'interfaccia di inserimento

Come illustrato per la prima volta in [una panoramica dell'inserimento, dell'aggiornamento e dell'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), il controllo DetailsView fornisce un'interfaccia di inserimento incorporata che può essere utilizzata quando si utilizza un controllo origine dati che supporta l'inserimento. Consente di aggiungere un controllo DetailsView a questa pagina sopra GridView che eseguirà il rendering permanente della relativa interfaccia di inserimento, consentendo a un utente di aggiungere rapidamente una nuova categoria. Quando si aggiunge una nuova categoria in DetailsView, il controllo GridView sottostante aggiorna automaticamente e visualizza la nuova categoria.

Per iniziare, trascinare un oggetto DetailsView dalla casella degli strumenti nella finestra di progettazione sopra il GridView, impostare la relativa proprietà `ID` su `NewCategory` ed eliminare i valori di `Height` e `Width` della proprietà. Dallo smart tag di DetailsView, associarlo alla `CategoriesDataSource` esistente e quindi selezionare la casella di controllo Abilita inserimento.

[![associare DetailsView a CategoriesDataSource e abilitare l'inserimento](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figura 6**: associare DetailsView al `CategoriesDataSource` e abilitare l'inserimento ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))

Per eseguire il rendering permanente di DetailsView nell'interfaccia di inserimento, impostare la relativa proprietà `DefaultMode` su `Insert`.

Si noti che DetailsView ha cinque BoundField `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath` anche se non viene eseguito il rendering del `CategoryID` BoundField nell'interfaccia di inserimento perché la relativa proprietà `InsertVisible` è impostata su `False`. Questi BoundField sono disponibili perché sono le colonne restituite dal metodo `GetCategories()`, ovvero ciò che ObjectDataSource richiama per recuperare i dati. Per l'inserimento, tuttavia, non si desidera consentire all'utente di specificare un valore per `NumberOfProducts`. Inoltre, è necessario consentire loro di caricare un'immagine per la nuova categoria, nonché di caricare un file PDF per la brochure.

Rimuovere il `NumberOfProducts` BoundField da DetailsView, quindi aggiornare le proprietà del `HeaderText` del `CategoryName` e `BrochurePath` i BoundField rispettivamente alla categoria e alla brochure. Convertire quindi il BoundField `BrochurePath` in un TemplateField e aggiungere una nuova TemplateField per l'immagine, assegnando a questo nuovo TemplateField un valore `HeaderText` immagine. Spostare il `Picture` TemplateField in modo che sia compreso tra i `BrochurePath` TemplateField e CommandField.

![Associare DetailsView a CategoriesDataSource e abilitare l'inserimento](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figura 7**: associare DetailsView al `CategoriesDataSource` e abilitare l'inserimento

Se la `BrochurePath` BoundField è stata convertita in un TemplateField tramite la finestra di dialogo Modifica campi, TemplateField include `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate`. È tuttavia necessario solo il `InsertItemTemplate`, quindi è possibile rimuovere gli altri due modelli. A questo punto la sintassi dichiarativa di DetailsView sarà simile alla seguente:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Aggiunta di controlli FileUpload per la brochure e i campi immagine

Attualmente, il `BrochurePath` TemplateField s `InsertItemTemplate` contiene una casella di testo, mentre la `Picture` TemplateField non contiene alcun modello. È necessario aggiornare questi due TemplateField `InsertItemTemplate` s per usare i controlli FileUpload.

Dallo smart tag di DetailsView, scegliere l'opzione modifica modelli, quindi selezionare l'`BrochurePath` TemplateField s `InsertItemTemplate` dall'elenco a discesa. Rimuovere la casella di testo e trascinare un controllo FileUpload dalla casella degli strumenti nel modello. Impostare il `ID` del controllo FileUpload su `BrochureUpload`. Analogamente, aggiungere un controllo FileUpload al `InsertItemTemplate``Picture` TemplateField s. Impostare questo controllo FileUpload s `ID` su `PictureUpload`.

[![aggiungere un controllo FileUpload a InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figura 8**: aggiungere un controllo FileUpload al `InsertItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))

Dopo aver apportato queste aggiunte, la sintassi dichiarativa per due TemplateField sarà:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Quando un utente aggiunge una nuova categoria, è necessario assicurarsi che la brochure e l'immagine siano del tipo di file corretto. Per la brochure, l'utente deve fornire un file PDF. Per l'immagine, è necessario che l'utente carica un file di immagine, ma è possibile usare *qualsiasi* file di immagine o solo file di immagine di un determinato tipo, ad esempio GIF o jpg. Per consentire tipi di file diversi, è necessario estendere lo schema `Categories` per includere una colonna che acquisisce il tipo di file in modo che questo tipo possa essere inviato al client tramite `Response.ContentType` in `DisplayCategoryPicture.aspx`. Poiché non è presente una colonna di questo tipo, è consigliabile limitare gli utenti a fornire solo un tipo di file di immagine specifico. Le immagini esistenti della tabella `Categories` sono bitmap, ma jpg sono un formato di file più appropriato per le immagini servite sul Web.

Se un utente carica un tipo di file non corretto, è necessario annullare l'inserimento e visualizzare un messaggio che indica il problema. Aggiungere un controllo Web etichetta sotto DetailsView. Impostare la relativa proprietà `ID` su `UploadWarning`, deselezionare la relativa proprietà `Text`, impostare la proprietà `CssClass` su Warning e le proprietà `Visible` e `EnableViewState` su `False`. La `Warning` classe CSS è definita in `Styles.css` ed esegue il rendering del testo in un tipo di carattere in grassetto, in rosso, in corsivo.

> [!NOTE]
> Idealmente, i BoundField `CategoryName` e `Description` verranno convertiti in TemplateFields e le interfacce di inserimento personalizzate. Il `Description` interfaccia di inserimento, ad esempio, sarebbe probabilmente più appropriato tramite una casella di testo a più righe. Poiché la colonna `CategoryName` non accetta valori `NULL`, è necessario aggiungere un RequiredFieldValidator per assicurarsi che l'utente fornisca un valore per il nome della nuova categoria. Questa procedura viene lasciata come esercizio al lettore. Fare riferimento alla pagina relativa alla [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) per un approfondimento sull'aumento delle interfacce di modifica dei dati.

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Passaggio 6: salvataggio della brochure caricata nel file System del server Web

Quando l'utente immette i valori per una nuova categoria e fa clic sul pulsante Inserisci, viene eseguito un postback e il flusso di lavoro di inserimento viene ripiegato. Innanzitutto, viene generato l' [evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) DetailsView s`ItemInserting`. Viene quindi richiamato il Metodo ObjectDataSource s `Insert()`, che comporta l'aggiunta di un nuovo record alla tabella `Categories`. Successivamente, viene generato l'evento DetailsView s [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) .

Prima che venga richiamato il Metodo ObjectDataSource s `Insert()`, prima di tutto è necessario verificare che i tipi di file appropriati siano stati caricati dall'utente e quindi salvare la brochure PDF sul file system del server Web. Creare un gestore eventi per l'evento `ItemInserting` DetailsView e aggiungere il codice seguente:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

Il gestore eventi inizia facendo riferimento al controllo `BrochureUpload` FileUpload dai modelli DetailsView. Quindi, se è stata caricata una brochure, viene esaminata l'estensione del file caricato. Se l'estensione non è. PDF, viene visualizzato un avviso, l'inserimento viene annullato e viene terminata l'esecuzione del gestore eventi.

> [!NOTE]
> Basarsi sull'estensione del file caricato non è una tecnica sicura per garantire che il file caricato sia un documento PDF. È possibile che l'utente disponga di un documento PDF valido con l'estensione `.Brochure`o che abbia accettato un documento non PDF e abbia assegnato un'estensione `.pdf`. Il contenuto binario del file deve essere esaminato a livello di codice per verificare in modo più conclusivo il tipo di file. Tali approcci, tuttavia, sono spesso eccessivi. il controllo dell'estensione è sufficiente per la maggior parte degli scenari.

Come illustrato nell'esercitazione sul [caricamento di file](uploading-files-vb.md) , è necessario prestare attenzione quando si salvano i file nel file System in modo che il caricamento di un utente non sovrascriva le altre. Per questa esercitazione si tenterà di usare lo stesso nome del file caricato. Se esiste già un file nella directory `~/Brochures` con lo stesso nome file, tuttavia, verrà aggiunto un numero alla fine finché non viene trovato un nome univoco. Ad esempio, se l'utente carica un file di brochure denominato `Meats.pdf`, ma esiste già un file denominato `Meats.pdf` nella cartella `~/Brochures`, il nome del file salvato verrà modificato in `Meats-1.pdf`. Se esiste, si tenterà di `Meats-2.pdf`e così via fino a quando non viene trovato un nome di file univoco.

Il codice seguente usa il [metodo`File.Exists(path)`](https://msdn.microsoft.com/library/system.io.file.exists.aspx) per determinare se esiste già un file con il nome file specificato. In tal caso, continua a provare i nuovi nomi di file per la brochure fino a quando non viene rilevato alcun conflitto.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Una volta trovato un nome di file valido, il file deve essere salvato nel file system e il valore di `brochurePath``InsertParameter` di ObjectDataSource deve essere aggiornato in modo che il nome file venga scritto nel database. Come è stato illustrato nell'esercitazione sul *caricamento di file* , il file può essere salvato usando il metodo del controllo FileUpload s `SaveAs(path)`. Per aggiornare il parametro `brochurePath` di ObjectDataSource, utilizzare la raccolta `e.Values`.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Passaggio 7: salvataggio dell'immagine caricata nel database

Per archiviare l'immagine caricata nel nuovo record di `Categories`, è necessario assegnare il contenuto binario caricato al parametro di ObjectDataSource s `picture` nell'evento `ItemInserting` di DetailsView. Prima di eseguire questa assegnazione, tuttavia, dobbiamo innanzitutto verificare che l'immagine caricata sia un formato JPG e non un altro tipo di immagine. Come nel passaggio 6, è possibile usare l'estensione di file dell'immagine caricata per verificare il tipo.

Sebbene la tabella `Categories` consenta `NULL` valori per la colonna `Picture`, tutte le categorie dispongono attualmente di un'immagine. Consente di forzare l'utente a fornire un'immagine durante l'aggiunta di una nuova categoria tramite questa pagina. Il codice seguente verifica che un'immagine sia stata caricata e che disponga di un'estensione appropriata.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Questo codice deve essere inserito *prima* del codice del passaggio 6, in modo che se si verifica un problema con il caricamento dell'immagine, il gestore eventi terminerà prima che il file della brochure venga salvato nel file System.

Supponendo che sia stato caricato un file appropriato, assegnare il contenuto binario caricato al valore del parametro immagine con la riga di codice seguente:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>Gestore dell'evento`ItemInserting`completo

Per completezza, di seguito è riportato il `ItemInserting` gestore dell'evento nel suo complesso:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Passaggio 8: correzione della pagina`DisplayCategoryPicture.aspx`

È possibile eseguire il test dell'interfaccia di inserimento e `ItemInserting` gestore eventi creato negli ultimi passaggi. Visitare la pagina `UploadInDetailsView.aspx` tramite un browser e provare ad aggiungere una categoria, ma omettere l'immagine o specificare un'immagine non JPG o una brochure non PDF. In uno di questi casi, verrà visualizzato un messaggio di errore e il flusso di lavoro di inserimento verrà annullato.

[![viene visualizzato un messaggio di avviso se viene caricato un tipo di file non valido](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figura 9**: viene visualizzato un messaggio di avviso se viene caricato un tipo di file non valido ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))

Dopo aver verificato che la pagina richiede il caricamento di un'immagine e non accetterà file non PDF o non JPG, aggiungere una nuova categoria con un'immagine JPG valida, lasciando vuoto il campo della brochure. Dopo aver fatto clic sul pulsante Inserisci, verrà eseguito il postback della pagina e verrà aggiunto un nuovo record alla tabella `Categories` con i contenuti binari dell'immagine caricata archiviati direttamente nel database. GridView viene aggiornato e Mostra una riga per la nuova categoria aggiunta, ma, come illustrato nella figura 10, il rendering dell'immagine della nuova categoria non viene eseguito correttamente.

[![l'immagine della nuova categoria non viene visualizzata](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figura 10**: l'immagine della nuova categoria non è visualizzata ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))

Il motivo per cui non viene visualizzata la nuova immagine è che la pagina `DisplayCategoryPicture.aspx` che restituisce l'immagine di una categoria specificata è configurata in modo da elaborare le bitmap con un'intestazione OLE. Questa intestazione di 78 byte viene rimossa dal contenuto binario della colonna `Picture` prima che vengano restituiti al client. Ma il file JPG appena caricato per la nuova categoria non ha questa intestazione OLE; Pertanto, i byte necessari validi verranno rimossi dai dati binari dell'immagine.

Poiché ora sono presenti entrambe le bitmap con intestazioni OLE e jpg nella tabella `Categories`, è necessario aggiornare `DisplayCategoryPicture.aspx` in modo che l'intestazione OLE venga rimossa per le otto categorie originali e ignori questa rimozione per i record di categoria più recenti. Nell'esercitazione successiva verrà esaminato come aggiornare un'immagine di record esistente e verranno aggiornate tutte le immagini di categoria precedenti in modo che siano jpg. Per il momento, tuttavia, usare il codice seguente in `DisplayCategoryPicture.aspx` per rimuovere le intestazioni OLE solo per le otto categorie originali:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Con questa modifica, l'immagine JPG viene ora sottoposta a rendering correttamente in GridView.

[![viene eseguito correttamente il rendering delle immagini JPG per le nuove categorie](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figura 11**: rendering corretto delle immagini jpg per le nuove categorie ([fare clic per visualizzare l'immagine con dimensioni complete](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Passaggio 9: eliminazione della brochure in presenza di un'eccezione

Uno dei problemi relativi all'archiviazione dei dati binari sul server Web file system è che introduce una disconnessione tra il modello di dati e i relativi dati binari. Pertanto, ogni volta che un record viene eliminato, è necessario rimuovere anche i dati binari corrispondenti nel file system. Questa operazione può essere riprodotto anche quando si inserisce. Si consideri lo scenario seguente: un utente aggiunge una nuova categoria, specificando un'immagine e un opuscolo validi. Quando si fa clic sul pulsante Inserisci, viene eseguito un postback e viene generato l'evento DetailsView s `ItemInserting`, salvando la brochure sul file system del server Web. Viene quindi richiamato il Metodo ObjectDataSource s `Insert()`, che chiama il metodo `InsertWithPicture` della classe `CategoriesBLL`, che chiama il metodo `InsertWithPicture` `CategoriesTableAdapter` s.

A questo punto, cosa accade se il database è offline o se si verifica un errore nell'`INSERT` istruzione SQL? Ovviamente, l'inserimento non riuscirà, pertanto non verrà aggiunta alcuna nuova riga di categoria al database. Ma il file della brochure caricato è ancora presente sul server Web file system. Questo file deve essere eliminato in presenza di un'eccezione durante il flusso di lavoro di inserimento.

Come descritto in precedenza nella [pagina relativa alla gestione delle eccezioni a livello BLL e dal in un'esercitazione della pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) , quando viene generata un'eccezione dall'interno della profondità dell'architettura, viene eseguito il propagazione dei vari livelli. A livello di presentazione, è possibile determinare se si è verificata un'eccezione dall'evento `ItemInserted` DetailsView. Questo gestore eventi fornisce inoltre i valori di ObjectDataSource s `InsertParameters`. Pertanto, è possibile creare un gestore eventi per l'evento `ItemInserted` che controlla se si è verificata un'eccezione e, in tal caso, Elimina il file specificato dal parametro `brochurePath` di ObjectDataSource:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Riepilogo

Per fornire un'interfaccia basata sul Web per l'aggiunta di record che includono dati binari, è necessario eseguire una serie di passaggi. Se i dati binari vengono archiviati direttamente nel database, è probabile che sia necessario aggiornare l'architettura, aggiungendo metodi specifici per gestire il caso in cui vengono inseriti i dati binari. Una volta aggiornata l'architettura, il passaggio successivo consiste nel creare l'interfaccia di inserimento, che può essere eseguita usando un oggetto DetailsView che è stato personalizzato per includere un controllo FileUpload per ogni campo di dati binario. I dati caricati possono quindi essere salvati nell'file system del server Web o assegnati a un parametro dell'origine dati nel gestore dell'evento DetailsView s `ItemInserting`.

Il salvataggio dei dati binari nel file system richiede una pianificazione maggiore rispetto al salvataggio dei dati direttamente nel database. È necessario scegliere uno schema di denominazione per evitare il caricamento di un utente sovrascrivendo un altro. Inoltre, è necessario eseguire passaggi aggiuntivi per eliminare il file caricato se l'inserimento del database ha esito negativo.

È ora possibile aggiungere nuove categorie al sistema con un opuscolo e un'immagine, ma è ancora necessario esaminare come aggiornare i dati binari di una categoria esistente o come rimuovere correttamente i dati binari per una categoria eliminata. Questi due argomenti verranno esaminati nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Dave Gardner, Teresa Murphy e Bernadette Leigh. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Successivo](updating-and-deleting-existing-binary-data-vb.md)
