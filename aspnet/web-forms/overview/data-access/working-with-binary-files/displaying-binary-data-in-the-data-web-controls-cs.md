---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Visualizzazione di dati binari nei controlli Web dei datiC#() | Microsoft Docs
author: rick-anderson
description: In questa esercitazione vengono esaminate le opzioni per presentare i dati binari in una pagina Web, inclusa la visualizzazione di un file di immagine e il provisioning di un collegamento "download" f...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: f38de7adcd77b3dc2622759646168cf533b8308f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642515"
---
# <a name="displaying-binary-data-in-the-data-web-controls-c"></a>Visualizzazione di dati binari nei controlli Web dei dati (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) o [scaricare il file PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> In questa esercitazione vengono esaminate le opzioni per presentare i dati binari in una pagina Web, inclusa la visualizzazione di un file di immagine e il provisioning di un collegamento "download" per un file PDF.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente sono state illustrate le due tecniche per associare i dati binari a un modello di dati sottostante dell'applicazione e si è usato il controllo FileUpload per caricare file da un browser all'file system del server Web. È ancora possibile vedere come associare i dati binari caricati al modello di dati. Ovvero, dopo il caricamento e il salvataggio di un file nella file system, un percorso del file deve essere archiviato nel record di database appropriato. Se i dati vengono archiviati direttamente nel database, i dati binari caricati non devono essere salvati nel file system, ma devono essere inseriti nel database.

Prima di esaminare l'associazione dei dati al modello di dati, tuttavia, è necessario prima esaminare come fornire i dati binari all'utente finale. La presentazione di dati di testo è abbastanza semplice, ma in che modo devono essere presentati i dati binari? Dipende, ovviamente, dal tipo di dati binari. Per le immagini, è probabile che si desideri visualizzare l'immagine; per i file PDF, i documenti di Microsoft Word, i file ZIP e altri tipi di dati binari, è probabilmente più appropriato fornire un collegamento per il download.

In questa esercitazione verrà illustrato come presentare i dati binari insieme ai dati di testo associati usando i controlli Web di dati come GridView e DetailsView. Nell'esercitazione successiva verrà rivolto l'associazione di un file caricato al database.

## <a name="step-1-providingbrochurepathvalues"></a>Passaggio 1: fornire valori`BrochurePath`

Nella colonna `Picture` della tabella `Categories` sono già contenuti dati binari per le varie immagini di categoria. In particolare, la colonna `Picture` per ogni record contiene il contenuto binario di un'immagine bitmap a 16 colori granulare, di bassa qualità. Ogni immagine di categoria è 172 pixel di larghezza e 120 pixel di altezza e utilizza circa 11 KB. Per ulteriori informazioni, il contenuto binario nella colonna `Picture` include un'intestazione [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) a 78 byte che deve essere rimossa prima di visualizzare l'immagine. Queste informazioni di intestazione sono presenti perché il database Northwind presenta le sue radici in Microsoft Access. In Access, i dati binari vengono archiviati utilizzando il tipo di dati Object OLE, che viene aggiunto a questa intestazione. Per il momento, verrà illustrato come rimuovere le intestazioni da queste immagini di bassa qualità per visualizzare l'immagine. In un'esercitazione futura verrà compilata un'interfaccia per l'aggiornamento di una colonna `Picture` di categoria e le immagini bitmap che usano le intestazioni OLE con immagini JPG equivalenti senza intestazioni OLE non necessarie.

Nell'esercitazione precedente è stato illustrato come usare il controllo FileUpload. Pertanto, è possibile procedere e aggiungere i file della brochure al file system del server Web. Questa operazione, tuttavia, non aggiorna la colonna `BrochurePath` nella tabella `Categories`. Nell'esercitazione successiva verrà illustrato come eseguire questa operazione, ma per ora è necessario specificare manualmente i valori per questa colonna.

In questo download di questa esercitazione sono disponibili sette file di brochure PDF nella cartella `~/Brochures`, uno per ogni categoria eccetto Seafood. Ho omesso intenzionalmente di aggiungere una brochure a frutti di pesce per illustrare come gestire gli scenari in cui non tutti i record contengono dati binari associati. Per aggiornare la tabella `Categories` con questi valori, fare clic con il pulsante destro del mouse sul nodo `Categories` da Esplora server e scegliere Mostra dati tabella. Quindi, immettere i percorsi virtuali dei file della brochure per ogni categoria con una brochure, come illustrato nella figura 1. Poiché non è disponibile alcuna brochure per la categoria Seafood, lasciare il relativo valore `BrochurePath` colonna come `NULL`.

[![immettere manualmente i valori per la colonna BrochurePath della tabella Categories](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Figura 1**: immettere manualmente i valori per la colonna `BrochurePath` della tabella `Categories` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Passaggio 2: fornire un collegamento di download per le brochure in un controllo GridView

Con i valori `BrochurePath` specificati per la tabella `Categories`, è possibile creare un controllo GridView che elenca ogni categoria insieme a un collegamento per scaricare la brochure di categoria. Nel passaggio 4 si estenderà questo GridView per visualizzare anche l'immagine della categoria.

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione della pagina `DisplayOrDownloadData.aspx` nella cartella `BinaryData`. Impostare il `ID` di GridView su `Categories` e tramite lo smart tag di GridView, scegliere di associarlo a una nuova origine dati. In particolare, è necessario associarlo a un ObjectDataSource denominato `CategoriesDataSource` che recupera i dati utilizzando il metodo `CategoriesBLL` `GetCategories()` oggetto s.

[![creare un nuovo ObjectDataSource denominato CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))

[![configurare ObjectDataSource per l'utilizzo della classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per l'uso della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))

[![recuperare l'elenco di categorie utilizzando il metodo GetCategories ()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Figura 4**: recuperare l'elenco di categorie usando il metodo `GetCategories()` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio aggiungerà automaticamente un BoundField al `Categories` GridView per il `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath` `DataColumn` s. Procedere e rimuovere il `NumberOfProducts` BoundField poiché la query del metodo `GetCategories()` non recupera queste informazioni. Rimuovere anche il BoundField `CategoryID` e rinominare i `CategoryName` e `BrochurePath` i BoundField `HeaderText` proprietà rispettivamente in Category e brochure. Dopo aver apportato queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Visualizzare questa pagina tramite un browser (vedere la figura 5). Ognuna delle otto categorie è elencata. Le sette categorie con valori `BrochurePath` hanno il valore `BrochurePath` visualizzato nel rispettivo BoundField. Seafood, che dispone di un valore `NULL` per la relativa `BrochurePath`, Visualizza una cella vuota.

[![viene elencato il nome, la descrizione e il valore BrochurePath di ogni categoria.](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Figura 5**: sono elencati il nome, la descrizione e il valore `BrochurePath` di ogni categoria ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))

Anziché visualizzare il testo della colonna `BrochurePath`, è necessario creare un collegamento alla brochure. A tale scopo, rimuovere il `BrochurePath` BoundField e sostituirlo con un HyperLinkField. Impostare la proprietà New HyperLinkField s `HeaderText` su brochure, la relativa proprietà `Text` per visualizzare la brochure e la relativa proprietà `DataNavigateUrlFields` su `BrochurePath`.

![Aggiungere un HyperLinkField per BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Figura 6**: aggiungere un HyperLinkField per `BrochurePath`

Verrà aggiunta una colonna di collegamenti a GridView, come illustrato nella figura 7. Se si fa clic su un collegamento Visualizza brochure, il file PDF verrà visualizzato direttamente nel browser o verrà richiesto all'utente di scaricare il file, a seconda che sia installato un lettore PDF e le impostazioni del browser.

[![è possibile visualizzare la brochure di una categoria facendo clic sul collegamento Visualizza brochure](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Figura 7**: è possibile visualizzare la brochure di una categoria facendo clic sul collegamento Visualizza brochure ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))

[![viene visualizzata la brochure della categoria PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Figura 8**: la brochure della categoria s PDF viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Nascondere il testo della brochure di visualizzazione per le categorie senza una brochure

Come illustrato nella figura 7, il `BrochurePath` HyperLinkField Visualizza il valore della proprietà `Text` (View brochure) per tutti i record, indipendentemente dal fatto che esista un valore non`NULL` per `BrochurePath`. Naturalmente, se `BrochurePath` è `NULL`, il collegamento viene visualizzato solo come testo, come nel caso della categoria Seafood (vedere di nuovo la figura 7). Anziché visualizzare la brochure di visualizzazione di testo, potrebbe essere utile fare in modo che le categorie senza un valore `BrochurePath` visualizzino un testo alternativo, ad esempio nessuna brochure disponibile.

Per fornire questo comportamento, è necessario usare un TemplateField il cui contenuto viene generato tramite una chiamata a un metodo di pagina che genera l'output appropriato in base al valore `BrochurePath`. Per prima cosa è stata esaminata questa tecnica di formattazione nell'esercitazione [using TemplateFields nell'esercitazione sul controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Trasformare il HyperLinkField in un TemplateField selezionando il `BrochurePath` HyperLinkField e quindi facendo clic sul collegamento Converti questo campo in un TemplateField nella finestra di dialogo Modifica colonne.

![Convertire HyperLinkField in TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Figura 9**: convertire il HyperLinkField in un TemplateField

Verrà creato un TemplateField con un `ItemTemplate` contenente un controllo Web collegamento ipertestuale la cui proprietà `NavigateUrl` è associata al valore di `BrochurePath`. Sostituire questo markup con una chiamata al metodo `GenerateBrochureLink`, passando il valore di `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Successivamente, creare un metodo di `protected` nella classe code-behind della pagina ASP.NET denominata `GenerateBrochureLink` che restituisce un `string` e accetta un `object` come parametro di input.

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Questo metodo determina se il valore `object` passato è un database `NULL` e, in caso affermativo, restituisce un messaggio che indica che la categoria non dispone di una brochure. In caso contrario, se è presente un valore `BrochurePath`, viene visualizzato in un collegamento ipertestuale. Si noti che se il valore `BrochurePath` è presente, viene passato nel [metodo`ResolveUrl(url)`](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Questo metodo risolve l' *URL*passato, sostituendo il carattere `~` con il percorso virtuale appropriato. Se, ad esempio, l'applicazione è radicata in `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` restituirà `/Tutorial55/Brochures/Meat.pdf`.

La figura 10 Mostra la pagina dopo l'applicazione delle modifiche. Si noti che il campo `BrochurePath` della categoria Seafood ora Visualizza il testo non è disponibile alcuna brochure.

[![viene visualizzato il testo nessuna brochure disponibile per le categorie senza una brochure](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Figura 10**: il testo nessuna brochure disponibile viene visualizzato per le categorie senza una brochure ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Passaggio 3: aggiunta di una pagina Web per visualizzare un'immagine di categoria

Quando un utente visita una pagina ASP.NET, riceve il codice HTML della pagina ASP.NET. Il codice HTML ricevuto è semplicemente testo e non contiene dati binari. Eventuali dati binari aggiuntivi, ad esempio immagini, file audio, Macromedia Flash applicazioni, video incorporati di Windows Media Player e così via, sono disponibili come risorse separate sul server Web. Il codice HTML contiene riferimenti a questi file, ma non include il contenuto effettivo dei file.

Ad esempio, in HTML l'elemento `<img>` viene usato per fare riferimento a un'immagine, con l'attributo `src` che punta al file di immagine come segue:

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Quando un browser riceve questo codice HTML, effettua un'altra richiesta al server Web per recuperare il contenuto binario del file di immagine, che viene quindi visualizzato nel browser. Lo stesso concetto è applicabile a qualsiasi dato binario. Nel passaggio 2 la brochure non è stata inviata al browser come parte del markup HTML della pagina. Piuttosto, i collegamenti ipertestuali forniti da HTML che, quando si fa clic, hanno causato la richiesta diretta del documento PDF da parte del browser.

Per visualizzare o consentire agli utenti di scaricare dati binari che si trovano all'interno del database, è necessario creare una pagina Web separata che restituisca i dati. Per l'applicazione, c'è un solo campo dati binario archiviato direttamente nel database dell'immagine della categoria. Pertanto, è necessaria una pagina che, quando chiamata, restituisca i dati dell'immagine per una determinata categoria.

Aggiungere una nuova pagina ASP.NET alla cartella `BinaryData` denominata `DisplayCategoryPicture.aspx`. Quando si esegue questa operazione, lasciare deselezionata la casella di controllo Seleziona pagina master. Questa pagina prevede un valore `CategoryID` in QueryString e restituisce i dati binari della colonna `Picture` della categoria. Poiché in questa pagina vengono restituiti dati binari e nient'altro, non è necessario alcun markup nella sezione HTML. Quindi, fare clic sulla scheda origine nell'angolo in basso a sinistra e rimuovere tutto il markup della pagina, ad eccezione della direttiva `<%@ Page %>`. Ovvero il markup dichiarativo `DisplayCategoryPicture.aspx` s deve essere costituito da una sola riga:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Se viene visualizzato l'attributo `MasterPageFile` nella direttiva `<%@ Page %>`, rimuoverlo.

Nella classe code-behind della pagina aggiungere il codice seguente al gestore dell'evento `Page_Load`:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Questo codice inizia leggendo il `CategoryID` valore querystring in una variabile denominata `categoryID`. Successivamente, i dati dell'immagine vengono recuperati tramite una chiamata al metodo `GetCategoryWithBinaryDataByCategoryID(categoryID)` della classe `CategoriesBLL`. Questi dati vengono restituiti al client tramite il metodo `Response.BinaryWrite(data)`, ma prima che venga chiamato, è necessario rimuovere l'intestazione OLE del valore della colonna `Picture`. Questa operazione viene eseguita creando una matrice di `byte` denominata `strippedImageData` che conterrà esattamente 78 caratteri inferiori rispetto a quelli presenti nella colonna `Picture`. Il [metodo`Array.Copy`](https://msdn.microsoft.com/library/z50k9bft.aspx) viene usato per copiare i dati da `category.Picture` a partire dalla posizione 78 al `strippedImageData`.

La proprietà `Response.ContentType` specifica il [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenuto restituito, in modo che il browser sappia come eseguirne il rendering. Poiché la colonna `Picture` `Categories` Table è un'immagine bitmap, qui viene usato il tipo MIME bitmap (Image/BMP). Se si omette il tipo MIME, la maggior parte dei browser visualizzerà l'immagine correttamente perché può dedurre il tipo in base al contenuto dei dati binari del file di immagine. Tuttavia, è prudente includere il tipo MIME quando possibile. Per un elenco completo dei [tipi di supporti MIME](http://www.iana.org/assignments/media-types/), vedere il [sito Web Internet Assigned Numbers Authority](http://www.iana.org/) .

Quando questa pagina è stata creata, è possibile visualizzare un'immagine specifica della categoria visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Nella figura 11 è illustrata l'immagine della categoria bevande, che può essere visualizzata da `DisplayCategoryPicture.aspx?CategoryID=1`.

[![viene visualizzata l'immagine della categoria bevande](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Figura 11**: viene visualizzata l'immagine della categoria bevande ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))

Se, durante la visita di `DisplayCategoryPicture.aspx?CategoryID=categoryID`, viene generata un'eccezione che indica che non è possibile eseguire il cast dell'oggetto di tipo ' System. DBNull ' al tipo ' System. Byte []'. Esistono due elementi che potrebbero causare questa situazione. Per prima cosa, la colonna `Picture` della tabella `Categories` consente `NULL` valori. La pagina `DisplayCategoryPicture.aspx`, tuttavia, presuppone che sia presente un valore non`NULL`. Non è possibile accedere direttamente alla proprietà `Picture` della `CategoriesDataTable` se è presente un valore di `NULL`. Se si desidera consentire `NULL` valori per la colonna `Picture`, è necessario includere la condizione seguente:

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Il codice precedente presuppone che esista un file di immagine denominato `NoPictureAvailable.gif` nella cartella `Images` che si desidera visualizzare per tali categorie senza un'immagine.

Questa eccezione può essere causata anche se l'istruzione `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT` è stata ripristinata nell'elenco di colonne della query principale, che può verificarsi se si usano istruzioni SQL ad hoc ed è necessario eseguire di nuovo la procedura guidata per la query principale di TableAdapter. Verificare che l'istruzione `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT` includa ancora la colonna `Picture`.

> [!NOTE]
> Ogni volta che viene visitata la `DisplayCategoryPicture.aspx`, viene eseguito l'accesso al database e vengono restituiti i dati dell'immagine della categoria specificata. Se l'immagine della categoria non è stata modificata dopo l'ultima visualizzazione da parte dell'utente, si tratta di un'operazione inutile. Fortunatamente, HTTP consente l' *ottenere condizionale*. Con un'operazione GET condizionale, il client che effettua la richiesta HTTP invia una [`If-Modified-Since` intestazione HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) che fornisce la data e l'ora dell'ultima volta in cui il client ha recuperato questa risorsa dal server Web. Se il contenuto non è stato modificato dopo la data specificata, il server Web può rispondere con un [codice di stato non modificato (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) e rinunciare a restituire il contenuto della risorsa richiesta. In breve, questa tecnica evita al server Web di dover restituire il contenuto di una risorsa se non è stato modificato dall'ultimo accesso del client.

Per implementare questo comportamento, tuttavia, richiede l'aggiunta di una colonna `PictureLastModified` alla tabella `Categories` per acquisire la data dell'ultimo aggiornamento della colonna `Picture`, nonché il codice per verificare l'intestazione `If-Modified-Since`. Per ulteriori informazioni sull'intestazione `If-Modified-Since` e sul flusso di lavoro GET condizionale, vedere [HTTP GET condizionale per gli hacker RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [un'analisi più approfondita dell'esecuzione di richieste HTTP in una pagina ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Passaggio 4: visualizzazione delle immagini delle categorie in un controllo GridView

Ora che è disponibile una pagina Web per visualizzare un'immagine di categoria specifica, è possibile visualizzarla usando il [controllo Web Image](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o un elemento `<img>` HTML che punta a `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Le immagini il cui URL è determinato dai dati del database possono essere visualizzate in GridView o DetailsView usando ImageField. Il ImageField contiene `DataImageUrlField` e `DataImageUrlFormatString` proprietà che funzionano come HyperLinkField s `DataNavigateUrlFields` e `DataNavigateUrlFormatString` proprietà.

Consente di aumentare la `Categories` GridView in `DisplayOrDownloadData.aspx` aggiungendo un ImageField per visualizzare l'immagine di ogni categoria. È sufficiente aggiungere il ImageField e impostare le proprietà `DataImageUrlField` e `DataImageUrlFormatString` rispettivamente su `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`. Verrà creata una colonna GridView che esegue il rendering di un elemento `<img>` il cui attributo `src` fa riferimento `DisplayCategoryPicture.aspx?CategoryID={0}`, dove {0} viene sostituito con il valore `CategoryID` della riga GridView.

![Aggiungere un ImageField a GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Figura 12**: aggiungere un ImageField a GridView

Dopo aver aggiunto il ImageField, la sintassi dichiarativa di GridView sarà simile a soothe:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Per visualizzare questa pagina, è possibile usare un browser. Si noti che ogni record include ora un'immagine per la categoria.

[![l'immagine della categoria viene visualizzata per ogni riga](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Figura 13**: l'immagine della categoria viene visualizzata per ogni riga ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato esaminato come presentare i dati binari. Il modo in cui vengono presentati i dati dipende dal tipo di dati. Per i file della brochure PDF è stato offerto un collegamento alla brochure di visualizzazione che, quando selezionato, ha portato l'utente direttamente al file PDF. Per l'immagine della categoria, è stata creata prima di tutto una pagina per recuperare e restituire i dati binari dal database e quindi utilizzare tale pagina per visualizzare l'immagine di ogni categoria in GridView.

Ora che si è appreso come visualizzare i dati binari, è possibile esaminare come eseguire operazioni di inserimento, aggiornamento ed eliminazione sul database con i dati binari. Nell'esercitazione successiva si esaminerà come associare un file caricato al record del database corrispondente. In seguito, nell'esercitazione verrà illustrato come aggiornare i dati binari esistenti e come eliminare i dati binari quando viene rimosso il record associato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Dave Gardner. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](uploading-files-cs.md)
> [Successivo](including-a-file-upload-option-when-adding-a-new-record-cs.md)
