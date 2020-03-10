---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Visualizzazione di dati con i controlli DataList e RepeaterC#() | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato usato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, si cercherà di creare modelli di report comuni con...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 09d3faf811f21a66bb5c234f71d77b2552ae6516
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611860"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Visualizzazione di dati con i controlli DataList e Repeater (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) o [scaricare il file PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Nelle esercitazioni precedenti è stato usato il controllo GridView per visualizzare i dati. A partire da questa esercitazione, viene illustrata la creazione di modelli di report comuni con i controlli DataList e Repeater, a partire dalle nozioni di base per la visualizzazione dei dati con questi controlli.

## <a name="introduction"></a>Introduzione

In tutti gli esempi per le ultime 28 esercitazioni, se era necessario visualizzare più record da un'origine dati, è stato attivato il controllo GridView. GridView esegue il rendering di una riga per ogni record nell'origine dati, visualizzando i campi dati del record in colonne. Mentre GridView lo rende uno snap per la visualizzazione, la pagina, l'ordinamento, la modifica e l'eliminazione dei dati, il suo aspetto è un po' squadrato. Inoltre, il markup responsabile della struttura di GridView s è fisso, include un `<table>` HTML con una riga di tabella (`<tr>`) per ogni record e una cella della tabella (`<td>`) per ogni campo.

Per fornire un livello di personalizzazione maggiore nell'aspetto e nel markup sottoposto a rendering quando si visualizzano più record, ASP.NET 2,0 offre i controlli DataList e Repeater (entrambi disponibili anche in ASP.NET versione 1. x). I controlli DataList e Repeater eseguono il rendering del contenuto utilizzando modelli anziché BoundField, CheckBoxFields, ButtonFields e così via. Come GridView, DataList esegue il rendering come `<table>`HTML, ma consente la visualizzazione di più record di origini dati per riga di tabella. Il ripetitore, d'altra parte, non esegue il rendering di markup aggiuntivi rispetto a quanto specificato in modo esplicito ed è un candidato ideale quando è necessario un controllo preciso sul markup emesso.

Nelle prossime dozzine di esercitazioni verrà illustrata la creazione di modelli di report comuni con i controlli DataList e Repeater, a partire dalle nozioni di base per la visualizzazione dei dati con questi modelli di controlli. Si vedrà come formattare questi controlli, come modificare il layout dei record dell'origine dati in DataList, scenari master/dettagli comuni, modi per modificare ed eliminare i dati, come passare da una pagina all'altra e così via.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Passaggio 1: aggiunta delle pagine Web dell'esercitazione su DataList e Repeater

Prima di iniziare questa esercitazione, è necessario prima di tutto aggiungere le pagine ASP.NET necessarie per questa esercitazione e le prossime esercitazioni relative alla visualizzazione dei dati tramite DataList e Repeater. Per iniziare, creare una nuova cartella nel progetto denominata `DataListRepeaterBasics`. Aggiungere quindi le cinque pagine ASP.NET seguenti a questa cartella, in modo che tutte siano configurate per l'uso della pagina master `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Creare una cartella DataListRepeaterBasics e aggiungere le pagine dell'esercitazione ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: creare una cartella di `DataListRepeaterBasics` e aggiungere le pagine dell'esercitazione ASP.NET

Aprire la pagina `Default.aspx` e trascinare il controllo utente `SectionLevelTutorialListing.ascx` dalla cartella `UserControls` nell'area di progettazione. Questo controllo utente, creato nell'esercitazione [pagine master e navigazione nel sito](../introduction/master-pages-and-site-navigation-cs.md) , enumera la mappa del sito e visualizza le esercitazioni dalla sezione corrente in un elenco puntato.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))

Per fare in modo che l'elenco puntato visualizzi le esercitazioni di DataList e Repeater da creare, è necessario aggiungerle alla mappa del sito. Aprire il file `Web.sitemap` e aggiungere il markup seguente dopo l'aggiunta del markup del nodo della mappa del sito dei pulsanti personalizzati:

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]

![Aggiornare la mappa del sito per includere le nuove pagine ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito per includere le nuove pagine ASP.NET

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Passaggio 2: visualizzazione delle informazioni sul prodotto con DataList

Analogamente a FormView, l'output sottoposto a rendering del controllo DataList dipende da modelli anziché BoundField, CheckBoxFields e così via. Diversamente da FormView, DataList è progettato per visualizzare un set di record anziché uno solitario. Per iniziare questa esercitazione, è possibile esaminare l'associazione delle informazioni sul prodotto a un DataList. Per iniziare, aprire la pagina `Basics.aspx` nella cartella `DataListRepeaterBasics`. Trascinare quindi un oggetto DataList dalla casella degli strumenti nella finestra di progettazione. Come illustrato nella figura 4, prima di specificare i modelli di DataList, la finestra di progettazione lo Visualizza come una casella grigia.

[![trascinare DataList dalla casella degli strumenti nella finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: trascinare DataList dalla casella degli strumenti nella finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))

Dallo smart tag di DataList aggiungere un nuovo ObjectDataSource e configurarlo per l'uso del metodo `ProductsBLL` Class s `GetProducts`. Poiché in questa esercitazione viene ricreato un oggetto DataList di sola lettura, impostare l'elenco a discesa su (nessuno) nelle schede di inserimento, aggiornamento ed eliminazione della procedura guidata.

[![scegliere di creare un nuovo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: scegliere di creare un nuovo ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurare ObjectDataSource per l'uso della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))

[![recuperare informazioni su tutti i prodotti utilizzando il metodo GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperare informazioni su tutti i prodotti usando il metodo `GetProducts` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))

Dopo aver configurato ObjectDataSource e averlo associato a DataList tramite il relativo smart tag, Visual Studio creerà automaticamente una `ItemTemplate` nel DataList che Visualizza il nome e il valore di ogni campo dati restituito dall'origine dati (vedere il markup riportato di seguito). Questo aspetto predefinito `ItemTemplate` s è identico a quello dei modelli creati automaticamente quando si associa un'origine dati a FormView tramite la finestra di progettazione.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Quando si associa un'origine dati a un controllo FormView tramite lo smart tag FormView s, Visual Studio ha creato un `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate`. Con DataList, tuttavia, viene creato solo un `ItemTemplate`. Questo perché DataList non dispone dello stesso supporto di modifica e inserimento incorporato offerto da FormView. Il DataList contiene eventi correlati a modifiche ed eliminazioni e la modifica e l'eliminazione del supporto possono essere aggiunte con un po' di codice, ma non è disponibile un semplice supporto predefinito come con FormView. Si vedrà come includere la modifica e l'eliminazione del supporto con DataList in un'esercitazione futura.

A questo punto, è possibile migliorare l'aspetto di questo modello. Anziché visualizzare tutti i campi dati, consente di visualizzare solo il nome del prodotto, Supplier, Category, Quantity per unit e il prezzo unitario. Inoltre, consente di visualizzare il nome in un'intestazione `<h4>` e di disporre i campi rimanenti utilizzando una `<table>` sotto l'intestazione.

Per apportare queste modifiche, è possibile usare le funzionalità di modifica dei modelli nella finestra di progettazione dallo smart tag di DataList facendo clic sul collegamento modifica modelli oppure è possibile modificare manualmente il modello tramite la sintassi dichiarativa della pagina. Se si usa l'opzione modifica modelli nella finestra di progettazione, il markup risultante potrebbe non corrispondere esattamente al markup seguente, ma quando viene visualizzato tramite un browser dovrebbe essere molto simile allo screenshot illustrato nella figura 8.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> Nell'esempio precedente vengono usati i controlli Web Label la cui proprietà `Text` viene assegnato il valore della sintassi di associazione dati. In alternativa, avremmo potuto omettere completamente le etichette, digitando solo la sintassi di associazione dati. Ovvero, invece di usare `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`, avremmo potuto usare invece la sintassi dichiarativa `<%# Eval("CategoryName") %>`.

L'uscita dai controlli Web Label offre tuttavia due vantaggi. In primo luogo, fornisce un mezzo più semplice per formattare i dati in base ai dati, come si vedrà nell'esercitazione successiva. In secondo luogo, l'opzione modifica modelli nella finestra di progettazione non Visualizza la sintassi di associazione dati dichiarativa visualizzata all'esterno di un controllo Web. Al contrario, l'interfaccia modifica modelli è progettata per semplificare l'utilizzo del markup statico e dei controlli Web e presuppone che l'associazione dati venga eseguita tramite la finestra di dialogo Modifica DataBindings, accessibile dagli smart tag dei controlli Web.

Pertanto, quando si utilizza DataList, che consente di modificare i modelli tramite la finestra di progettazione, è preferibile utilizzare i controlli Web Label, in modo che il contenuto sia accessibile tramite l'interfaccia modifica modelli. Come si vedrà a breve, il ripetitore richiede che il contenuto del modello venga modificato dalla visualizzazione origine. Di conseguenza, quando si creano i modelli del Repeater, spesso si ometteranno i controlli Web dell'etichetta, a meno che non sia necessario formattare l'aspetto del testo associato ai dati in base alla logica a livello di codice.

[![viene eseguito il rendering dell'output di ogni prodotto utilizzando il servizio DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: viene eseguito il rendering dell'output di ogni prodotto utilizzando il `ItemTemplate` DataList ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Passaggio 3: miglioramento dell'aspetto di DataList

Come GridView, DataList offre diverse proprietà correlate allo stile, ad esempio `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e così via. Quando si usano i controlli GridView e DetailsView, sono stati creati file di interfaccia nel tema `DataWebControls` che predefinivano le proprietà `CssClass` per questi due controlli e la proprietà `CssClass` per diverse proprietà (`RowStyle`, `HeaderStyle`e così via). Consente di eseguire la stessa operazione per DataList.

Come illustrato nell'esercitazione [visualizzazione dei dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , un file di interfaccia specifica le proprietà correlate all'aspetto predefinito per un controllo Web. un tema è una raccolta di file skin, CSS, image e JavaScript che definiscono un aspetto specifico per un sito Web. Nell'esercitazione *visualizzazione dei dati con ObjectDataSource* è stato creato un tema `DataWebControls` (implementato come una cartella all'interno della cartella `App_Themes`) che dispone attualmente di due file di interfaccia, `GridView.skin` e `DetailsView.skin`. Consente di aggiungere un terzo file di skin per specificare le impostazioni di stile predefinite per DataList.

Per aggiungere un file di interfaccia personalizzata, fare clic con il pulsante destro del mouse sulla cartella `App_Themes/DataWebControls`, scegliere Aggiungi un nuovo elemento e selezionare l'opzione file di interfaccia dall'elenco. Denominare il file `DataList.skin`.

[![creare un nuovo file di interfaccia con nome DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: creare un nuovo file di interfaccia personalizzata denominato `DataList.skin` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))

Usare il markup seguente per il file di `DataList.skin`:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Queste impostazioni assegnano le stesse classi CSS alle proprietà di DataList appropriate come sono state usate con i controlli GridView e DetailsView. Le classi CSS utilizzate in questo `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`e così via sono definite nel file `Styles.css` e sono state aggiunte nelle esercitazioni precedenti.

Con l'aggiunta di questo file di interfaccia, l'aspetto di DataList viene aggiornato nella finestra di progettazione. potrebbe essere necessario aggiornare la visualizzazione di progettazione per visualizzare gli effetti del nuovo file di interfaccia. dal menu Visualizza scegliere Aggiorna. Come illustrato nella figura 10, ogni prodotto alternato ha un colore di sfondo rosa chiaro.

[![creare un nuovo file di interfaccia con nome DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: creare un nuovo file di interfaccia personalizzata denominato `DataList.skin` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Passaggio 4: esplorazione degli altri modelli di DataList

Oltre alla `ItemTemplate`, DataList supporta sei altri modelli facoltativi:

- `HeaderTemplate` se specificato, aggiunge una riga di intestazione all'output e viene utilizzata per eseguire il rendering di questa riga.
- `AlternatingItemTemplate` utilizzato per eseguire il rendering di elementi alternativi
- `SelectedItemTemplate` utilizzato per eseguire il rendering dell'elemento selezionato; l'elemento selezionato è l'elemento il cui indice corrisponde alla [proprietà`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx) di DataList
- `EditItemTemplate` utilizzato per eseguire il rendering dell'elemento modificato
- `SeparatorTemplate` se specificato, aggiunge un separatore tra ogni elemento e viene usato per eseguire il rendering del separatore
- `FooterTemplate`: se specificato, aggiunge una riga di piè di pagina all'output e viene utilizzata per eseguire il rendering di questa riga.

Quando si specifica il `HeaderTemplate` o `FooterTemplate`, DataList aggiunge un'intestazione o una riga del piè di pagina aggiuntiva all'output sottoposto a rendering. Analogamente alle righe dell'intestazione e del piè di pagina di GridView, l'intestazione e il piè di pagina di un controllo DataList non sono associati ai dati. Pertanto, qualsiasi sintassi di associazione dati nel `HeaderTemplate` o `FooterTemplate` che tenta di accedere ai dati associati restituirà una stringa vuota.

> [!NOTE]
> Come illustrato nella visualizzazione delle [informazioni di riepilogo nell'esercitazione del piè](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) di pagina di GridView, mentre le righe di intestazione e piè di pagina Don t supportano la sintassi di associazione dati, le informazioni specifiche dei dati possono essere inserite direttamente in queste righe dal gestore dell'evento gridview s `RowDataBound`. Questa tecnica può essere usata per calcolare i totali in esecuzione o altre informazioni dai dati associati al controllo, oltre ad assegnare tali informazioni al piè di pagina. Questo stesso concetto può essere applicato ai controlli DataList e Repeater; l'unica differenza è che per DataList e Repeater viene creato un gestore eventi per l'evento `ItemDataBound` (anziché per l'evento `RowDataBound`).

Per questo esempio, è possibile fare in modo che le informazioni sul prodotto del titolo siano visualizzate nella parte superiore dei risultati di DataList in un'intestazione di `<h3>`. A tale scopo, aggiungere un `HeaderTemplate` con il markup appropriato. Dalla finestra di progettazione questa operazione può essere eseguita facendo clic sul collegamento modifica modelli nello smart tag di DataList, scegliendo il modello di intestazione nell'elenco a discesa e digitando il testo dopo aver selezionato l'opzione titolo 3 dall'elenco a discesa stile (vedere la figura 11).

[![aggiungere un oggetto HeaderTemplate con le informazioni sul prodotto di testo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: aggiungere un `HeaderTemplate` con le informazioni sul prodotto di testo ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))

In alternativa, può essere aggiunto in modo dichiarativo immettendo il markup seguente all'interno dei tag `<asp:DataList>`:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Per aggiungere uno spazio tra ogni listato di prodotti, aggiungere un `SeparatorTemplate` che includa una linea tra le singole sezioni. Il tag della regola orizzontale (`<hr>`) aggiunge un divisore di questo tipo. Creare il `SeparatorTemplate` in modo che includa il markup seguente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Analogamente a `HeaderTemplate` e `FooterTemplates`, il `SeparatorTemplate` non è associato ad alcun record dell'origine dati e pertanto non può accedere direttamente ai record dell'origine dati associati a DataList.

Dopo aver apportato questa aggiunta, quando si visualizza la pagina tramite un browser, l'aspetto dovrebbe essere simile a quello della figura 12. Prendere nota della riga di intestazione e della riga tra ogni listato di prodotti.

[![DataList include una riga di intestazione e una regola orizzontale tra ogni listato di prodotti](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: il DataList include una riga di intestazione e una regola orizzontale tra ogni listato[di prodotti (fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Passaggio 5: rendering di markup specifico con il controllo Repeater

Se si esegue una visualizzazione o un'origine dal browser quando si visita l'esempio di DataList della figura 12, si noterà che DataList genera un `<table>` HTML che contiene una riga di tabella (`<tr>`) con una singola cella della tabella (`<td>`) per ogni elemento associato a DataList. Questo output, infatti, è identico a quello che verrebbe generato da un controllo GridView con un singolo TemplateField. Come si vedrà in un'esercitazione futura, DataList consente di personalizzare ulteriormente l'output, consentendo di visualizzare più record di origini dati per riga di tabella.

Cosa accade se non si vuole creare un `<table>`HTML? Per il controllo totale e completo sul markup generato da un controllo Web dati, è necessario usare il controllo Repeater. Analogamente a DataList, il ripetitore viene costruito in base ai modelli. Il Repeater, tuttavia, offre solo i cinque modelli seguenti:

- `HeaderTemplate` se specificato, aggiunge il markup specificato prima degli elementi
- `ItemTemplate` utilizzato per eseguire il rendering degli elementi
- `AlternatingItemTemplate` se specificato, usato per eseguire il rendering di elementi alternativi
- `SeparatorTemplate` se specificato, aggiunge il markup specificato tra ogni elemento
- `FooterTemplate`: se specificato, aggiunge il markup specificato dopo gli elementi

In ASP.NET 1. x, il controllo Repeater veniva utilizzato comunemente per visualizzare un elenco puntato i cui dati provengono da un'origine dati. In tal caso, il `HeaderTemplate` e il `FooterTemplates` contengono rispettivamente i tag di `<ul>` di apertura e di chiusura, mentre il `ItemTemplate` conterrebbe gli elementi `<li>` con la sintassi di associazione dati. Questo approccio può comunque essere usato in ASP.NET 2,0, come illustrato in due esempi nell'esercitazione sulle [pagine master e sull'esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) :

- Nella pagina master `Site.master` è stato usato un ripetitore per visualizzare un elenco puntato del contenuto della mappa del sito di livello superiore (report di base, filtraggio dei report, formattazione personalizzata e così via); è stato usato un altro Repeater annidato per visualizzare le sezioni figlio delle sezioni di primo livello
- In `SectionLevelTutorialListing.ascx`, è stato usato un ripetitore per visualizzare un elenco puntato delle sezioni figlio della sezione della mappa del sito corrente

> [!NOTE]
> ASP.NET 2,0 introduce il nuovo [controllo BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), che può essere associato a un controllo origine dati per visualizzare un semplice elenco puntato. Con il controllo BulletedList non è necessario specificare alcun codice HTML correlato all'elenco; al contrario, è sufficiente indicare il campo dati da visualizzare come testo per ogni elemento dell'elenco.

Il ripetitore funge da controllo Web catch all data. Se non è presente un controllo che genera il markup necessario, è possibile usare il controllo Repeater. Per illustrare l'uso del Repeater, è necessario che l'elenco delle categorie sia visualizzato sopra il DataList delle informazioni sul prodotto creato nel passaggio 2. In particolare, le categorie vengono visualizzate in un `<table>` HTML a riga singola con ogni categoria visualizzata come colonna nella tabella.

A tale scopo, iniziare trascinando un controllo Repeater dalla casella degli strumenti nella finestra di progettazione, sopra il DataList informazioni sul prodotto. Come per DataList, il Repeater viene inizialmente visualizzato come una casella grigia fino a quando non sono stati definiti i relativi modelli.

[![aggiungere un Repeater alla finestra di progettazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: aggiungere un Repeater alla finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))

Nello smart tag del Repeater è presente una sola opzione: scegliere l'origine dati. Scegliere di creare un nuovo ObjectDataSource e configurarlo per l'uso del metodo `CategoriesBLL` Class s `GetCategories`.

[![creare un nuovo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figura 14**: creare un nuovo ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))

[![configurare ObjectDataSource per l'utilizzo della classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurare ObjectDataSource per l'uso della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))

[![recuperare informazioni su tutte le categorie utilizzando il metodo GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperare informazioni su tutte le categorie usando il metodo `GetCategories` ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))

A differenza di DataList, Visual Studio non crea automaticamente un ItemTemplate per il ripetitore dopo l'associazione a un'origine dati. Inoltre, i modelli Repeater non possono essere configurati tramite la finestra di progettazione e devono essere specificati in modo dichiarativo.

Per visualizzare le categorie come `<table>` a riga singola con una colonna per ogni categoria, è necessario che il Repeater emetta un markup simile al seguente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Poiché il testo `<td>Category X</td>` è la parte che si ripete, questo verrà visualizzato nell'ItemTemplate del Repeater. Il markup che viene visualizzato prima di `<table><tr>`-verrà inserito nel `HeaderTemplate` mentre il markup finale-`</tr></table>`-verrà inserito nel `FooterTemplate`. Per immettere queste impostazioni del modello, passare alla parte dichiarativa della pagina ASP.NET facendo clic sul pulsante origine nell'angolo in basso a sinistra e digitare la sintassi seguente:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Il Repeater emette il markup preciso come specificato dai relativi modelli, nient'altro, nient'altro. Nella figura 17 viene illustrato l'output del Repeater quando viene visualizzato tramite un browser.

[![tabella &lt;HTML A riga singola&gt; elenca ogni categoria in una colonna separata](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: un `<table>` HTML a riga singola elenca ogni categoria in una colonna separata ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Passaggio 6: miglioramento dell'aspetto del Repeater

Poiché il Repeater emette esattamente il markup specificato dai relativi modelli, non dovrebbe sorprendere il fatto che non esistano proprietà correlate allo stile per il ripetitore. Per modificare l'aspetto del contenuto generato dal Repeater, è necessario aggiungere manualmente il contenuto HTML o CSS necessario direttamente ai modelli del ripetitore.

Per questo esempio, è possibile usare le colonne Category per i colori di sfondo alternativi, come con le righe alternate in DataList. A tale scopo, è necessario assegnare la classe `RowStyle` CSS a ogni elemento del ripetitore e la classe `AlternatingRowStyle` CSS a ogni elemento del ripetitore alternato tramite i modelli `ItemTemplate` e `AlternatingItemTemplate`, come segue:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Consente inoltre di aggiungere una riga di intestazione all'output con le categorie di prodotti di testo. Poiché non sappiamo quante colonne saranno costituite dal `<table>` risultante, il modo più semplice per generare una riga di intestazione che garantisce l'estensione di tutte le colonne consiste nell'usare *due* `<table>` s. Il primo `<table>` conterrà due righe la riga di intestazione e una riga che conterrà la seconda `<table>` a riga singola con una colonna per ogni categoria del sistema. Ovvero si vuole creare il markup seguente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Il `HeaderTemplate` e il `FooterTemplate` seguenti generano il markup desiderato:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Nella figura 18 viene illustrato il ripetitore dopo che queste modifiche sono state apportate.

[![le colonne di categoria si alternano al colore di sfondo e include una riga di intestazione](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: le colonne di categoria si alternano al colore di sfondo e includono una riga di intestazione ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))

## <a name="summary"></a>Riepilogo

Sebbene il controllo GridView semplifica la visualizzazione, la modifica, l'eliminazione, l'ordinamento e la paginazione dei dati, l'aspetto è molto semplice e simile a una griglia. Per un maggiore controllo sull'aspetto, è necessario trasformare i controlli DataList o Repeater. Entrambi i controlli visualizzano un set di record usando i modelli anziché i BoundField, CheckBoxFields e così via.

Il controllo DataList viene eseguito come `<table>` HTML che, per impostazione predefinita, Visualizza ogni record dell'origine dati in una singola riga della tabella, come un controllo GridView con un singolo TemplateField. Come si vedrà in un'esercitazione futura, tuttavia, DataList consente la visualizzazione di più record per riga di tabella. Il Repeater, d'altra parte, emette rigorosamente il markup specificato nei modelli; non aggiunge alcun markup aggiuntivo e pertanto viene comunemente usato per visualizzare i dati negli elementi HTML diversi da un `<table>`, ad esempio in un elenco puntato.

Sebbene DataList e Repeater offrano maggiore flessibilità nell'output di cui è stato eseguito il rendering, non dispongono di molte delle funzionalità predefinite disponibili in GridView. Come si esamineranno nelle esercitazioni future, alcune di queste funzionalità possono essere riportate senza troppi sforzi, ma tenere presente che l'utilizzo di DataList o Repeater al posto di GridView limita le funzionalità che è possibile utilizzare senza dover implementare tali funzionalità. te.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
