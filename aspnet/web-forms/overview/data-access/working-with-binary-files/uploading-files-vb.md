---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Caricamento di file (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come consentire agli utenti di caricare file binari (ad esempio documenti di Word o PDF) nel sito Web in cui possono essere archiviati nel file system del server...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e0d57ef2f1e8132f19777a7d14e94611c68adcd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615449"
---
# <a name="uploading-files-vb"></a>Caricamento di file (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) o [scaricare il file PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Informazioni su come consentire agli utenti di caricare file binari (ad esempio documenti di Word o PDF) nel sito Web in cui possono essere archiviati nel file system o nel database del server.

## <a name="introduction"></a>Introduzione

Tutte le esercitazioni esaminate finora hanno funzionato esclusivamente con i dati di testo. Tuttavia, molte applicazioni dispongono di modelli di dati che acquisiscono dati di testo e binari. Un sito di data dating online potrebbe consentire agli utenti di caricare un'immagine da associare al profilo. Un sito Web di reclutamento potrebbe consentire agli utenti di caricare il proprio curriculum come documento di Microsoft Word o PDF.

L'utilizzo di dati binari comporta l'aggiunta di un nuovo set di problemi. È necessario decidere la modalità di archiviazione dei dati binari nell'applicazione. L'interfaccia utilizzata per l'inserimento di nuovi record deve essere aggiornata per consentire all'utente di caricare un file dal computer ed è necessario eseguire passaggi aggiuntivi per visualizzare o fornire un mezzo per scaricare i dati binari associati a un record. In questa esercitazione e nei prossimi tre verrà illustrato come superare tali problemi. Al termine di queste esercitazioni è stata creata un'applicazione completamente funzionante che associa un'immagine e un opuscolo PDF a ogni categoria. In questa esercitazione verranno esaminate diverse tecniche per l'archiviazione dei dati binari e verrà illustrato come consentire agli utenti di caricare un file dal computer e salvarlo sul server Web file system.

> [!NOTE]
> I dati binari che fanno parte del modello di dati di un'applicazione vengono talvolta indicati come [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), acronimo di Binary Large Object. In queste esercitazioni ho scelto di usare i dati binari della terminologia, anche se il termine BLOB è sinonimo.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Passaggio 1: creazione dell'utilizzo di pagine Web di dati binari

Prima di iniziare a esplorare le questioni associate all'aggiunta del supporto per i dati binari, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le prossime tre. Per iniziare, aggiungere una nuova cartella denominata `BinaryData`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari](uploading-files-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari

Analogamente alle altre cartelle, `Default.aspx` nella cartella `BinaryData` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image2.png))

Infine, aggiungere queste pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo aver migliorato il `<siteMapNode>`GridView:

[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni sull'utilizzo di dati binari.

![La mappa del sito include ora le voci per le esercitazioni sull'utilizzo di dati binari](uploading-files-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora le voci per le esercitazioni sull'utilizzo di dati binari

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Passaggio 2: decidere dove archiviare i dati binari

I dati binari associati al modello di dati dell'applicazione possono essere archiviati in una delle due posizioni seguenti: sul server Web file system con un riferimento al file archiviato nel database. o direttamente all'interno del database stesso (vedere la figura 4). Ogni approccio ha un proprio set di vantaggi e svantaggi e merita una discussione più dettagliata.

[![dati binari possono essere archiviati nel file System o direttamente nel database](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: i dati binari possono essere archiviati nel file System o direttamente nel database ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image4.png))

Si supponga di voler estendere il database Northwind per associare un'immagine a ogni prodotto. Un'opzione consiste nell'archiviare questi file di immagine sul server Web file system e registrare il percorso nella tabella `Products`. Con questo approccio, si aggiunge una colonna `ImagePath` alla tabella `Products` di tipo `varchar(200)`, ad esempio. Quando un utente ha caricato un'immagine per Chai, tale immagine potrebbe essere archiviata nel server Web file system in `~/Images/Tea.jpg`, in cui `~` rappresenta il percorso fisico dell'applicazione. Ovvero, se il sito Web è radicato nel percorso fisico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` sarebbe equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Dopo aver caricato il file di immagine, si aggiorna il record Chai nella tabella `Products` in modo che la colonna `ImagePath` faccia riferimento al percorso della nuova immagine. È possibile utilizzare `~/Images/Tea.jpg` o semplicemente `Tea.jpg` se si è deciso di inserire tutte le immagini del prodotto nella cartella `Images` dell'applicazione.

I principali vantaggi derivanti dall'archiviazione dei dati binari nel file system sono i seguenti:

- Una **semplice implementazione** , come si vedrà a breve, l'archiviazione e il recupero di dati binari archiviati direttamente nel database implicano un numero maggiore di codice rispetto a quando si utilizzano i dati attraverso la file System. Inoltre, per consentire a un utente di visualizzare o scaricare i dati binari, è necessario che venga visualizzato un URL per tali dati. Se i dati si trovano nel file system del server Web, l'URL è semplice. Se i dati vengono archiviati nel database, tuttavia, è necessario creare una pagina Web per recuperare e restituire i dati dal database.
- **Accesso più ampio ai dati binari** è possibile che i dati binari debbano essere accessibili ad altri servizi o applicazioni, quelli che non possono eseguire il pull dei dati dal database. Ad esempio, le immagini associate a ogni prodotto possono anche essere disponibili per gli utenti tramite [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), nel qual caso è necessario archiviare i dati binari nel file System.
- **Prestazioni** se i dati binari vengono archiviati nel file System, la congestione della richiesta e della rete tra il server di database e il server Web sarà inferiore a se i dati binari vengono archiviati direttamente nel database.

Lo svantaggio principale dell'archiviazione dei dati binari nell'file system è che separa i dati dal database. Se un record viene eliminato dalla tabella `Products`, il file associato nel file system del server Web non viene eliminato automaticamente. È necessario scrivere codice aggiuntivo per eliminare il file o il file system diventerà ingombrante con i file orfani non usati. Inoltre, quando si esegue il backup del database, è necessario assicurarsi di eseguire i backup dei dati binari associati anche sul file system. Lo stato di trasferimento del database in un altro sito o server comporta problemi analoghi.

In alternativa, è possibile archiviare i dati binari direttamente in un database Microsoft SQL Server 2005 creando una colonna di tipo `varbinary`. Analogamente a quanto avviene con altri tipi di dati a lunghezza variabile, è possibile specificare una lunghezza massima dei dati binari che possono essere conservati in questa colonna. Ad esempio, per riservare al massimo 5.000 byte, usare `varbinary(5000)`; `varbinary(MAX)` consente le dimensioni massime di archiviazione, circa 2 GB.

Il vantaggio principale dell'archiviazione dei dati binari direttamente nel database è il stretto accoppiamento tra i dati binari e il record del database. Questo semplifica notevolmente le attività di amministrazione del database, ad esempio i backup o lo stato di trasferimento del database in un altro sito o server. Inoltre, l'eliminazione di un record comporta l'eliminazione automatica dei dati binari corrispondenti. L'archiviazione dei dati binari nel database presenta anche vantaggi più evidenti. Per una discussione più approfondita, vedere [archiviazione dei file binari direttamente nel database con ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) .

> [!NOTE]
> In Microsoft SQL Server 2000 e versioni precedenti, il tipo di dati `varbinary` ha un limite massimo di 8.000 byte. Per archiviare fino a 2 GB di dati binari, è necessario usare invece [`image` tipo di dati](https://msdn.microsoft.com/library/ms187993.aspx) . Con l'aggiunta di `MAX` in SQL Server 2005, tuttavia, il tipo di dati `image` è stato deprecato. È ancora supportata per la compatibilità con le versioni precedenti, ma Microsoft ha annunciato che il tipo di dati `image` verrà rimosso in una versione futura di SQL Server.

Se si utilizza un modello di dati precedente, è possibile che venga visualizzato il tipo di dati `image`. Nella tabella `Categories` del database Northwind è presente una colonna `Picture` che può essere utilizzata per archiviare i dati binari di un file di immagine per la categoria. Poiché il database Northwind presenta le sue radici in Microsoft Access e nelle versioni precedenti di SQL Server, questa colonna è di tipo `image`.

Per questa esercitazione e per i successivi tre, verranno usati entrambi gli approcci. La tabella `Categories` dispone già di una colonna `Picture` per archiviare il contenuto binario di un'immagine per la categoria. Si aggiungerà una colonna aggiuntiva, `BrochurePath`, per archiviare un percorso di un file PDF sul file system server Web che può essere usato per fornire una panoramica della categoria di qualità stampata.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Passaggio 3: aggiunta della colonna`BrochurePath`alla tabella`Categories`

Attualmente la tabella Categories include solo quattro colonne: `CategoryID`, `CategoryName`, `Description`e `Picture`. Oltre a questi campi, è necessario aggiungerne uno nuovo che faccia riferimento alla brochure della categoria (se ne esiste una). Per aggiungere questa colonna, passare alla Esplora server, eseguire il drill-down nelle tabelle, fare clic con il pulsante destro del mouse sulla tabella `Categories` e scegliere Apri definizione tabella (vedere la figura 5). Se la Esplora server non viene visualizzata, selezionarla scegliendo l'opzione Esplora server dal menu Visualizza oppure premere CTRL + ALT + S.

Aggiungere una nuova colonna `varchar(200)` alla tabella `Categories` denominata `BrochurePath` e consentire `NULL` s e fare clic sull'icona Salva (oppure premere CTRL + S).

[![aggiungere una colonna BrochurePath alla tabella Categories](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: aggiungere una colonna `BrochurePath` alla tabella `Categories` ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Passaggio 4: aggiornamento dell'architettura per l'utilizzo delle colonne`Picture`e`BrochurePath`

Il `CategoriesDataTable` al livello di accesso ai dati (DAL) dispone attualmente di quattro `DataColumn` s definiti: `CategoryID`, `CategoryName`, `Description`e `NumberOfProducts`. Quando l'oggetto DataTable è stato progettato in origine nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) , il `CategoriesDataTable` includeva solo le prime tre colonne. il `NumberOfProducts` colonna è stato aggiunto in [Master/Detail usando un elenco puntato di record master con un'esercitazione di DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) .

Come descritto in *creazione di un livello di accesso ai dati*, le DataTable nel DataSet tipizzato costituiscono gli oggetti business. I TableAdapter sono responsabili della comunicazione con il database e del popolamento degli oggetti business con i risultati della query. Il `CategoriesDataTable` viene popolato dall'`CategoriesTableAdapter`, che presenta tre metodi di recupero dei dati:

- `GetCategories()` esegue la query principale di TableAdapter s e restituisce i campi `CategoryID`, `CategoryName`e `Description` di tutti i record nella tabella `Categories`. La query principale è quella usata dai metodi di `Insert` e `Update` generati automaticamente.
- `GetCategoryByCategoryID(categoryID)` restituisce i campi `CategoryID`, `CategoryName`e `Description` della categoria il cui `CategoryID` è uguale a *CategoryID*.
- `GetCategoriesAndNumberOfProducts()`: restituisce i campi `CategoryID`, `CategoryName`e `Description` per tutti i record nella tabella `Categories`. USA inoltre una sottoquery per restituire il numero di prodotti associati a ogni categoria.

Si noti che nessuna di queste query restituisce la tabella `Categories` s `Picture` o `BrochurePath` colonne; il `CategoriesDataTable` non fornisce `DataColumn` per questi campi. Per usare le proprietà Picture e `BrochurePath`, è necessario prima aggiungerle al `CategoriesDataTable` e quindi aggiornare la classe `CategoriesTableAdapter` per restituire queste colonne.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Aggiunta del`Picture`e della`BrochurePath``DataColumn`

Per iniziare, aggiungere queste due colonne al `CategoriesDataTable`. Fare clic con il pulsante destro del mouse sull'intestazione `CategoriesDataTable`, scegliere Aggiungi dal menu di scelta rapida, quindi scegliere l'opzione colonna. Verrà creato un nuovo `DataColumn` nella DataTable denominata `Column1`. Rinominare la colonna in `Picture`. Dalla Finestra Proprietà impostare la proprietà `DataType` di `DataColumn` s su `System.Byte[]`. questa opzione non è disponibile nell'elenco a discesa. è necessario digitarla.

[![creare un'immagine DataColumn denominata il cui tipo di dati è System. Byte []](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: creare un `DataColumn` denominato `Picture` il cui `DataType` è `System.Byte[]` ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image8.png))

Aggiungere un altro `DataColumn` alla DataTable, denominarlo `BrochurePath` usando il valore predefinito di `DataType` (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Restituzione dei valori`Picture`e`BrochurePath`dal TableAdapter

Con questi due `DataColumn` aggiunti al `CategoriesDataTable`, è possibile aggiornare il `CategoriesTableAdapter`. È possibile che vengano restituiti entrambi i valori di colonna nella query TableAdapter principale, ma ciò comporta la restituzione dei dati binari ogni volta che è stato richiamato il metodo `GetCategories()`. Al contrario, consente di aggiornare la query TableAdapter principale per riportare `BrochurePath` e creare un metodo di recupero dei dati aggiuntivo che restituisca una determinata categoria `Picture` colonna.

Per aggiornare la query TableAdapter principale, fare clic con il pulsante destro del mouse sull'intestazione `CategoriesTableAdapter` s e scegliere l'opzione Configura dal menu di scelta rapida. Viene visualizzata la procedura guidata di configurazione dell'adattatore della tabella, che è stata illustrata in una serie di esercitazioni precedenti. Aggiornare la query per ripristinare il `BrochurePath` e fare clic su fine.

[![aggiornare l'elenco di colonne nell'istruzione SELECT per restituire anche BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: aggiornare l'elenco di colonne nell'istruzione `SELECT` per restituire anche `BrochurePath` ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image10.png))

Quando si usano istruzioni SQL ad hoc per il TableAdapter, l'aggiornamento dell'elenco di colonne nella query principale aggiorna l'elenco di colonne per tutti i `SELECT` metodi di query nell'oggetto TableAdapter. Questo significa che il metodo `GetCategoryByCategoryID(categoryID)` è stato aggiornato per restituire la colonna `BrochurePath`, che potrebbe essere quello previsto. Tuttavia, ha aggiornato anche l'elenco di colonne nel metodo `GetCategoriesAndNumberOfProducts()`, rimuovendo la sottoquery che restituisce il numero di prodotti per ogni categoria. Pertanto, è necessario aggiornare questo metodo s `SELECT` query. Fare clic con il pulsante destro del mouse sul metodo `GetCategoriesAndNumberOfProducts()`, scegliere Configura e ripristinare il valore originale della query `SELECT`:

[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Successivamente, creare un nuovo metodo TableAdapter che restituisca una categoria specifica `Picture` valore della colonna. Fare clic con il pulsante destro del mouse sull'intestazione `CategoriesTableAdapter` s e scegliere l'opzione Aggiungi query per avviare la configurazione guidata query TableAdapter. Il primo passaggio della procedura guidata chiede se si vuole eseguire query sui dati usando un'istruzione SQL ad hoc, un nuovo stored procedure o uno esistente. Selezionare Usa istruzioni SQL e fare clic su Avanti. Poiché verrà restituita una riga, scegliere l'opzione SELECT that returns Rows dal secondo passaggio.

[![selezionare l'opzione Usa istruzioni SQL](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: selezionare l'opzione Usa istruzioni SQL ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image12.png))

[![poiché la query restituirà un record dalla tabella delle categorie, scegliere Seleziona per restituire le righe.](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: poiché la query restituirà un record dalla tabella Categories, scegliere SELECT che restituisce righe ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image14.png))

Nel terzo passaggio, immettere la query SQL seguente e fare clic su Avanti:

[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

L'ultimo passaggio consiste nel scegliere il nome per il nuovo metodo. Usare `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` per compilare un DataTable e restituire rispettivamente i modelli DataTable. Fare clic su Fine per completare la procedura guidata.

[![scegliere i nomi dei metodi TableAdapter](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: scegliere i nomi dei metodi TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image16.png))

> [!NOTE]
> Al termine della procedura guidata per la configurazione della query dell'adattatore della tabella, è possibile che venga visualizzata una finestra di dialogo che informa che il nuovo testo del comando restituisce dati con schema diverso dallo schema della query principale. In breve, la procedura guidata rileva che la query principale TableAdapter s `GetCategories()` restituisce uno schema diverso da quello appena creato. Ma questo è il risultato desiderato, quindi è possibile ignorare questo messaggio.

Tenere inoltre presente che se si utilizzano istruzioni SQL ad hoc e si utilizza la procedura guidata per modificare la query principale di TableAdapter in un momento successivo, verrà modificato l'elenco di colonne dell'istruzione `GetCategoryWithBinaryDataByCategoryID` `SELECT` metodo in modo da includere solo le colonne della query principale, ovvero verrà rimossa la colonna `Picture` dalla query. Sarà necessario aggiornare manualmente l'elenco di colonne per restituire la colonna `Picture`, in modo analogo a quanto è stato fatto con il metodo `GetCategoriesAndNumberOfProducts()` in precedenza in questo passaggio.

Dopo aver aggiunto i due `DataColumn` al `CategoriesDataTable` e il metodo `GetCategoryWithBinaryDataByCategoryID` al `CategoriesTableAdapter`, queste classi nella finestra di progettazione DataSet tipizzata dovrebbero avere un aspetto simile a quello della schermata nella figura 11.

![In Progettazione DataSet sono inclusi i nuovi metodi e colonne](uploading-files-vb/_static/image11.gif)

**Figura 11**: Progettazione DataSet include le nuove colonne e il metodo

## <a name="updating-the-business-logic-layer-bll"></a>Aggiornamento del livello di logica di business (BLL)

Con il DAL aggiornato, tutto ciò che rimane è quello di aumentare il livello di logica di business (BLL) per includere un metodo per il nuovo `CategoriesTableAdapter` metodo. Aggiungere il metodo seguente alla classe `CategoriesBLL`:

[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Passaggio 5: caricamento di un file dal client al server Web

Quando si raccolgono dati binari, spesso questi dati vengono forniti da un utente finale. Per acquisire queste informazioni, l'utente deve essere in grado di caricare un file dal computer nel server Web. I dati caricati devono quindi essere integrati con il modello di dati, che può significare salvare il file nel server Web file system e aggiungere un percorso al file nel database o scrivere il contenuto binario direttamente nel database. In questo passaggio verrà illustrato come consentire a un utente di caricare i file dal computer al server. Nell'esercitazione successiva si rivelerà l'integrazione del file caricato con il modello di dati.

ASP.NET 2,0 s nuovo [controllo Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fornisce un meccanismo per consentire agli utenti di inviare un file dal computer al server Web. Il rendering del controllo FileUpload viene eseguito come elemento `<input>` il cui attributo `type` è impostato su file, che i browser visualizzano come casella di testo con un pulsante Sfoglia. Facendo clic sul pulsante Sfoglia viene visualizzata una finestra di dialogo da cui l'utente può selezionare un file. Quando viene eseguito il postback del modulo, il contenuto del file selezionato viene inviato insieme al postback. Sul lato server, le informazioni sul file caricato sono accessibili tramite le proprietà del controllo FileUpload.

Per illustrare il caricamento di file, aprire la pagina `FileUpload.aspx` nella cartella `BinaryData`, trascinare un controllo FileUpload dalla casella degli strumenti nella finestra di progettazione e impostare la proprietà `ID` del controllo su `UploadTest`. Successivamente, aggiungere un controllo Web Button impostando le proprietà `ID` e `Text` su `UploadButton` e caricare rispettivamente il file selezionato. Inserire infine un controllo Web etichetta sotto il pulsante, deselezionare la relativa proprietà `Text` e impostare la relativa proprietà `ID` su `UploadDetails`.

[![aggiungere un controllo FileUpload alla pagina ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: aggiungere un controllo FileUpload alla pagina ASP.NET ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image18.png))

La figura 13 Mostra questa pagina quando viene visualizzata tramite un browser. Si noti che facendo clic sul pulsante Sfoglia viene visualizzata una finestra di dialogo di selezione file che consente all'utente di selezionare un file dal computer. Una volta selezionato un file, facendo clic sul pulsante Carica file selezionato si crea un postback che invia il contenuto binario del file selezionato al server Web.

[![l'utente può selezionare un file da caricare dal computer al server](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: l'utente può selezionare un file da caricare dal computer al server ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image20.png))

Durante il postback, il file caricato può essere salvato nel file system o i dati binari possono essere usati direttamente tramite un flusso. Per questo esempio, creare una cartella di `~/Brochures` e salvare il file caricato. Per iniziare, aggiungere la cartella `Brochures` al sito come sottocartella della directory radice. Successivamente, creare un gestore eventi per l'evento `Click` `UploadButton` s e aggiungere il codice seguente:

[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Il controllo FileUpload fornisce un'ampia gamma di proprietà per l'utilizzo dei dati caricati. Ad esempio, la [proprietà`HasFile`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se un file è stato caricato dall'utente, mentre la [Proprietà`FileBytes`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornisce l'accesso ai dati binari caricati sotto forma di matrice di byte. Il gestore dell'evento `Click` inizia assicurandosi che un file sia stato caricato. Se un file è stato caricato, l'etichetta Mostra il nome del file caricato, le dimensioni in byte e il tipo di contenuto.

> [!NOTE]
> Per assicurarsi che l'utente carichi un file, è possibile controllare la proprietà `HasFile` e visualizzare un avviso se è `False`oppure è possibile utilizzare il [controllo RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) .

Il `SaveAs(filePath)` FileUpload Salva il file caricato nel *percorso specificato.* *filePath* deve essere un *percorso fisico* (`C:\Websites\Brochures\SomeFile.pdf`) anziché un *percorso* virtuale (`/Brochures/SomeFile.pdf`). Il [metodo`Server.MapPath(virtPath)`](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) accetta un percorso virtuale e restituisce il percorso fisico corrispondente. Il percorso virtuale è `~/Brochures/fileName`, dove *filename* è il nome del file caricato. Per ulteriori informazioni sui percorsi virtuali e fisici e sull'utilizzo di `Server.MapPath`, vedere [using Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) .

Dopo aver completato il gestore dell'evento `Click`, provare a eseguire il test della pagina in un browser. Fare clic sul pulsante Sfoglia e selezionare un file dal disco rigido e quindi fare clic sul pulsante Carica file selezionato. Il postback invierà il contenuto del file selezionato al server Web, che visualizzerà quindi le informazioni sul file prima di salvarlo nella cartella `~/Brochures`. Dopo aver caricato il file, tornare a Visual Studio e fare clic sul pulsante Aggiorna nel Esplora soluzioni. Il file appena caricato verrà visualizzato nella cartella ~/brochures

[![il file EvolutionValley. jpg è stato caricato sul server Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: il file `EvolutionValley.jpg` è stato caricato sul server Web ([fare clic per visualizzare l'immagine con dimensioni complete](uploading-files-vb/_static/image22.png))

![EvolutionValley. jpg è stato salvato nella cartella ~/brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` salvato nella cartella `~/Brochures`

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sottigliezze con il salvataggio dei file caricati nel file System

Esistono diverse sottigliezze che è necessario risolvere quando si salva il caricamento di file nel server Web file system. Prima di tutto, c'è un problema di sicurezza. Per salvare un file nel file system, il contesto di sicurezza in cui è in esecuzione la pagina ASP.NET deve disporre delle autorizzazioni di scrittura. Il server Web di sviluppo ASP.NET viene eseguito nel contesto dell'account utente corrente. Se si utilizza Microsoft s Internet Information Services (IIS) come server Web, il contesto di sicurezza dipende dalla versione di IIS e dalla relativa configurazione.

Un altro problema relativo al salvataggio dei file nella file system si basa sulla denominazione dei file. Attualmente, la pagina Salva tutti i file caricati nella directory `~/Brochures` usando lo stesso nome del file nel computer client. Se l'utente A carica una brochure con il nome `Brochure.pdf`, il file verrà salvato come `~/Brochure/Brochure.pdf`. Cosa accade se successivamente l'utente B carica un file di brochure diverso che ha lo stesso nome file (`Brochure.pdf`)? Con il codice a questo punto, l'utente A verrà sovrascritto con il caricamento dell'utente B.

Sono disponibili diverse tecniche per la risoluzione dei conflitti di nomi di file. Un'opzione consiste nel proibire il caricamento di un file se ne esiste già uno con lo stesso nome. Con questo approccio, quando l'utente B tenta di caricare un file denominato `Brochure.pdf`, il sistema non salva il file e visualizza invece un messaggio che informa l'utente B di rinominare il file e riprovare. Un altro approccio consiste nel salvare il file utilizzando un nome di file univoco, che può essere un [identificatore univoco globale (Guid)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) o il valore delle colonne chiave primaria del record del database corrispondente (presupponendo che il caricamento sia associato a una determinata riga nel modello di dati). Nell'esercitazione successiva verranno esaminate in modo più dettagliato tali opzioni.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Problemi legati a quantità molto elevate di dati binari

In queste esercitazioni si presuppone che i dati binari acquisiti siano di dimensioni modeste. L'utilizzo di quantità molto elevate di file di dati binari che sono costituiti da diversi megabyte o più grandi introduce nuove problemi che esulano dall'ambito di queste esercitazioni. Per impostazione predefinita, ad esempio, ASP.NET rifiuterà il caricamento di più di 4 MB, sebbene possa essere configurato tramite l' [elemento`<httpRuntime>`](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`. IIS impone anche le proprie limitazioni relative alle dimensioni di caricamento dei file. Per ulteriori informazioni, vedere le [dimensioni del file di caricamento IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) . Inoltre, il tempo impiegato per caricare file di grandi dimensioni potrebbe superare il valore predefinito di 110 secondi. ASP.NET sarà in attesa di una richiesta. Si verificano inoltre problemi di memoria e prestazioni che si verificano quando si utilizzano file di grandi dimensioni.

Il controllo FileUpload non è pratico per i caricamenti di file di grandi dimensioni. Quando il contenuto del file viene inviato al server, è necessario che l'utente finale attenda pazientemente senza alcuna conferma di avanzamento del caricamento. Si tratta di un problema che si verifica quando si gestiscono file più piccoli che possono essere caricati in pochi secondi, ma possono costituire un problema quando si gestiscono file di dimensioni maggiori che potrebbero richiedere minuti di caricamento. Sono disponibili diversi controlli per il caricamento di file di terze parti che sono più adatti per la gestione di carichi di grandi dimensioni e molti di questi fornitori forniscono indicatori di stato e gestori di caricamento ActiveX che presentano un'esperienza utente molto più levigata.

Se l'applicazione deve gestire file di grandi dimensioni, sarà necessario esaminare attentamente le esigenze e individuare le soluzioni appropriate per le proprie esigenze specifiche.

## <a name="summary"></a>Riepilogo

La creazione di un'applicazione che deve acquisire dati binari introduce una serie di problemi. In questa esercitazione sono state esaminate le prime due posizioni: decidere dove archiviare i dati binari e consentire a un utente di caricare contenuto binario tramite una pagina Web. Nelle prossime tre esercitazioni verrà illustrato come associare i dati caricati a un record nel database e come visualizzare i dati binari insieme ai campi dati di testo.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Utilizzo di tipi di dati con valori di grandi dimensioni](https://msdn.microsoft.com/library/ms178158.aspx)
- [Guide introduttive ai controlli FileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Controllo server ASP.NET 2,0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Lato oscuro dei caricamenti di file](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Bernadette Leigh. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](updating-and-deleting-existing-binary-data-cs.md)
> [Successivo](displaying-binary-data-in-the-data-web-controls-vb.md)
