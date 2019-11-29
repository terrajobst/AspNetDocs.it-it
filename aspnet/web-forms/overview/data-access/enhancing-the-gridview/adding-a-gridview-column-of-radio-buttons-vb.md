---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Aggiunta di una colonna GridView di pulsanti di opzione (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene illustrato come aggiungere una colonna di pulsanti di opzione a un controllo GridView per fornire all'utente un modo più intuitivo per selezionare una singola riga di...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: ee67a4556c65d2c9570bf15b42fc3c8e5f555bda
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592740"
---
# <a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Aggiunta di una colonna GridView di pulsanti di opzione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) o [scaricare il file PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> In questa esercitazione viene illustrato come aggiungere una colonna di pulsanti di opzione a un controllo GridView per fornire all'utente un modo più intuitivo per selezionare una singola riga di GridView.

## <a name="introduction"></a>Introduzione

Il controllo GridView offre una grande quantità di funzionalità predefinite. Include una serie di campi diversi per la visualizzazione di testo, immagini, collegamenti ipertestuali e pulsanti. Supporta i modelli per un'ulteriore personalizzazione. Con pochi clic del mouse, è possibile creare un controllo GridView in cui ogni riga può essere selezionata tramite un pulsante o per abilitare la modifica o l'eliminazione delle funzionalità. Nonostante la pletora di funzionalità fornite, spesso si verificano situazioni in cui è necessario aggiungere funzionalità aggiuntive non supportate. In questa esercitazione e nei prossimi due si esaminerà come migliorare la funzionalità di GridView per includere funzionalità aggiuntive.

Questa esercitazione e quella successiva si concentrano sul miglioramento del processo di selezione delle righe. Come esaminato in [Master/Detail usando un GridView Master selezionabile con un controllo DetailView per di dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), è possibile aggiungere un oggetto CommandField al GridView che include un pulsante Select. Quando si fa clic su un postback, viene eseguito un postback e la proprietà `SelectedIndex` GridView s viene aggiornata all'indice della riga di cui è stato fatto clic sul pulsante Seleziona. In [Master/Details con un'esercitazione Selectable Master GridView with an Details controllo DetailView per](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) è stato illustrato come usare questa funzionalità per visualizzare i dettagli per la riga GridView selezionata.

Mentre il pulsante Seleziona funziona in molte situazioni, potrebbe non funzionare anche per altri. Anziché utilizzare un pulsante, altri due elementi dell'interfaccia utente vengono comunemente utilizzati per la selezione: il pulsante di opzione e la casella di controllo. È possibile aumentare il controllo GridView in modo che invece di un pulsante di selezione ogni riga contenga un pulsante di opzione o una casella di controllo. Negli scenari in cui l'utente può solo selezionare uno dei record GridView, il pulsante di opzione potrebbe essere preferito sul pulsante Seleziona. Nelle situazioni in cui l'utente può potenzialmente selezionare più record, ad esempio in un'applicazione di posta elettronica basata sul Web, in cui un utente potrebbe voler selezionare più messaggi per l'eliminazione della casella di controllo offre funzionalità non disponibili tramite il pulsante Seleziona o pulsante di opzione interfacce utente.

Questa esercitazione illustra come aggiungere una colonna di pulsanti di opzione a GridView. L'esercitazione continuazione Esplora l'uso delle caselle di controllo.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Passaggio 1: creazione dell'oggetto che migliora le pagine Web GridView

Prima di iniziare a migliorare GridView per includere una colonna di pulsanti di opzione, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le due successive. Per iniziare, aggiungere una nuova cartella denominata `EnhancedGridView`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni correlate a SqlDataSource

Analogamente alle altre cartelle, `Default.aspx` nella cartella `EnhancedGridView` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))

Infine, aggiungere le quattro pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo l'utilizzo del controllo SqlDataSource `<siteMapNode>`:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica, inserimento ed eliminazione.

![La mappa del sito include ora le voci per migliorare le esercitazioni di GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora le voci per migliorare le esercitazioni di GridView

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Passaggio 2: visualizzazione dei fornitori in un controllo GridView

Per questa esercitazione, s compila un GridView che elenca i fornitori degli Stati Uniti, con ogni riga GridView che fornisce un pulsante di opzione. Dopo aver selezionato un fornitore tramite il pulsante di opzione, l'utente può visualizzare i prodotti dei fornitori facendo clic su un pulsante. Sebbene questa attività possa sembrare semplice, esistono diverse sottigliezze che lo rendono particolarmente complesso. Prima di approfondire queste sottigliezze, si ottiene prima di tutto un GridView che elenca i fornitori.

Per iniziare, aprire la pagina `RadioButtonField.aspx` nella cartella `EnhancedGridView` trascinando un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare il `ID` di GridView su `Suppliers` e, dallo smart tag, scegliere di creare una nuova origine dati. In particolare, creare un ObjectDataSource denominato `SuppliersDataSource` che estrae i dati dall'oggetto `SuppliersBLL`.

[![creare un nuovo ObjectDataSource denominato SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: creare un nuovo ObjectDataSource denominato `SuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))

[![configurare ObjectDataSource per l'utilizzo della classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurare ObjectDataSource per l'uso della classe `SuppliersBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))

Poiché si desidera solo elencare i fornitori negli Stati Uniti, scegliere il metodo `GetSuppliersByCountry(country)` dall'elenco a discesa nella scheda Seleziona.

[![configurare ObjectDataSource per l'utilizzo della classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurare ObjectDataSource per l'uso della classe `SuppliersBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))

Dalla scheda aggiornamento, selezionare l'opzione (nessuno) e fare clic su Avanti.

[![configurare ObjectDataSource per l'utilizzo della classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurare ObjectDataSource per l'uso della classe `SuppliersBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))

Poiché il metodo `GetSuppliersByCountry(country)` accetta un parametro, la procedura guidata Configura origine dati richiede l'origine del parametro. Per specificare un valore hardcoded (USA, in questo esempio), lasciare l'elenco a discesa parametro source impostato su None e immettere il valore predefinito nella casella di testo. Fare clic su Fine per completare la procedura guidata.

[![USA USA come valore predefinito per il parametro Country](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: usare USA come valore predefinito per il parametro `country` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))

Al termine della procedura guidata, in GridView verrà incluso un BoundField per ognuno dei campi dati fornitore. Rimuovere tutti i BoundField, tranne i `CompanyName`, `City`e `Country`, e rinominare la proprietà `CompanyName` BoundFields `HeaderText` in Supplier. Dopo questa operazione, la sintassi dichiarativa GridView e ObjectDataSource dovrebbe essere simile alla seguente.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Per questa esercitazione, consente all'utente di visualizzare i prodotti dei fornitori selezionati nella stessa pagina dell'elenco Supplier o in una pagina diversa. Per risolvere questo problema, aggiungere due controlli Web Button alla pagina. Ho impostato la `ID` di questi due pulsanti su `ListProducts` e `SendToProducts`, con l'idea che, quando si fa clic su `ListProducts` si verificherà un postback, i prodotti selezionati verranno elencati nella stessa pagina, ma quando si fa clic su `SendToProducts`, l'utente verrà sbattuto in un'altra pagina in cui sono elencati i prodotti.

Nella figura 9 è illustrato il `Suppliers` GridView e i controlli Web di due pulsanti quando vengono visualizzati tramite un browser.

[![ai fornitori degli Stati Uniti le informazioni relative a nome, città e paese sono elencate](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: per i fornitori degli Stati Uniti sono elencate le informazioni relative a nome, città e paese ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Passaggio 3: aggiunta di una colonna di pulsanti di opzione

A questo punto, il `Suppliers` GridView ha tre BoundField che visualizzano il nome della società, la città e il paese di ogni fornitore negli Stati Uniti. Tuttavia, manca ancora una colonna di pulsanti di opzione. Sfortunatamente, GridView non include una RadioButtonField incorporata, in caso contrario è possibile aggiungerla alla griglia ed essere eseguita. Al contrario, è possibile aggiungere un TemplateField e configurarne la `ItemTemplate` per eseguire il rendering di un pulsante di opzione, ottenendo un pulsante di opzione per ogni riga di GridView.

Inizialmente, si potrebbe supporre che l'interfaccia utente desiderata possa essere implementata aggiungendo un controllo Web RadioButton al `ItemTemplate` di un TemplateField. Sebbene venga effettivamente aggiunto un singolo pulsante di opzione a ogni riga di GridView, i pulsanti di opzione non possono essere raggruppati e pertanto non si escludono a vicenda. Ovvero, un utente finale può selezionare più pulsanti di opzione contemporaneamente da GridView.

Anche se l'uso di un TemplateField di controlli Web RadioButton non offre le funzionalità necessarie, consente di implementare questo approccio, perché vale la pena esaminare il motivo per cui i pulsanti di opzione risultanti non sono raggruppati. Per iniziare, aggiungere un TemplateField al GridView Suppliers, rendendolo il campo più a sinistra. Quindi, dallo smart tag di GridView, fare clic sul collegamento modifica modelli e trascinare un controllo Web RadioButton dalla casella degli strumenti in TemplateField s `ItemTemplate` (vedere la figura 10). Impostare la proprietà `ID` di RadioButton su `RowSelector` e la proprietà `GroupName` su `SuppliersGroup`.

[![aggiungere un controllo Web RadioButton a ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: aggiungere un controllo Web RadioButton al `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))

Dopo aver apportato queste aggiunte tramite la finestra di progettazione, il markup di GridView avrà un aspetto simile al seguente:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

La [proprietà`GroupName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) di RadioButton è quella usata per raggruppare una serie di pulsanti di opzione. Tutti i controlli RadioButton con lo stesso valore `GroupName` sono considerati raggruppati; è possibile selezionare un solo pulsante di opzione da un gruppo alla volta. La proprietà `GroupName` specifica il valore per l'attributo `name` del pulsante di opzione di cui è stato eseguito il rendering. Il browser esamina i pulsanti di opzione `name` attributi per determinare i raggruppamenti dei pulsanti di opzione.

Con il controllo Web RadioButton aggiunto al `ItemTemplate`, visitare questa pagina tramite un browser e fare clic sui pulsanti di opzione nelle righe della griglia. Si noti che i pulsanti di opzione non sono raggruppati, rendendo possibile la selezione di tutte le righe, come illustrato nella figura 11.

[![i pulsanti di opzione di GridView non sono raggruppati](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: i pulsanti di opzione di GridView non sono raggruppati ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))

Il motivo per cui i pulsanti di opzione non sono raggruppati è il fatto che gli attributi `name` sottoposti a rendering sono diversi, pur avendo la stessa impostazione della proprietà `GroupName`. Per visualizzare queste differenze, eseguire una visualizzazione/origine dal browser ed esaminare il markup del pulsante di opzione:

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Si noti che gli attributi `name` e `id` non sono i valori esatti specificati nel Finestra Proprietà, ma sono preceduto da un numero di altri valori di `ID`. I valori `ID` aggiuntivi aggiunti all'inizio degli `id` sottoposti a rendering e `name` gli attributi sono i `ID` s dei pulsanti di opzione padre controllano le `GridViewRow` s `ID`, il `ID`di GridView, il `ID`del controllo contenuto e il `ID`Web Form. Questi `ID` vengono aggiunti in modo che ogni controllo Web di cui è stato eseguito il rendering in GridView disponga di un valore `id` e `name` univoco.

Ogni controllo di cui è stato eseguito il rendering necessita di un `name` e `id` diversi perché questo è il modo in cui il browser identifica in modo univoco ogni controllo sul lato client e come identifica al server Web quale azione o modifica si è verificata durante il postback. Si supponga, ad esempio, di voler eseguire parte del codice sul lato server ogni volta che lo stato selezionato di RadioButton è stato modificato. A tale scopo, è possibile impostare la proprietà `AutoPostBack` di RadioButton su `True` e creare un gestore eventi per l'evento `CheckChanged`. Tuttavia, se il `name` e i valori di `id` di cui è stato eseguito il rendering per tutti i pulsanti di opzione sono uguali, durante il postback non è stato possibile determinare quale RadioButton specifico è stato selezionato.

Il breve è che non è possibile creare una colonna di pulsanti di opzione in GridView usando il controllo Web RadioButton. Al contrario, è necessario utilizzare tecniche piuttosto arcaiche per garantire che il markup appropriato venga inserito in ogni riga di GridView.

> [!NOTE]
> Analogamente al controllo Web RadioButton, il controllo HTML del pulsante di opzione, quando viene aggiunto a un modello, includerà l'attributo `name` univoco, rendendo i pulsanti di opzione nella griglia non raggruppati. Se non si ha familiarità con i controlli HTML, è possibile ignorare questa nota, perché i controlli HTML vengono usati raramente, soprattutto in ASP.NET 2,0. Tuttavia, se si è interessati a saperne di più, vedere la pagina relativa ai controlli [Web e ai controlli HTML](http://www.odetocode.com/Articles/348.aspx)del post del Blog di [Scott Allen](http://odetocode.com/blogs/scott/default.aspx) .

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Uso di un controllo Literal per inserire il markup del pulsante di opzione

Per raggruppare correttamente tutti i pulsanti di opzione all'interno di GridView, è necessario inserire manualmente il markup dei pulsanti di opzione nel `ItemTemplate`. Per ogni pulsante di opzione è necessario lo stesso attributo `name`, ma deve essere presente un attributo `id` univoco, nel caso in cui si desideri accedere a un pulsante di opzione tramite script sul lato client. Quando un utente seleziona un pulsante di opzione e inserisce nuovamente la pagina, il browser restituirà il valore dell'attributo `value` del pulsante di opzione selezionato. Per ogni pulsante di opzione sarà pertanto necessario un attributo `value` univoco. Infine, al postback è necessario assicurarsi di aggiungere l'attributo `checked` al pulsante di opzione uno selezionato. in caso contrario, dopo che l'utente effettua una selezione e invia un postback, i pulsanti di opzione torneranno allo stato predefinito (tutti deselezionati).

Esistono due approcci che è possibile adottare per inserire markup di basso livello in un modello. Uno consiste nell'eseguire una combinazione di markup e chiamate a metodi di formattazione definiti nella classe code-behind. Questa tecnica è stata descritta per la prima volta nell'esercitazione sull' [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) . In questo caso potrebbe essere simile a:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

In questo caso, `GetUniqueRadioButton` e `GetRadioButtonValue` sono i metodi definiti nella classe code-behind che hanno restituito i valori appropriati di `id` e `value` attributo per ogni pulsante di opzione. Questo approccio funziona bene per l'assegnazione degli attributi `id` e `value`, ma non è sufficiente quando è necessario specificare il valore dell'attributo `checked` perché la sintassi DataBinding viene eseguita solo quando i dati vengono prima associati al controllo GridView. Se, pertanto, per GridView è abilitato lo stato di visualizzazione, i metodi di formattazione verranno attivati solo quando la pagina viene caricata per la prima volta (o quando GridView viene riassociato in modo esplicito all'origine dati) e pertanto la funzione che imposta l'attributo `checked` non verrà chiamata durante il postback. Si tratta di un problema piuttosto sottile e di un po' oltre l'ambito di questo articolo, quindi lascio l'argomento. Tuttavia, si consiglia di provare a usare l'approccio precedente e risolverlo fino al punto in cui si rimarrà bloccati. Sebbene tale esercizio non consenta di avvicinarsi a una versione funzionante, consentirà di favorire una conoscenza più approfondita di GridView e del ciclo di vita dell'associazione dati.

L'altro approccio per inserire markup personalizzato di basso livello in un modello e l'approccio che verrà usato per questa esercitazione consiste nell'aggiungere un [controllo letterale](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) al modello. Nel gestore dell'evento GridView s `RowCreated` o `RowDataBound`, il controllo Literal può essere accessibile a livello di codice e la relativa proprietà `Text` impostata sul markup da emettere.

Per iniziare, rimuovere il controllo RadioButton dal `ItemTemplate`s di TemplateField, sostituendo il controllo con un controllo Literal. Impostare la `ID` del controllo letterale su `RadioButtonMarkup`.

[![aggiungere un controllo Literal a ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: aggiungere un controllo Literal al `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))

Successivamente, creare un gestore eventi per l'evento GridView s `RowCreated`. L'evento `RowCreated` viene generato una volta per ogni riga aggiunta, indipendentemente dal fatto che i dati vengano riassociati al controllo GridView. Ciò significa che anche su un postback quando i dati vengono ricaricati dallo stato di visualizzazione, l'evento `RowCreated` viene comunque attivato e questo è il motivo per cui viene utilizzato anziché `RowDataBound` (che viene attivato solo quando i dati vengono associati in modo esplicito al controllo Web dati).

In questo gestore dell'evento si vuole continuare solo se si sta usando una riga di dati. Per ogni riga di dati si vuole fare riferimento a livello di codice al controllo `RadioButtonMarkup` valore letterale e impostare la relativa proprietà `Text` sul markup da emettere. Come illustrato nel codice seguente, il markup generato crea un pulsante di opzione il cui attributo `name` è impostato su `SuppliersGroup`, il cui attributo `id` è impostato su `RowSelectorX`, dove *X* è l'indice della riga GridView e il cui attributo `value` è impostato sull'indice della riga GridView.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Quando si seleziona una riga GridView e si verifica un postback, si è interessati al `SupplierID` del fornitore selezionato. Pertanto, si potrebbe pensare che il valore di ogni pulsante di opzione sia il `SupplierID` effettivo (anziché l'indice della riga GridView). Sebbene ciò possa funzionare in determinate circostanze, sarebbe un rischio per la sicurezza di accettare ed elaborare in modo cieco un `SupplierID`. Il GridView, ad esempio, elenca solo i fornitori negli Stati Uniti. Tuttavia, se il `SupplierID` viene passato direttamente dal pulsante di opzione, cosa impedisce a un utente non autorizzato di manipolare il valore `SupplierID` restituito durante il postback? Utilizzando l'indice di riga come `value`e quindi recuperando il `SupplierID` durante il postback dalla raccolta `DataKeys`, è possibile garantire che l'utente utilizzi solo uno dei valori `SupplierID` associati a una delle righe di GridView.

Dopo l'aggiunta del codice del gestore eventi, provare a eseguire il test della pagina in un browser. Si noti innanzitutto che è possibile selezionare un solo pulsante di opzione nella griglia alla volta. Tuttavia, quando si seleziona un pulsante di opzione e si fa clic su uno dei pulsanti, viene eseguito un postback e i pulsanti di opzione ripristinano lo stato iniziale, ovvero al postback, il pulsante di opzione selezionato non è più selezionato. Per risolvere questo problema, è necessario aumentare il gestore dell'evento `RowCreated` in modo che controlli l'indice del pulsante di opzione selezionato inviato dal postback e aggiunga l'attributo `checked="checked"` al markup emesso delle corrispondenze dell'indice di riga.

Quando si verifica un postback, il browser restituisce il `name` e `value` del pulsante di opzione selezionato. Il valore può essere recuperato a livello di codice utilizzando `Request.Form("name")`. La [proprietà`Request.Form`](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornisce un [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) che rappresenta le variabili del form. Le variabili del modulo sono i nomi e i valori dei campi del modulo nella pagina Web e vengono restituiti dal Web browser ogni volta che viene eseguito un postback. Poiché l'attributo `name` sottoposto a rendering dei pulsanti di opzione in GridView è `SuppliersGroup`, quando viene eseguito il postback della pagina Web, il browser invierà `SuppliersGroup=valueOfSelectedRadioButton` al server Web (insieme agli altri campi del modulo). È quindi possibile accedere a queste informazioni dalla proprietà `Request.Form` utilizzando: `Request.Form("SuppliersGroup")`.

Poiché è necessario determinare l'indice del pulsante di opzione selezionato non solo nel gestore dell'evento `RowCreated`, ma nei gestori eventi `Click` per i controlli Web del pulsante, consente di aggiungere una proprietà `SuppliersSelectedIndex` alla classe code-behind che restituisce `-1` se non è stato selezionato alcun pulsante di opzione e l'indice selezionato se uno dei pulsanti di opzione è selezionato.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Con questa proprietà aggiunta, è possibile aggiungere il markup `checked="checked"` nel gestore eventi `RowCreated` quando `SuppliersSelectedIndex` è uguale a `e.Row.RowIndex`. Aggiornare il gestore eventi per includere questa logica:

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Con questa modifica, il pulsante di opzione selezionato rimane selezionato dopo un postback. Ora che è possibile specificare il pulsante di opzione selezionato, è possibile modificare il comportamento in modo che, quando la pagina è stata visitata per la prima volta, sia stato selezionato il primo pulsante di opzione della riga GridView, anziché avere selezionato pulsanti di opzione per impostazione predefinita, che corrisponde all'attuale comportamento). Per fare in modo che il primo pulsante di opzione sia selezionato per impostazione predefinita, è sufficiente modificare l'istruzione `If SuppliersSelectedIndex = e.Row.RowIndex Then` come riportato di seguito: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

A questo punto è stata aggiunta una colonna di pulsanti di opzione raggruppati a GridView che consente di selezionare e ricordare una singola riga GridView nei postback. I passaggi successivi sono la visualizzazione dei prodotti forniti dal fornitore selezionato. Nel passaggio 4 verrà illustrato come reindirizzare l'utente a un'altra pagina, inviando il `SupplierID`selezionato. Nel passaggio 5 verrà illustrato come visualizzare i prodotti selezionati del fornitore in un controllo GridView nella stessa pagina.

> [!NOTE]
> Invece di usare un TemplateField (l'obiettivo di questa lunga fase 3), è possibile creare una classe di `DataControlField` personalizzata che esegua il rendering dell'interfaccia utente e della funzionalità appropriate. La [classe`DataControlField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) è la classe di base da cui derivano i campi BoundField, CheckBoxField, TemplateField e altri campi GridView e DetailsView predefiniti. La creazione di una classe di `DataControlField` personalizzata significherebbe che la colonna di pulsanti di opzione potrebbe essere aggiunta usando solo la sintassi dichiarativa e anche la replica della funzionalità su altre pagine Web e altre applicazioni Web in modo molto più semplice.

Se sono già stati creati controlli compilati e personalizzati in ASP.NET, tuttavia, si sa che questa operazione richiede una notevole quantità di noia e porta un host di sottigliezze e casi limite che devono essere gestiti con cautela. Di conseguenza, si rinuncia a implementare una colonna di pulsanti di opzione come classe `DataControlField` personalizzata per il momento e come usare l'opzione TemplateField. Probabilmente potrai esplorare la creazione, l'uso e la distribuzione di classi di `DataControlField` personalizzate in un'esercitazione futura.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Passaggio 4: visualizzazione dei prodotti selezionati per i fornitori in una pagina separata

Dopo che l'utente ha selezionato una riga GridView, è necessario mostrare i prodotti selezionati del fornitore. In alcuni casi, è possibile che si desideri visualizzare questi prodotti in una pagina separata, in altri potrebbe essere preferibile eseguire questa operazione nella stessa pagina. Consente di esaminare prima di tutto come visualizzare i prodotti in una pagina separata; nel passaggio 5 si esaminerà l'aggiunta di GridView a `RadioButtonField.aspx` per visualizzare i prodotti selezionati dei fornitori.

Attualmente sono presenti due controlli Web dei pulsanti nella pagina `ListProducts` e `SendToProducts`. Quando si fa clic sul pulsante `SendToProducts`, si vuole inviare l'utente a `~/Filtering/ProductsForSupplierDetails.aspx`. Questa pagina è stata creata nell'esercitazione [filtro master/dettagli in due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) e Visualizza i prodotti per il fornitore il cui `SupplierID` viene passato tramite il campo querystring denominato `SupplierID`.

Per fornire questa funzionalità, creare un gestore eventi per l'evento `Click` del pulsante `SendToProducts`. Nel passaggio 3 è stata aggiunta la proprietà `SuppliersSelectedIndex`, che restituisce l'indice della riga di cui è stato selezionato il pulsante di opzione. Il `SupplierID` corrispondente può essere recuperato dalla raccolta `DataKeys` di GridView e l'utente può quindi essere inviato a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` utilizzando `Response.Redirect("url")`.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Questo codice funziona meravigliosamente purché uno dei pulsanti di opzione sia selezionato da GridView. Se, inizialmente, per GridView non sono selezionati pulsanti di opzione e l'utente fa clic sul pulsante `SendToProducts`, `SuppliersSelectedIndex` verrà `-1`, che genera un'eccezione poiché `-1` non è compreso nell'intervallo di indici della raccolta di `DataKeys`. Questo non è un problema, tuttavia, se si è deciso di aggiornare il gestore dell'evento `RowCreated` come illustrato nel passaggio 3, in modo da avere inizialmente selezionato il primo pulsante di opzione in GridView.

Per adattare un valore `SuppliersSelectedIndex` di `-1`, aggiungere un controllo Web Label alla pagina sopra GridView. Impostare la relativa proprietà `ID` su `ChooseSupplierMsg`, la relativa proprietà `CssClass` su `Warning`, le proprietà `EnableViewState` e `Visible` su `False`e la relativa proprietà `Text` per scegliere un fornitore dalla griglia. La classe CSS `Warning` Visualizza il testo in un tipo di carattere rosso, corsivo, grassetto e di grandi dimensioni e viene definito in `Styles.css`. Impostando le proprietà `EnableViewState` e `Visible` su `False`, l'etichetta non viene sottoposta a rendering ad eccezione dei soli postback in cui la proprietà `Visible` del controllo è impostata a livello di codice su `True`.

[![aggiungere un controllo Web Label sopra GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: aggiungere un controllo Web etichetta sopra il GridView ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))

Successivamente, aumentare il gestore dell'evento `Click` per visualizzare l'etichetta `ChooseSupplierMsg` se `SuppliersSelectedIndex` è minore di zero e reindirizzare l'utente a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` in caso contrario.

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visita la pagina in un browser e fai clic sul pulsante `SendToProducts` prima di selezionare un fornitore in GridView. Come illustrato nella figura 14, viene visualizzata l'etichetta `ChooseSupplierMsg`. Selezionare quindi un fornitore e fare clic sul pulsante `SendToProducts`. Questa operazione consente di visualizzare una pagina in cui sono elencati i prodotti forniti dal fornitore selezionato. Nella figura 15 viene illustrata la pagina `ProductsForSupplierDetails.aspx` quando è stato selezionato il fornitore di Bigfoot birrerie.

[![viene visualizzata l'etichetta ChooseSupplierMsg se non è selezionato alcun fornitore](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: l'etichetta di `ChooseSupplierMsg` viene visualizzata se non è selezionato alcun fornitore ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))

[![i prodotti selezionati vengono visualizzati in ProductsForSupplierDetails. aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: i prodotti selezionati sono visualizzati in `ProductsForSupplierDetails.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Passaggio 5: visualizzazione dei prodotti selezionati del fornitore nella stessa pagina

Nel passaggio 4 è stato illustrato come inviare l'utente a un'altra pagina Web per visualizzare i prodotti selezionati del fornitore. In alternativa, è possibile visualizzare i prodotti dei fornitori selezionati nella stessa pagina. Per illustrare questo problema, si aggiungerà un altro GridView per `RadioButtonField.aspx` per visualizzare i prodotti selezionati del fornitore.

Poiché si desidera che questo GridView di prodotti venga visualizzato solo dopo la selezione di un fornitore, aggiungere un controllo Web Panel sotto la `Suppliers` GridView, impostando il `ID` su `ProductsBySupplierPanel` e la relativa proprietà `Visible` su `False`. All'interno del pannello aggiungere i prodotti di testo per il fornitore selezionato, seguito da un controllo GridView denominato `ProductsBySupplier`. Dallo smart tag GridView s scegliere di associarlo a un nuovo ObjectDataSource denominato `ProductsBySupplierDataSource`.

[![associare GridView ProductsBySupplier a un nuovo ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: associare il `ProductsBySupplier` GridView a un nuovo ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))

Configurare quindi ObjectDataSource per l'uso della classe `ProductsBLL`. Poiché si desidera recuperare solo i prodotti forniti dal fornitore selezionato, specificare che ObjectDataSource deve richiamare il metodo `GetProductsBySupplierID(supplierID)` per recuperare i dati. Selezionare (nessuno) dagli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina.

[![configurare ObjectDataSource per l'utilizzo del metodo GetProductsBySupplierID (supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurare ObjectDataSource per l'uso del metodo `GetProductsBySupplierID(supplierID)` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))

[![impostare gli elenchi a discesa su (nessuno) nelle schede Aggiorna, Inserisci ed Elimina](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: impostare gli elenchi a discesa su (nessuno) nelle schede di aggiornamento, inserimento ed eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))

Dopo aver configurato le schede SELECT, UPDATE, INSERT e DELETE, fare clic su Next. Poiché il metodo `GetProductsBySupplierID(supplierID)` prevede un parametro di input, la creazione guidata origine dati richiede di specificare l'origine per il valore del parametro.

Qui sono disponibili due opzioni per specificare l'origine del valore del parametro. È possibile utilizzare l'oggetto parametro predefinito e assegnare a livello di codice il valore della proprietà `SuppliersSelectedIndex` alla proprietà `DefaultValue` del parametro nel gestore dell'evento ObjectDataSource s `Selecting`. Fare riferimento all' [impostazione a livello di codice dell'esercitazione sui valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) per un aggiornamento a livello di codice per l'assegnazione di valori ai parametri di ObjectDataSource.

In alternativa, è possibile usare un ControlParameter e fare riferimento alla proprietà `Suppliers` GridView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (vedere la figura 19). La proprietà `SelectedValue` di GridView restituisce il valore `DataKey` corrispondente alla [proprietà`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Per consentire il funzionamento di questa opzione, è necessario impostare a livello di codice la proprietà `SelectedIndex` di GridView sulla riga selezionata quando si fa clic sul pulsante `ListProducts`. Come ulteriore vantaggio, impostando la `SelectedIndex`, il record selezionato prenderà in `SelectedRowStyle` definito nel tema `DataWebControls` (uno sfondo giallo).

[![utilizzare un ControlParameter per specificare il valore di GridView s SelectedValue come origine del parametro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: usare un ControlParameter per specificare il valore di GridView s SelectedValue come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))

Quando si completa la procedura guidata, Visual Studio aggiunge automaticamente i campi per i campi dati del prodotto. Rimuovere tutti i BoundField tranne `ProductName`, `CategoryName`e `UnitPrice` e modificare le proprietà del `HeaderText` in Product, Category e price. Configurare il BoundField `UnitPrice` in modo che il relativo valore venga formattato come valuta. Dopo aver apportato queste modifiche, il markup dichiarativo Panel, GridView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Per completare questo esercizio, è necessario impostare la proprietà `SelectedIndex` di GridView sul `SelectedSuppliersIndex` e la proprietà `Visible` del pannello `ProductsBySupplierPanel` per `True` quando si fa clic sul pulsante `ListProducts`. A tale scopo, creare un gestore eventi per l'evento `Click` del controllo Web di `ListProducts` Button e aggiungere il codice seguente:

[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Se un fornitore non è stato selezionato da GridView, viene visualizzata l'etichetta `ChooseSupplierMsg` e il pannello `ProductsBySupplierPanel` nascosto. In caso contrario, se è stato selezionato un fornitore, viene visualizzato il `ProductsBySupplierPanel` e viene aggiornata la proprietà `SelectedIndex` di GridView.

La figura 20 Mostra i risultati dopo che il fornitore di Bigfoot birrerie è stato selezionato ed è stato fatto clic sul pulsante Mostra prodotti su pagina.

[![i prodotti forniti da Bigfoot birrifici sono elencati nella stessa pagina](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: i prodotti forniti da un birrificio Bigfoot sono elencati nella stessa pagina ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))

## <a name="summary"></a>Riepilogo

Come illustrato in [Master/Details con un'esercitazione Selectable Master GridView with an Details controllo DetailView per](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) , è possibile selezionare i record da un GridView usando un oggetto CommandField la cui proprietà `ShowSelectButton` è impostata su `True`. Tuttavia, CommandField Visualizza i pulsanti come pulsanti di push normali, collegamenti o immagini. Un'interfaccia utente alternativa per la selezione delle righe consiste nel fornire un pulsante di opzione o una casella di controllo in ogni riga di GridView. In questa esercitazione è stato esaminato come aggiungere una colonna di pulsanti di opzione.

Sfortunatamente, l'aggiunta di una colonna di pulsanti di opzione non è semplice o semplice come si può prevedere. Non è presente alcun RadioButtonField incorporato che può essere aggiunto con un clic di un pulsante e l'uso del controllo Web RadioButton all'interno di un TemplateField introduce un proprio set di problemi. Alla fine, per fornire un'interfaccia di questo tipo, è necessario creare una classe di `DataControlField` personalizzata o ricorrere a inserire il codice HTML appropriato in un TemplateField durante l'evento di `RowCreated`.

Dopo aver esplorato come aggiungere una colonna di pulsanti di opzione, è possibile riportare l'attenzione sull'aggiunta di una colonna di caselle di controllo. Con una colonna di caselle di controllo, un utente può selezionare una o più righe GridView, quindi eseguire alcune operazioni su tutte le righe selezionate, ad esempio selezionando un set di messaggi di posta elettronica da un client di posta elettronica basato sul Web e quindi scegliendo di eliminare tutti i messaggi di posta elettronica selezionati. Nell'esercitazione successiva si vedrà come aggiungere una colonna di questo tipo.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato David Suru. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Successivo](adding-a-gridview-column-of-checkboxes-vb.md)
