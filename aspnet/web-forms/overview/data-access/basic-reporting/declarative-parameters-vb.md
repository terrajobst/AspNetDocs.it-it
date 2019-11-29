---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parametri dichiarativi (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come utilizzare un set di parametri su un valore hardcoded per selezionare i dati da visualizzare in un controllo DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: cdc42752fc78d18366af037a81fe4ebe5a1646ef
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612740"
---
# <a name="declarative-parameters-vb"></a>Parametri dichiarativi (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) o [scaricare il file PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> In questa esercitazione verrà illustrato come utilizzare un set di parametri su un valore hardcoded per selezionare i dati da visualizzare in un controllo DetailsView.

## <a name="introduction"></a>Introduzione

Nell' [ultima esercitazione](displaying-data-with-the-objectdatasource-vb.md) è stata esaminata la visualizzazione dei dati con i controlli GridView, DetailsView e FormView associati a un controllo ObjectDataSource che ha richiamato il metodo `GetProducts()` dalla classe `ProductsBLL`. Il metodo `GetProducts()` restituisce un oggetto DataTable fortemente tipizzato popolato con tutti i record della tabella `Products` del database Northwind. La classe `ProductsBLL` contiene metodi aggiuntivi per restituire solo i subset dei prodotti-`GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`e `GetProductsBySupplierID(supplierID)`. Questi tre metodi prevedono un parametro di input che indichi come filtrare le informazioni sul prodotto restituite.

È possibile utilizzare ObjectDataSource per richiamare metodi che prevedono parametri di input, ma a tale scopo è necessario specificare la posizione da cui provengono i valori per questi parametri. I valori dei parametri possono essere hardcoded oppure possono provenire da diverse origini dinamiche, tra cui: valori QueryString, variabili di sessione, il valore della proprietà di un controllo Web nella pagina o altri.

Per questa esercitazione verrà illustrato come usare un set di parametri in un valore hardcoded. In particolare, si esaminerà l'aggiunta di un DetailsView alla pagina che visualizza le informazioni su un prodotto specifico, ovvero la combinazione di gumbo di chef Anton, che ha un `ProductID` di 5. Si vedrà quindi come impostare il valore del parametro in base a un controllo Web. In particolare, si userà una casella di testo per consentire all'utente di digitare un paese, dopo il quale è possibile fare clic su un pulsante per visualizzare l'elenco dei fornitori che risiedono in quel paese.

## <a name="using-a-hard-coded-parameter-value"></a>Uso di un valore di parametro hardcoded

Per il primo esempio, iniziare aggiungendo un controllo DetailsView alla pagina `DeclarativeParams.aspx` nella cartella `BasicReporting`. Dallo smart tag di DetailsView selezionare &lt;nuova origine dati&gt; dall'elenco a discesa e scegliere di aggiungere ObjectDataSource.

[![aggiungere ObjectDataSource alla pagina](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: aggiungere ObjectDataSource alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image3.png))

Verrà avviata automaticamente la scelta guidata origine dati del controllo ObjectDataSource. Selezionare la classe `ProductsBLL` dalla prima schermata della procedura guidata.

[![selezionare la classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: selezionare la classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image6.png))

Poiché si desidera visualizzare informazioni su un prodotto specifico, è necessario utilizzare il metodo `GetProductByProductID(productID)`.

[![scegliere il metodo GetProductByProductID (productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: scegliere il metodo `GetProductByProductID(productID)` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image9.png))

Poiché il metodo selezionato include un parametro, è disponibile un'altra schermata per la procedura guidata, in cui viene chiesto di definire il valore da usare per il parametro. L'elenco a sinistra Mostra tutti i parametri per il metodo selezionato. Per `GetProductByProductID(productID)` è presente una sola `productID`. A destra è possibile specificare il valore per il parametro selezionato. Nell'elenco a discesa parametro source enumera le varie origini possibili per il valore del parametro. Poiché si desidera specificare un valore hardcoded di 5 per il parametro `productID`, lasciare l'origine del parametro come None e immettere 5 nella casella di testo DefaultValue.

[![un valore di parametro hardcoded pari a 5 verrà usato per il parametro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: verrà usato un valore di parametro hardcoded di 5 per il parametro `productID` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image12.png))

Al termine della configurazione guidata origine dati, il markup dichiarativo del controllo ObjectDataSource include un oggetto `Parameter` nella raccolta `SelectParameters` per ognuno dei parametri di input previsti dal metodo definito nella proprietà `SelectMethod`. Poiché il metodo usato in questo esempio prevede un solo parametro di input, `parameterID`, è presente una sola voce. La raccolta di `SelectParameters` può contenere qualsiasi classe derivata dalla classe `Parameter` nello spazio dei nomi `System.Web.UI.WebControls`. Per i valori di parametro hardcoded viene utilizzata la classe di base `Parameter`, ma per le altre opzioni di origine parametro viene utilizzata una classe `Parameter` derivata. Se necessario, è anche possibile creare [tipi di parametro personalizzati](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11).

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Se si sta seguendo il proprio computer, il markup dichiarativo visualizzato a questo punto può includere valori per le proprietà `InsertMethod`, `UpdateMethod`e `DeleteMethod`, nonché `DeleteParameters`. La procedura guidata Scegli origine dati di ObjectDataSource specifica automaticamente i metodi della `ProductBLL` da usare per l'inserimento, l'aggiornamento e l'eliminazione, quindi, a meno che non vengano cancellati in modo esplicito, verranno inclusi nel markup precedente.

Quando si visita questa pagina, il controllo Web dei dati richiama il metodo di `Select` di ObjectDataSource, che chiamerà il metodo `GetProductByProductID(productID)` della classe `ProductsBLL` usando il valore hardcoded di 5 per il parametro di input `productID`. Il metodo restituirà un oggetto `ProductDataTable` fortemente tipizzato che contiene una singola riga con informazioni sulla combinazione di gumbo di chef Anton (il prodotto con `ProductID` 5).

[vengono visualizzate ![informazioni sulla combinazione di gumbo di chef Anton](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: sono visualizzate informazioni sulla combinazione di gumbo di chef Anton ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Impostazione del valore del parametro sul valore della proprietà di un controllo Web

I valori dei parametri di ObjectDataSource possono essere impostati anche in base al valore di un controllo Web nella pagina. Per illustrare questa operazione, si disporrà di un controllo GridView in cui sono elencati tutti i fornitori che si trovano in un paese specificato dall'utente. A tale scopo, aggiungere una casella di testo alla pagina in cui l'utente può immettere un nome di paese. Impostare la proprietà `ID` del controllo TextBox su `CountryName`. Aggiungere anche un controllo Web Button.

[![aggiungere una casella di testo alla pagina con ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: aggiungere una casella di testo alla pagina con `ID` `CountryName` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image18.png))

Successivamente, aggiungere un controllo GridView alla pagina e, dallo smart tag, scegliere di aggiungere un nuovo ObjectDataSource. Poiché si desidera visualizzare le informazioni sui fornitori, selezionare la classe `SuppliersBLL` dalla prima schermata della procedura guidata. Dalla seconda schermata, selezionare il metodo `GetSuppliersByCountry(country)`.

[![scegliere il metodo GetSuppliersByCountry (Country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: scegliere il metodo `GetSuppliersByCountry(country)` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image21.png))

Poiché il metodo `GetSuppliersByCountry(country)` dispone di un parametro di input, la procedura guidata include di nuovo una schermata finale per la scelta del valore del parametro. Questa volta, impostare l'origine del parametro su Control. Verrà popolato l'elenco a discesa ControlID con i nomi dei controlli nella pagina; Selezionare il controllo `CountryName` dall'elenco. Quando la pagina viene visitata per la prima volta, il `CountryName` casella di testo sarà vuota, pertanto non viene restituito alcun risultato e non viene visualizzato nulla. Se per impostazione predefinita si desidera visualizzare alcuni risultati, impostare la casella di testo DefaultValue di conseguenza.

[![impostare il valore del parametro sul valore del controllo CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: impostare il valore del parametro sul valore del controllo `CountryName` ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image24.png))

Il markup dichiarativo di ObjectDataSource è leggermente diverso dal primo esempio, usando un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) anziché l'oggetto `Parameter` standard. Un `ControlParameter` dispone di proprietà aggiuntive per specificare la `ID` del controllo Web e il valore della proprietà da utilizzare per il parametro (`PropertyName`). La configurazione guidata origine dati è stata sufficientemente intelligente da determinare che, per una casella di testo, è probabile che si desideri utilizzare la proprietà `Text` per il valore del parametro. Se tuttavia si vuole usare un valore di proprietà diverso dal controllo Web, è possibile modificare il valore di `PropertyName` qui o facendo clic sul collegamento "Mostra proprietà avanzate" nella procedura guidata.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Quando si visita la pagina per la prima volta, il `CountryName` casella di testo è vuota. Il metodo `Select` di ObjectDataSource viene ancora richiamato da GridView, ma nel metodo `GetSuppliersByCountry(country)` viene passato un valore di `Nothing`. Il TableAdapter converte il `Nothing` in un valore di `NULL` del database (`DBNull.Value`), ma la query utilizzata dal metodo `GetSuppliersByCountry(country)` viene scritta in modo che non restituisca alcun valore quando viene specificato un valore `NULL` per il parametro `@CategoryID`. In breve, non viene restituito alcun fornitore.

Quando il visitatore entra in un paese, tuttavia, fa clic sul pulsante Mostra fornitori per generare un postback, viene eseguita una query sul metodo `Select` di ObjectDataSource, passando il valore `Text` del controllo TextBox come parametro `country`.

[![vengono visualizzati i fornitori del Canada](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: vengono visualizzati i fornitori del Canada ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Visualizzazione di tutti i fornitori per impostazione predefinita

Anziché visualizzare nessuno dei fornitori alla prima visualizzazione della pagina, potrebbe essere necessario mostrare *tutti* i fornitori inizialmente, consentendo all'utente di ridurre l'elenco immettendo un nome di paese nella casella di testo. Quando la casella di testo è vuota, il metodo di `GetSuppliersByCountry(country)` della classe `SuppliersBLL` viene passato in `Nothing` per il parametro di input *`country`* . Questo valore `Nothing` viene quindi passato al metodo di `GetSupplierByCountry(country)` DAL, dove viene convertito in un valore di `NULL` di database per il parametro `@Country` nella query seguente:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

L'espressione `Country = NULL` restituisce sempre false, anche per i record la cui colonna `Country` ha un valore `NULL`; Pertanto, non viene restituito alcun record.

Per restituire *tutti* i fornitori quando la casella di testo Country è vuota, è possibile aumentare il metodo `GetSuppliersByCountry(country)` nell'oggetto BLL per richiamare il metodo `GetSuppliers()` quando il relativo parametro country è `Nothing` e chiamare il metodo di `GetSuppliersByCountry(country)` dal, in caso contrario. Questa operazione avrà come risultato la restituzione di tutti i fornitori quando non viene specificato alcun paese e il subset appropriato di fornitori quando il parametro Country è incluso.

Modificare il metodo `GetSuppliersByCountry(country)` nella classe `SuppliersBLL` nel modo seguente:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Con questa modifica la pagina `DeclarativeParams.aspx` Mostra tutti i fornitori al momento della prima visita (o ogni volta che la casella di testo `CountryName` è vuota).

[![tutti i fornitori sono ora visualizzati per impostazione predefinita](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: tutti i fornitori sono ora visualizzati per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni complete](declarative-parameters-vb/_static/image30.png))

## <a name="summary"></a>Riepilogo

Per usare i metodi con parametri di input, è necessario specificare i valori per i parametri nella raccolta `SelectParameters` di ObjectDataSource. Tipi diversi di parametri consentono di ottenere il valore del parametro da origini diverse. Il tipo di parametro predefinito usa un valore hardcoded, ma è possibile ottenere i valori di parametro da QueryString, le variabili di sessione, i cookie e persino i valori immessi dall'utente dai controlli Web nella pagina.

Gli esempi esaminati in questa esercitazione illustrano come usare i valori dei parametri dichiarativi. Tuttavia, in alcuni casi è necessario usare un'origine dei parametri non disponibile, ad esempio la data e l'ora correnti oppure, se il sito usa l'appartenenza, l'ID utente del visitatore. Per questi scenari è possibile impostare i valori dei parametri a livello di codice prima di ObjectDataSource richiamando il metodo dell'oggetto sottostante. Nell' [esercitazione successiva](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)verrà illustrato come eseguire questa operazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-objectdatasource-vb.md)
> [Successivo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
