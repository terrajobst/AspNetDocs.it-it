---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Visualizzazione dei dati con ObjectDataSource (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene esaminato il controllo ObjectDataSource utilizzando questo controllo è possibile associare i dati recuperati dall'oggetto BLL creato nell'esercitazione precedente senza havi...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 9419504ace15b39c35a034dda22f2700ee720157
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577476"
---
# <a name="displaying-data-with-the-objectdatasource-c"></a>Visualizzazione dei dati con ObjectDataSource (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) o [scaricare il file PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> In questa esercitazione viene esaminato il controllo ObjectDataSource utilizzando questo controllo è possibile associare i dati recuperati dall'oggetto BLL creato nell'esercitazione precedente senza dover scrivere una riga di codice.

## <a name="introduction"></a>Introduzione

Con l'architettura dell'applicazione e il layout della pagina del sito Web completati, siamo pronti per iniziare a scoprire come eseguire un'ampia gamma di attività comuni correlate ai dati e alla creazione di report. Nelle esercitazioni precedenti è stato illustrato come associare a livello di codice i dati da DAL e BLL a un controllo Web dati in una pagina ASP.NET. Questa sintassi che assegna la proprietà `DataSource` del controllo Web dati ai dati da visualizzare e quindi chiamare il metodo di `DataBind()` del controllo è il modello usato nelle applicazioni ASP.NET 1. x e può continuare a essere usato nelle applicazioni 2,0. Tuttavia, i nuovi controlli origine dati di ASP.NET 2.0 offrono una modalità dichiarativa per lavorare con i dati. Con questi controlli è possibile associare i dati recuperati dal BLL creato nell' [esercitazione precedente](../introduction/creating-a-business-logic-layer-cs.md) senza dover scrivere una riga di codice.

ASP.NET 2,0 viene fornito con cinque controlli origine dati predefiniti [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx)e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) , sebbene sia possibile creare [controlli origine dati personalizzati](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), se necessario. Poiché è stata sviluppata un'architettura per l'applicazione di esercitazione, si userà ObjectDataSource con le classi BLL.

![ASP.NET 2,0 include cinque controlli origine dati incorporati](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: ASP.NET 2,0 include cinque controlli dell'origine dati incorporati

ObjectDataSource funge da proxy per l'utilizzo di un altro oggetto. Per configurare ObjectDataSource, è necessario specificare questo oggetto sottostante e il modo in cui i relativi metodi vengono mappati ai metodi `Select`, `Insert`, `Update`e `Delete` di ObjectDataSource. Una volta specificato questo oggetto sottostante e i relativi metodi mappati a ObjectDataSource, è possibile associare ObjectDataSource a un controllo Web dati. ASP.NET viene fornito con molti controlli Web di dati, tra cui GridView, DetailsView, RadioButtonList ed DropDownList. Durante il ciclo di vita della pagina, il controllo Web dei dati potrebbe dover accedere ai dati a cui è associato, operazione che verrà eseguita richiamando il metodo `Select` di ObjectDataSource; Se il controllo Web dei dati supporta l'inserimento, l'aggiornamento o l'eliminazione, è possibile che vengano effettuate chiamate ai metodi `Insert`, `Update`o `Delete` di ObjectDataSource. Queste chiamate vengono quindi instradate da ObjectDataSource ai metodi dell'oggetto sottostante appropriato, come illustrato nel diagramma seguente.

[![ObjectDataSource funge da proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: ObjectDataSource funge da proxy ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image4.png))

Sebbene sia possibile utilizzare ObjectDataSource per richiamare metodi per l'inserimento, l'aggiornamento o l'eliminazione di dati, è sufficiente concentrarsi sulla restituzione di dati; le esercitazioni future esamineranno l'utilizzo di ObjectDataSource e dei controlli Web dati che modificano i dati.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 1: aggiunta e configurazione del controllo ObjectDataSource

Per iniziare, aprire la pagina `SimpleDisplay.aspx` nella cartella `BasicReporting`, passare alla visualizzazione progettazione, quindi trascinare un controllo ObjectDataSource dalla casella degli strumenti nell'area di progettazione della pagina. ObjectDataSource viene visualizzato come una casella grigia nell'area di progettazione perché non produce alcun markup; accede semplicemente ai dati richiamando un metodo da un oggetto specificato. I dati restituiti da ObjectDataSource possono essere visualizzati da un controllo Web dati, ad esempio GridView, DetailsView, FormView e così via.

> [!NOTE]
> In alternativa, è possibile aggiungere prima il controllo Web dati alla pagina e quindi, dallo smart tag, scegliere l'opzione &lt;nuova origine dati&gt; dall'elenco a discesa.

Per specificare l'oggetto sottostante di ObjectDataSource e il modo in cui i metodi dell'oggetto vengono mappati a ObjectDataSource, fare clic sul collegamento Configura origine dati dallo smart tag di ObjectDataSource.

[![fare clic sul collegamento Configura origine dati dallo smart tag](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: fare clic sul collegamento Configura origine dati dallo smart tag ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image7.png))

Viene visualizzata la procedura guidata Configura origine dati. In primo luogo, è necessario specificare l'oggetto che ObjectDataSource deve usare. Se è selezionata la casella di controllo "Mostra solo componenti dati", l'elenco a discesa in questa schermata elenca solo gli oggetti che sono stati decorati con l'attributo `DataObject`. Attualmente l'elenco include gli oggetti TableAdapter nel DataSet tipizzato e le classi BLL create nell'esercitazione precedente. Se si è dimenticato di aggiungere l'attributo `DataObject` alle classi del livello della logica di business, queste non verranno visualizzate in questo elenco. In tal caso, deselezionare la casella di controllo "Mostra solo componenti dati" per visualizzare tutti gli oggetti, che devono includere le classi BLL, insieme alle altre classi del DataSet tipizzato DataTables, DataRows e così via.

Da questa prima schermata scegliere la classe `ProductsBLL` dall'elenco a discesa e fare clic su Avanti.

[![specificare l'oggetto da utilizzare con il controllo ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Figura 4**: specificare l'oggetto da usare con il controllo ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image10.png))

Nella schermata successiva della procedura guidata viene richiesto di selezionare il metodo che ObjectDataSource deve richiamare. Nell'elenco a discesa sono elencati i metodi che restituiscono i dati nell'oggetto selezionato nella schermata precedente. Qui vengono visualizzati `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`e `GetProductsBySupplierID`. Selezionare il metodo `GetProducts` dall'elenco a discesa e fare clic su fine (se è stato aggiunto il `DataObjectMethodAttribute` ai metodi del `ProductBLL`, come illustrato nell'esercitazione precedente, questa opzione sarà selezionata per impostazione predefinita).

[![scegliere il metodo per la restituzione dei dati dalla scheda Seleziona](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Figura 5**: scegliere il metodo per la restituzione dei dati dalla scheda Seleziona ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Configurare ObjectDataSource manualmente

La configurazione guidata origine dati di ObjectDataSource offre un modo rapido per specificare l'oggetto utilizzato e per associare i metodi dell'oggetto richiamati. È tuttavia possibile configurare ObjectDataSource tramite le relative proprietà, tramite il Finestra Proprietà o direttamente nel markup dichiarativo. È sufficiente impostare la proprietà `TypeName` sul tipo di oggetto sottostante da utilizzare e il `SelectMethod` al metodo da richiamare durante il recupero dei dati.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Anche se si preferisce la configurazione guidata origine dati, a volte può essere necessario configurare manualmente ObjectDataSource, perché la procedura guidata elenca solo le classi create dallo sviluppatore. Se si vuole associare ObjectDataSource a una classe nel .NET Framework come la [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), per accedere alle informazioni sull'account utente o alla [classe di directory](https://msdn.microsoft.com/library/system.io.directory.aspx) per usare le informazioni di file System, è necessario impostare manualmente le proprietà di ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Passaggio 2: aggiungere un controllo Web dati e associarlo a ObjectDataSource

Una volta che ObjectDataSource è stato aggiunto alla pagina e configurato, è possibile aggiungere i controlli Web di dati alla pagina per visualizzare i dati restituiti dal metodo `Select` di ObjectDataSource. Qualsiasi controllo Web dei dati può essere associato a un ObjectDataSource; verranno ora esaminati i dati di ObjectDataSource in GridView, DetailsView e FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Associazione di un controllo GridView a ObjectDataSource

Aggiungere un controllo GridView dalla casella degli strumenti all'area di progettazione del `SimpleDisplay.aspx`. Dallo smart tag di GridView, scegliere il controllo ObjectDataSource aggiunto nel passaggio 1. Verrà creato automaticamente un BoundField in GridView per ogni proprietà restituita dai dati del metodo `Select` di ObjectDataSource, ovvero le proprietà definite dal DataTable Products.

[![è stato aggiunto un controllo GridView alla pagina ed è stato associato a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Figura 6**: è stato aggiunto un controllo GridView alla pagina ed è stato associato a ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image16.png))

È quindi possibile personalizzare, ridisporre o rimuovere i BoundField di GridView facendo clic sull'opzione modifica colonne dallo smart tag.

[![gestire i BoundField di GridView tramite la finestra di dialogo Modifica colonne](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Figura 7**: gestire i BoundField di GridView tramite la finestra di dialogo Modifica colonne ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image19.png))

Modificare i BoundField di GridView, rimuovendo i BoundField `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel`. È sufficiente selezionare il BoundField nell'elenco in basso a sinistra e fare clic sul pulsante Elimina (la X rossa) per rimuoverli. Successivamente, ridisporre i BoundField in modo che i BoundField `CategoryName` e `SupplierName` precedano il `UnitPrice` BoundField selezionando i BoundField e facendo clic sulla freccia su. Impostare le proprietà `HeaderText` dei BoundField rimanenti rispettivamente su `Products`, `Category`, `Supplier`e `Price`. Quindi, fare in modo che il BoundField `Price` venga formattato come valuta impostando la proprietà `HtmlEncode` di BoundField su false e la relativa proprietà `DataFormatString` su `{0:c}`. Infine, allineare orizzontalmente il `Price` a destra e la casella di controllo `Discontinued` al centro tramite la proprietà `ItemStyle`/`HorizontalAlign`.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

[![i BoundField di GridView sono stati personalizzati](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Figura 8**: i BoundField di GridView sono stati personalizzati ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Uso dei temi per un aspetto coerente

Queste esercitazioni tentano di rimuovere tutte le impostazioni di stile a livello di controllo, usando invece fogli di stile CSS definiti in un file esterno, laddove possibile. Il file di `Styles.css` contiene `DataWebControlStyle`, `HeaderStyle`, `RowStyle`e `AlternatingRowStyle` classi CSS che devono essere utilizzate per determinare l'aspetto dei controlli Web utilizzati in queste esercitazioni. A tale scopo, è possibile impostare la proprietà `CssClass` di GridView su `DataWebControlStyle`e le proprietà `HeaderStyle`, `RowStyle`e `AlternatingRowStyle` `CssClass` di conseguenza.

Se si impostano queste proprietà `CssClass` sul controllo Web, è necessario ricordarsi di impostare in modo esplicito questi valori di proprietà per ogni controllo Web dati aggiunto alle esercitazioni. Un approccio più gestibile consiste nel definire le proprietà predefinite correlate a CSS per i controlli GridView, DetailsView e FormView usando un tema. Un tema è una raccolta di impostazioni delle proprietà a livello di controllo, immagini e classi CSS che è possibile applicare alle pagine in un sito per applicare un aspetto comune.

Il tema non includerà immagini o file CSS (il foglio di stile verrà lasciato invariato `Styles.css`, definito nella cartella radice dell'applicazione Web), ma includerà due interfacce. Un'interfaccia è un file che definisce le proprietà predefinite per un controllo Web. In particolare, sarà presente un file di interfaccia per i controlli GridView e DetailsView, che indica le proprietà predefinite correlate al `CssClass`.

Per iniziare, aggiungere un nuovo file di interfaccia al progetto denominato `GridView.skin` facendo clic con il pulsante destro del mouse sul nome del progetto nel Esplora soluzioni e scegliendo Aggiungi nuovo elemento.

[![aggiungere un file di interfaccia con nome GridView. Skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Figura 9**: aggiungere un file di interfaccia denominato `GridView.skin` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image25.png))

I file di interfaccia devono essere inseriti in un tema, che si trovano nella cartella `App_Themes`. Poiché non si dispone ancora di una cartella di questo tipo, in Visual Studio viene gentilmente offerto di crearne una per l'aggiunta della prima interfaccia. Fare clic su Sì per creare la cartella `App_Theme` e inserire il nuovo file di `GridView.skin`.

[![consentire a Visual Studio di creare la cartella App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Figura 10**: consentire a Visual Studio di creare la cartella `App_Theme` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image28.png))

Verrà creato un nuovo tema nella cartella `App_Themes` denominato GridView con il file skin `GridView.skin`.

![Il tema GridView è stato aggiunto alla cartella App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Figura 11**: il tema GridView è stato aggiunto alla cartella `App_Theme`

Rinominare il tema GridView in DataWebControls (fare clic con il pulsante destro del mouse sulla cartella GridView nella cartella `App_Theme` e scegliere Rinomina). Immettere quindi il markup seguente nel file di `GridView.skin`:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Definisce le proprietà predefinite per le proprietà correlate a `CssClass`per qualsiasi GridView in qualsiasi pagina che usa il tema DataWebControls. Verrà ora aggiunta un'altra interfaccia per DetailsView, un controllo Web dati che verrà usato a breve. Aggiungere una nuova interfaccia al tema DataWebControls denominato `DetailsView.skin` e aggiungere il markup seguente:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Con il tema definito, l'ultimo passaggio consiste nell'applicare il tema alla pagina ASP.NET. Un tema può essere applicato in base a una pagina per pagina o a tutte le pagine di un sito Web. Questo tema verrà usato per tutte le pagine del sito Web. A tale scopo, aggiungere il markup seguente alla sezione `<system.web>` di `Web.config`:

[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

Questo è tutto. L'impostazione `styleSheetTheme` indica che le proprietà specificate nel tema *non* devono eseguire l'override delle proprietà specificate a livello di controllo. Per specificare che le impostazioni del tema devono avere le impostazioni di controllo Trump, usare l'attributo `theme` al posto di `styleSheetTheme`; Sfortunatamente, le impostazioni del tema specificate tramite l'attributo `theme` non vengono visualizzate nella visualizzazione progettazione di Visual Studio. Per ulteriori informazioni sui temi e le interfacce, vedere Panoramica dei temi e delle [interfacce ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) e [stili lato server utilizzando i temi](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) . per altre informazioni sulla configurazione di una pagina per l'uso di un tema, vedere [procedura: applicare temi ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) .

[![GridView Visualizza il nome del prodotto, la categoria, il fornitore, il prezzo e le informazioni sospese](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: GridView Visualizza il nome del prodotto, la categoria, il fornitore, il prezzo e le informazioni non più utilizzate ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Visualizzazione di un record alla volta in DetailsView

GridView visualizza una riga per ogni record restituito dal controllo origine dati a cui è associato. In alcuni casi, tuttavia, potrebbe essere necessario visualizzare un solo record o un solo record alla volta. Il [controllo DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) offre questa funzionalità, eseguendo il rendering come `<table>` HTML con due colonne e una riga per ogni colonna o proprietà associata al controllo. È possibile considerare DetailsView come GridView con un singolo record ruotato di 90 gradi.

Per iniziare, aggiungere un controllo DetailsView *sopra* GridView in `SimpleDisplay.aspx`. Quindi, associarlo allo stesso controllo ObjectDataSource di GridView. Analogamente a GridView, un BoundField verrà aggiunto a DetailsView per ogni proprietà nell'oggetto restituito dal metodo `Select` di ObjectDataSource. L'unica differenza è che i BoundField di DetailsView sono disposti orizzontalmente anziché verticalmente.

[![aggiungere un oggetto DetailsView alla pagina e associarlo a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Figura 13**: aggiungere un oggetto DetailsView alla pagina e associarlo a ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image35.png))

Come GridView, i BoundField di DetailsView possono essere ottimizzati per fornire una visualizzazione più personalizzata dei dati restituiti da ObjectDataSource. Nella figura 14 viene illustrato il DetailsView dopo che i BoundField e `CssClass` proprietà sono stati configurati in modo da renderne l'aspetto simile all'esempio di GridView.

[![DetailsView mostra un singolo record](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Figura 14**: DetailsView mostra un singolo record ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image38.png))

Si noti che DetailsView visualizza solo il primo record restituito dalla relativa origine dati. Per consentire all'utente di scorrere tutti i record, uno alla volta, è necessario abilitare il paging per DetailsView. A tale scopo, tornare a Visual Studio e selezionare la casella di controllo Abilita paging nello smart tag di DetailsView.

[![Abilita paging nel controllo DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Figura 15**: abilitare il paging nel controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image41.png))

[![con il paging abilitato, DetailsView consente all'utente di visualizzare tutti i prodotti](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Figura 16**: con il paging abilitato, DetailsView consente all'utente di visualizzare tutti i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image44.png))

Nelle esercitazioni future verranno fornite ulteriori informazioni sul paging.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Layout più flessibile per la visualizzazione di un record alla volta

DetailsView è piuttosto rigido nella modalità di visualizzazione di ogni record restituito da ObjectDataSource. Potrebbe essere necessario disporre di una visualizzazione più flessibile dei dati. Ad esempio, invece di visualizzare il nome del prodotto, la categoria, il fornitore, il prezzo e le informazioni non più utilizzate in una riga separata, è possibile che si desideri mostrare il nome del prodotto e il prezzo in un'intestazione di `<h4>`, con le informazioni sulla categoria e sul fornitore visualizzate sotto il nome e il prezzo in una dimensione del carattere minore. E potrebbe non essere necessario visualizzare i nomi delle proprietà (Product, Category e così via) accanto ai valori.

Il [controllo FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) fornisce questo livello di personalizzazione. Anziché utilizzare i campi (come GridView e DetailsView), FormView utilizza i modelli, che consentono una combinazione di controlli Web, HTML statico e [sintassi di associazione dati](http://www.15seconds.com/issue/040630.htm). Se si ha familiarità con il controllo Repeater di ASP.NET 1. x, è possibile considerare FormView come il ripetitore per visualizzare un singolo record.

Aggiungere un controllo FormView all'area di progettazione della pagina `SimpleDisplay.aspx`. Inizialmente FormView viene visualizzato come un blocco grigio, che informa che è necessario fornire almeno la `ItemTemplate`del controllo.

[![il FormView deve includere un ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Figura 17**: il FormView deve includere un `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image47.png))

È possibile associare FormView direttamente a un controllo origine dati tramite lo smart tag di FormView, che creerà automaticamente un `ItemTemplate` predefinito (insieme a una `EditItemTemplate` e `InsertItemTemplate`, se le proprietà `InsertMethod` e `UpdateMethod` del controllo ObjectDataSource sono impostate). In questo esempio, tuttavia, i dati vengono associati a FormView e ne viene specificato il `ItemTemplate` manualmente. Per iniziare, impostare la proprietà `DataSourceID` di FormView sul `ID` del controllo ObjectDataSource, `ObjectDataSource1`. Successivamente, creare il `ItemTemplate` in modo che visualizzi il nome e il prezzo del prodotto in un elemento `<h4>` e i nomi di categoria e spedizioniere sotto tale dimensione in una dimensione del carattere inferiore.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]

[![il primo prodotto (Chai) viene visualizzato in un formato personalizzato](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Figura 18**: il primo prodotto (Chai) viene visualizzato in un formato personalizzato ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-objectdatasource-cs/_static/image50.png))

Il `<%# Eval(propertyName) %>` è la sintassi di associazione dati. Il metodo `Eval` restituisce il valore della proprietà specificata per l'oggetto corrente associato al controllo FormView. Vedere l'articolo relativo alla [sintassi di data binding semplificato ed esteso](http://www.15seconds.com/issue/040630.htm) di Alex Homer in ASP.NET 2,0 per ulteriori informazioni sugli ins e i outs dell'associazione dati.

Come DetailsView, FormView Mostra solo il primo record restituito da ObjectDataSource. È possibile abilitare il paging in FormView per consentire ai visitatori di scorrere i prodotti uno alla volta.

## <a name="summary"></a>Riepilogo

L'accesso e la visualizzazione dei dati da un livello della logica di business possono essere eseguiti senza scrivere una riga di codice grazie al controllo ObjectDataSource di ASP.NET 2.0. ObjectDataSource richiama un metodo specificato di una classe e restituisce i risultati. Questi risultati possono essere visualizzati in un controllo Web dati associato a ObjectDataSource. In questa esercitazione è stato esaminato l'associazione dei controlli GridView, DetailsView e FormView a ObjectDataSource.

Finora è stato illustrato come usare ObjectDataSource per richiamare un metodo senza parametri, ma cosa accade se si vuole richiamare un metodo che prevede parametri di input, ad esempio la `GetProductsByCategoryID(categoryID)`della classe `ProductBLL`? Per chiamare un metodo che prevede uno o più parametri, è necessario configurare ObjectDataSource per specificare i valori per questi parametri. Nell' [esercitazione successiva](declarative-parameters-cs.md)verrà illustrato come eseguire questa operazione.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Creazione di controlli origine dati personalizzati](https://msdn.microsoft.com/library/ms364049.aspx)
- [Esempi di GridView per ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Sintassi semplificata ed estesa di data binding in ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Temi in ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Stili lato server con temi](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Procedura: applicare temi ASP.NET a livello di codice](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](declarative-parameters-cs.md)
