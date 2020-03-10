---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Paging e ordinamento dei dati del reportC#() | Microsoft Docs
author: rick-anderson
description: Il paging e l'ordinamento sono due funzionalità molto comuni quando si visualizzano i dati in un'applicazione online. In questa esercitazione verrà esaminato prima di tutto l'aggiunta di ordinamento e...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f77040316dadc218b8183e52628dc0cfe3b35a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78619518"
---
# <a name="paging-and-sorting-report-data-c"></a>Suddivisione in pagine e ordinamento dei dati dei report (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) o [scaricare il file PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Il paging e l'ordinamento sono due funzionalità molto comuni quando si visualizzano i dati in un'applicazione online. In questa esercitazione verrà esaminato in modo approfondito l'aggiunta di ordinamento e paging ai report, che verranno quindi compilati in esercitazioni future.

## <a name="introduction"></a>Introduzione

Il paging e l'ordinamento sono due funzionalità molto comuni quando si visualizzano i dati in un'applicazione online. Ad esempio, quando si cercano libri ASP.NET in una libreria online, è possibile che siano presenti centinaia di libri di questo tipo, ma il report che elenca i risultati della ricerca elenca solo dieci corrispondenze per pagina. Inoltre, i risultati possono essere ordinati in base a titolo, prezzo, conteggio delle pagine, nome dell'autore e così via. Mentre le ultime 23 esercitazioni hanno esaminato come creare un'ampia gamma di report, incluse le interfacce che consentono l'aggiunta, la modifica e l'eliminazione di dati, non è stato esaminato come ordinare i dati e gli unici esempi di paging riscontrati sono stati con DetailsView e FormView. controlli.

In questa esercitazione verrà illustrato come aggiungere ordinamento e paging ai report, che possono essere eseguiti semplicemente selezionando alcune caselle di controllo. Sfortunatamente, questa implementazione semplicistica ha svantaggi che l'interfaccia di ordinamento lascia un po' di desiderio e le routine di paging non sono progettate per eseguire il paging in modo efficiente tramite set di risultati di grandi dimensioni. Nelle esercitazioni future viene illustrato come superare le limitazioni delle soluzioni di paging e ordinamento predefinite.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: aggiunta delle pagine Web dell'esercitazione per il paging e l'ordinamento

Prima di iniziare questa esercitazione, è necessario prima di tutto aggiungere le pagine ASP.NET necessarie per questa esercitazione e le tre successive. Per iniziare, creare una nuova cartella nel progetto denominata `PagingAndSorting`. Aggiungere quindi le cinque pagine ASP.NET seguenti a questa cartella, in modo che tutte siano configurate per l'uso della pagina master `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Creare una cartella PagingAndSorting e aggiungere le pagine dell'esercitazione ASP.NET](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: creare una cartella PagingAndSorting e aggiungere le pagine dell'esercitazione ASP.NET

Aprire quindi la pagina `Default.aspx` e trascinare il controllo utente `SectionLevelTutorialListing.ascx` dalla cartella `UserControls` nell'area di progettazione. Questo controllo utente, creato nell'esercitazione [pagine master e navigazione nel sito](../introduction/master-pages-and-site-navigation-cs.md) , enumera la mappa del sito e visualizza le esercitazioni nella sezione corrente di un elenco puntato.

![Aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx

Per fare in modo che l'elenco puntato visualizzi le esercitazioni di paging e ordinamento da creare, è necessario aggiungerle alla mappa del sito. Aprire il file di `Web.sitemap` e aggiungere il markup seguente dopo il markup di modifica, inserimento ed eliminazione del nodo della mappa del sito:

[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]

![Aggiornare la mappa del sito per includere le nuove pagine ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: aggiornare la mappa del sito per includere le nuove pagine ASP.NET

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Passaggio 2: visualizzazione delle informazioni sul prodotto in un controllo GridView

Prima di implementare effettivamente le funzionalità di paging e ordinamento, è necessario creare prima un GridView standard non ordinabile e non paginabile che elenca le informazioni sul prodotto. Si tratta di un'attività eseguita più volte prima di tutta questa serie di esercitazioni, in modo da acquisire familiarità con questa procedura. Per iniziare, aprire la pagina `SimplePagingSorting.aspx` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa proprietà `ID` su `Products`. Successivamente, creare un nuovo ObjectDataSource che usa il metodo della classe ProductsBLL `GetProducts()` per restituire tutte le informazioni sul prodotto.

![Recuperare informazioni su tutti i prodotti usando il metodo GetProducts ()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperare informazioni su tutti i prodotti usando il metodo GetProducts ()

Poiché questo report è un report di sola lettura, non è necessario eseguire il mapping dei metodi di `Insert()`, `Update()`o `Delete()` di ObjectDataSource ai metodi di `ProductsBLL` corrispondenti; Pertanto, scegliere (nessuno) dall'elenco a discesa per le schede Aggiorna, Inserisci ed Elimina.

![Scegliere l'opzione (nessuno) nell'elenco a discesa nelle schede Aggiorna, Inserisci ed Elimina.](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: scegliere l'opzione (nessuno) nell'elenco a discesa nelle schede di aggiornamento, inserimento ed eliminazione

A questo punto, è consigliabile personalizzare i campi di GridView in modo che vengano visualizzati solo i nomi dei prodotti, i fornitori, le categorie, i prezzi e gli stati sospesi. Inoltre, è possibile apportare modifiche alla formattazione a livello di campo, ad esempio modificare le proprietà del `HeaderText` o formattare il prezzo come valuta. Una volta apportate queste modifiche, il markup dichiarativo di GridView dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

La figura 6 Mostra lo stato di avanzamento fino a questo punto quando viene visualizzato tramite un browser. Si noti che la pagina elenca tutti i prodotti in un'unica schermata, mostrando il nome del prodotto, la categoria, il fornitore, il prezzo e lo stato interrotto.

[![tutti i prodotti sono elencati](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: tutti i prodotti sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Passaggio 3: aggiunta del supporto per il paging

L'elenco di *tutti* i prodotti in una schermata può comportare un sovraccarico delle informazioni per l'utente che esamina i dati. Per semplificare la gestione dei risultati, è possibile suddividere i dati in pagine di dati più piccole e consentire all'utente di scorrere i dati una pagina alla volta. Per eseguire questa operazione, è sufficiente selezionare la casella di controllo Abilita paging dello smart tag di GridView (in questo modo la [proprietà`AllowPaging`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) GridView s viene impostata su `true`).

[![selezionare la casella di controllo Abilita paging per aggiungere il supporto per il paging](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: selezionare la casella di controllo Abilita paging per aggiungere il supporto per il paging ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image11.png))

L'abilitazione del paging limita il numero di record visualizzati per pagina e aggiunge un' *interfaccia di paging* a GridView. L'interfaccia di paging predefinita, mostrata nella figura 7, è costituita da una serie di numeri di pagina che consentono all'utente di spostarsi rapidamente da una pagina di dati a un'altra. Questa interfaccia di paging dovrebbe avere un aspetto familiare, come abbiamo visto quando si aggiunge il supporto per il paging ai controlli DetailsView e FormView nelle esercitazioni precedenti.

Entrambi i controlli DetailsView e FormView visualizzano solo un singolo record per ogni pagina. GridView, tuttavia, consulta la relativa [proprietà`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) per determinare il numero di record da visualizzare per pagina (il valore predefinito di questa proprietà è 10).

È possibile personalizzare l'interfaccia di paging GridView, DetailsView e FormView usando le proprietà seguenti:

- `PagerStyle` indica le informazioni sullo stile per l'interfaccia di paging; può specificare impostazioni come `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e così via.
- `PagerSettings` contiene un stuolo di proprietà che possono personalizzare le funzionalità dell'interfaccia di paging; `PageButtonCount` indica il numero massimo di numeri di pagina numerici visualizzati nell'interfaccia di paging (il valore predefinito è 10); la [proprietà`Mode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica il funzionamento dell'interfaccia di paging e può essere impostata su: 

    - `NextPrevious` Mostra i pulsanti avanti e indietro, consentendo all'utente di eseguire il passaggio o indietro di una pagina alla volta
    - `NextPreviousFirstLast` oltre ai pulsanti avanti e indietro, sono inclusi anche il primo e l'ultimo pulsante, che consentono all'utente di passare rapidamente alla prima o all'ultima pagina di dati
    - `Numeric` Mostra una serie di numeri di pagina, consentendo all'utente di passare immediatamente a qualsiasi pagina
    - `NumericFirstLast` oltre ai numeri di pagina, include il primo e l'ultimo pulsante, consentendo all'utente di passare rapidamente alla prima o all'ultima pagina di dati; il primo/ultimo pulsante viene visualizzato solo se non è possibile adattare tutti i numeri di pagina numerici

Inoltre, GridView, DetailsView e FormView offrono tutte le proprietà `PageIndex` e `PageCount`, che indicano rispettivamente la pagina corrente e il numero totale di pagine di dati. La proprietà `PageIndex` è indicizzata a partire da 0, vale a dire che, quando si visualizza la prima pagina di dati `PageIndex` sarà uguale a 0. `PageCount`, d'altra parte, inizia il conteggio da 1, vale a dire che `PageIndex` è limitato ai valori compresi tra 0 e `PageCount - 1`.

A questo punto, è possibile migliorare l'aspetto predefinito dell'interfaccia di paging di GridView. In particolare, è possibile fare in modo che l'interfaccia di paging sia allineata a destra con uno sfondo grigio chiaro. Anziché impostare queste proprietà direttamente tramite la proprietà `PagerStyle` di GridView, creare una classe CSS in `Styles.css` denominata `PagerRowStyle` e quindi assegnare la proprietà `PagerStyle` s `CssClass` tramite il tema. Per iniziare, aprire `Styles.css` e aggiungere la seguente definizione di classe CSS:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Successivamente, aprire il file `GridView.skin` nella cartella `DataWebControls` all'interno della cartella `App_Themes`. Come illustrato nell'esercitazione sulle *pagine master e sulla navigazione nel sito* , è possibile usare i file di interfaccia per specificare i valori di proprietà predefiniti per un controllo Web. Pertanto, è possibile aumentare le impostazioni esistenti per includere l'impostazione della proprietà `CssClass` di `PagerStyle` per `PagerRowStyle`. Consente inoltre di configurare l'interfaccia di paging in modo da visualizzare al massimo cinque pulsanti di pagina numerici utilizzando l'interfaccia di paging `NumericFirstLast`.

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Esperienza utente di paging

Nella figura 8 viene illustrata la pagina Web visitata tramite un browser dopo che la casella di controllo Abilita paging di GridView s è stata selezionata e le configurazioni `PagerStyle` e `PagerSettings` sono state effettuate tramite il file `GridView.skin`. Si noti come vengono visualizzati solo dieci record e l'interfaccia di paging indica che viene visualizzata la prima pagina di dati.

[![con il paging abilitato, viene visualizzato solo un subset di record alla volta](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: con il paging abilitato, viene visualizzato solo un subset di record alla volta ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image14.png))

Quando l'utente fa clic su uno dei numeri di pagina nell'interfaccia di paging, si verifica un postback e il caricamento della pagina mostra i record della pagina richiesti. La figura 9 Mostra i risultati dopo aver scelto di visualizzare la pagina finale dei dati. Si noti che la pagina finale contiene un solo record; Ciò è dovuto al fatto che in totale sono presenti 81 record, ottenendo otto pagine di 10 record per pagina, più una pagina con un record solitario.

[![fare clic su un numero di pagina causa un postback e Mostra il subset appropriato di record](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: se si fa clic su un numero di pagina si verifica un postback e viene visualizzato il subset appropriato di record ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Flusso di lavoro lato server di paging

Quando l'utente finale fa clic su un pulsante nell'interfaccia di paging, si verifica un postback e viene avviato il flusso di lavoro lato server seguente:

1. Viene generato l'evento GridView s (o DetailsView o FormView) `PageIndexChanging`
2. ObjectDataSource richiede nuovamente *tutti* i dati dal BLL; i valori delle proprietà `PageIndex` e `PageSize` di GridView vengono utilizzati per determinare quali record restituiti dall'elemento BLL devono essere visualizzati in GridView
3. L'evento `PageIndexChanged` GridView s viene attivato

Nel passaggio 2, ObjectDataSource richiede nuovamente tutti i dati dalla relativa origine dati. Questo stile di paging è comunemente noto come *paging predefinito*, in quanto è il comportamento di paging usato per impostazione predefinita quando si imposta la proprietà `AllowPaging` su `true`. Con il paging predefinito, il controllo Web dei dati recupera in modo ingenuo tutti i record per ogni pagina di dati, anche se viene effettivamente eseguito il rendering solo di un subset di record nel codice HTML inviato al browser. A meno che i dati del database non vengano memorizzati nella cache da BLL o ObjectDataSource, il paging predefinito non è utilizzabile per set di risultati di dimensioni sufficientemente grandi o per applicazioni Web con molti utenti simultanei.

Nell'esercitazione successiva si esaminerà come implementare il *paging personalizzato*. Con il paging personalizzato è possibile indicare in modo specifico a ObjectDataSource di recuperare solo il set preciso di record necessari per la pagina di dati richiesta. Come si può immaginare, il paging personalizzato migliora notevolmente l'efficienza del paging tramite set di risultati di grandi dimensioni.

> [!NOTE]
> Mentre il paging predefinito non è adatto per il paging di set di risultati di dimensioni sufficientemente grandi o per siti con molti utenti simultanei, tenere presente che il paging personalizzato richiede un maggior numero di modifiche e un impegno di implementazione e non è semplice come selezionare una casella di controllo (come predefinito paging). Pertanto, il paging predefinito può essere la scelta ideale per siti Web di piccole dimensioni e a basso traffico o durante il paging di set di risultati relativamente piccoli, perché risulta molto più semplice e veloce da implementare.

Se, ad esempio, si è certi che nel database non saranno mai presenti più di 100 prodotti, il miglioramento minimo delle prestazioni di cui è stato eseguito il paging personalizzato è probabilmente offset in base al lavoro richiesto per implementarlo. Se, tuttavia, è possibile che un giorno sia costituito da migliaia o decine di migliaia di prodotti, la *mancata* implementazione del paging personalizzato comprometterebbe in modo sostanziale la scalabilità dell'applicazione.

## <a name="step-4-customizing-the-paging-experience"></a>Passaggio 4: personalizzazione dell'esperienza di paging

I controlli Web di dati forniscono una serie di proprietà che possono essere usate per migliorare l'esperienza di paging dell'utente. La proprietà `PageCount`, ad esempio, indica il numero di pagine totali, mentre la proprietà `PageIndex` indica che la pagina corrente viene visitata e può essere impostata per spostare rapidamente un utente in una pagina specifica. Per illustrare come usare queste proprietà per migliorare l'esperienza di paging degli utenti, aggiungere un controllo Web Label alla pagina che informa l'utente della pagina che è attualmente in visita, insieme a un controllo DropDownList che consente di passare rapidamente a una pagina specifica .

In primo luogo, aggiungere un controllo Web Label alla pagina, impostare la relativa proprietà `ID` su `PagingInformation`e deselezionare la relativa proprietà `Text`. Successivamente, creare un gestore eventi per l'evento GridView s `DataBound` e aggiungere il codice seguente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Questo gestore dell'evento assegna la proprietà `PagingInformation` label s `Text` a un messaggio che informa l'utente che la pagina visitata `Products.PageIndex + 1` fuori dal numero totale di pagine `Products.PageCount` (si aggiunge 1 alla proprietà `Products.PageIndex` perché `PageIndex` è indicizzata a partire da 0). Si è scelto di assegnare questa etichetta `Text` proprietà nel gestore dell'evento `DataBound` invece che nel gestore dell'evento `PageIndexChanged` perché l'evento `DataBound` viene attivato ogni volta che i dati vengono associati a GridView mentre il gestore eventi `PageIndexChanged` viene attivato solo quando viene modificato l'indice della pagina. Quando GridView è inizialmente associato ai dati nella prima visita della pagina, l'evento `PageIndexChanging` non viene attivato, mentre l'evento `DataBound` lo fa.

A questo punto, all'utente viene visualizzato un messaggio che indica la pagina visitata e il numero totale di pagine di dati.

[![viene visualizzato il numero di pagina corrente e il numero totale di pagine](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: vengono visualizzati il numero di pagina corrente e il numero totale di pagine ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image20.png))

Oltre al controllo Label, consente di aggiungere anche un controllo DropDownList che elenca i numeri di pagina in GridView con la pagina attualmente visualizzata selezionata. L'idea è che l'utente possa passare rapidamente dalla pagina corrente a un'altra selezionando semplicemente il nuovo indice della pagina dal DropDownList. Per iniziare, aggiungere un controllo DropDownList alla finestra di progettazione, impostare la relativa proprietà `ID` su `PageList` e selezionare l'opzione Abilita AutoPostBack dallo smart tag.

Tornare quindi al gestore dell'evento `DataBound` e aggiungere il codice seguente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Questo codice inizia deselezionando gli elementi nel `PageList` DropDownList. Questo può sembrare superfluo, perché non si prevede che il numero di pagine venga modificato, ma è possibile che altri utenti utilizzino il sistema simultaneamente, aggiungendo o rimuovendo i record dalla tabella `Products`. Tali inserimenti o eliminazioni potrebbero modificare il numero di pagine di dati.

A questo punto, è necessario creare nuovamente i numeri di pagina e fare in modo che il mapping al GridView corrente `PageIndex` selezionato per impostazione predefinita. Questa operazione viene eseguita con un ciclo compreso tra 0 e `PageCount - 1`, aggiungendo una nuova `ListItem` in ogni iterazione e impostando la relativa proprietà `Selected` su true se l'indice di iterazione corrente è uguale alla proprietà `PageIndex` di GridView.

Infine, è necessario creare un gestore eventi per l'evento `SelectedIndexChanged` DropDownList, che viene attivato ogni volta che l'utente seleziona un elemento diverso dall'elenco. Per creare questo gestore eventi, è sufficiente fare doppio clic sul DropDownList nella finestra di progettazione, quindi aggiungere il codice seguente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Come illustrato nella figura 11, la semplice modifica della proprietà `PageIndex` di GridView causa la riassociazione dei dati al controllo GridView. Nel gestore dell'evento GridView s `DataBound` è selezionato il `ListItem` DropDownList appropriato.

[![l'utente viene automaticamente immesso nella sesta pagina quando si seleziona l'elemento dell'elenco a discesa pagina 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: l'utente viene automaticamente visualizzata alla sesta pagina quando si seleziona l'elemento dell'elenco a discesa pagina 6 ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image23.png))

## <a name="step-5-adding-bi-directional-sorting-support"></a>Passaggio 5: aggiunta del supporto per l'ordinamento bidirezionale

L'aggiunta del supporto per l'ordinamento bidirezionale è semplice quanto l'aggiunta del supporto di paging è sufficiente selezionare l'opzione Abilita ordinamento dallo smart tag di GridView (che imposta la [proprietà`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) di gridview su `true`). In questo modo tutte le intestazioni dei campi di GridView vengono visualizzate come LinkButton che, quando selezionate, provocano un postback e restituiscono i dati ordinati in base alla colonna selezionata in ordine crescente. Facendo clic sulla stessa intestazione LinkButton, i dati vengono nuovamente ordinati in ordine decrescente.

> [!NOTE]
> Se si usa un livello di accesso ai dati personalizzato anziché un DataSet tipizzato, è possibile che non si disponga di un'opzione Abilita ordinamento nello smart tag di GridView. Questa casella di controllo è disponibile solo per le origini dati associate a GridView che supportano in modo nativo l'ordinamento. Il set di dati tipizzato fornisce il supporto per l'ordinamento predefinito, perché ADO.NET DataTable fornisce un metodo `Sort` che, quando viene richiamato, Ordina le DataRows di DataTable usando i criteri specificati.

Se il DAL non restituisce oggetti che supportano in modo nativo l'ordinamento, è necessario configurare ObjectDataSource per passare le informazioni di ordinamento al livello della logica di business, che può ordinare i dati o avere i dati ordinati in base a DAL. Verranno illustrate le procedure per ordinare i dati ai livelli di logica di business e di accesso ai dati in un'esercitazione futura.

Il rendering degli LinkButton di ordinamento viene eseguito come collegamenti ipertestuali HTML, i cui colori correnti (blu per un collegamento non visitato e un rosso scuro per un collegamento visitato) si scontrano con il colore di sfondo della riga di intestazione. Al contrario, è possibile visualizzare tutti i collegamenti di riga di intestazione in bianco, indipendentemente dal fatto che siano stati visitati o meno. Questa operazione può essere eseguita aggiungendo quanto segue alla classe `Styles.css`:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Questa sintassi indica di usare testo bianco quando si visualizzano i collegamenti ipertestuali all'interno di un elemento che usa la classe header.

Dopo l'aggiunta di questo CSS, quando si visita la pagina tramite un browser, la schermata dovrebbe essere simile alla figura 12. In particolare, la figura 12 Mostra i risultati dopo aver fatto clic sul collegamento di intestazione del campo del prezzo.

[![i risultati sono stati ordinati in base a PrezzoUnitario in ordine crescente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: i risultati sono stati ordinati in base a PrezzoUnitario in ordine crescente ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Analisi del flusso di lavoro di ordinamento

Tutti i campi GridView i BoundField, CheckBoxField, TemplateField e così via hanno una proprietà `SortExpression` che indica l'espressione da usare per ordinare i dati quando si fa clic sul collegamento dell'intestazione di ordinamento del campo. GridView dispone inoltre di una proprietà `SortExpression`. Quando si fa clic su un LinkButton di intestazione di ordinamento, GridView assegna tale campo a `SortExpression` valore alla relativa proprietà `SortExpression`. Successivamente, i dati vengono recuperati nuovamente da ObjectDataSource e ordinati in base alla proprietà `SortExpression` di GridView. Nell'elenco seguente viene illustrata la sequenza di passaggi che si verifica quando un utente finale Ordina i dati in un controllo GridView:

1. L'evento di [ordinamento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) di GridView s viene attivato
2. La [proprietà`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) di GridView è impostata sul `SortExpression` del campo di cui è stato fatto clic con l'intestazione LinkButton di ordinamento.
3. Il controllo ObjectDataSource recupera tutti i dati dal BLL e quindi Ordina i dati usando GridView s `SortExpression`
4. La proprietà `PageIndex` GridView s viene reimpostata su 0. Ciò significa che, quando l'ordinamento dell'utente viene restituito alla prima pagina di dati (presupponendo che sia stato implementato il supporto per il paging)
5. L' [evento`Sorted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) GridView s viene attivato

Analogamente al paging predefinito, l'opzione di ordinamento predefinita recupera nuovamente *tutti* i record dal BLL. Quando si utilizza l'ordinamento senza paging o quando si utilizza l'ordinamento con il paging predefinito, non esiste alcun modo per aggirare questo impatto sulle prestazioni (a meno che i dati del database non vengano memorizzati nella cache). Tuttavia, come si vedrà in un'esercitazione futura, è possibile ordinare i dati in modo efficiente quando si usa il paging personalizzato.

Quando si associa ObjectDataSource a GridView tramite l'elenco a discesa nello smart tag di GridView, ogni campo GridView dispone automaticamente della proprietà `SortExpression` assegnata al nome del campo dati nella classe `ProductsRow`. Ad esempio, il `SortExpression` di `ProductName` BoundField è impostato su `ProductName`, come illustrato nel markup dichiarativo seguente:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Un campo può essere configurato in modo che non sia ordinabile cancellando la relativa proprietà `SortExpression` (assegnando tale campo a una stringa vuota). Per illustrare questo problema, si supponga di non voler consentire ai clienti di ordinare i prodotti in base al prezzo. È possibile rimuovere la proprietà `UnitPrice` BoundField `SortExpression` dal markup dichiarativo o dalla finestra di dialogo campi (accessibile facendo clic sul collegamento Modifica colonne nello smart tag GridView s).

![I risultati sono stati ordinati in base a PrezzoUnitario in ordine crescente](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: i risultati sono stati ordinati in base a PrezzoUnitario in ordine crescente

Una volta rimossa la proprietà `SortExpression` per il `UnitPrice` BoundField, l'intestazione viene sottoposta a rendering come testo anziché come collegamento, impedendo così agli utenti di ordinare i dati in base al prezzo.

[![rimuovendo la proprietà SortExpression, gli utenti non possono più ordinare i prodotti in base al prezzo](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: rimuovendo la proprietà SortExpression, gli utenti non possono più ordinare i prodotti in base al prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image30.png))

## <a name="programmatically-sorting-the-gridview"></a>Ordinamento a livello di codice di GridView

È anche possibile ordinare il contenuto di GridView a livello di codice usando il Metodo GridView s [`Sort`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). È sufficiente passare il valore `SortExpression` per eseguire l'ordinamento in base al [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`) e i dati di GridView verranno riordinati.

Si supponga che il motivo per cui è stata disattivata l'ordinamento da parte del `UnitPrice` fosse dovuto al fatto che i clienti avrebbero semplicemente acquistato solo i prodotti a prezzo più basso. Tuttavia, vogliamo incoraggiarli ad acquistare i prodotti più costosi, quindi ci auguriamo che siano in grado di ordinare i prodotti in base al prezzo, ma solo dal prezzo più costoso a quello meno recente.

A tale scopo, aggiungere un controllo Web Button alla pagina, impostare la relativa proprietà `ID` su `SortPriceDescending`e la relativa proprietà `Text` per eseguire l'ordinamento in base al prezzo. Successivamente, creare un gestore eventi per l'evento `Click` Button facendo doppio clic sul controllo Button nella finestra di progettazione. Aggiungere il codice seguente al gestore eventi:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Se si fa clic su questo pulsante, l'utente viene reimpostato sulla prima pagina con i prodotti ordinati per prezzo, dal più costoso al meno costoso (vedere la figura 15).

[![facendo clic sul pulsante vengono ordinati i prodotti dal più costoso al meno](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: facendo clic sul pulsante si ordinano i prodotti dal più costoso al meno ([fare clic per visualizzare l'immagine con dimensioni complete](paging-and-sorting-report-data-cs/_static/image33.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come implementare le funzionalità predefinite di paging e ordinamento, entrambe semplici come la selezione di una casella di controllo. Quando un utente ordina o passa pagine attraverso dati, un flusso di lavoro simile viene ripiegato:

1. Segue un postback
2. Viene generato l'evento di pre-livello del controllo Web dati (`PageIndexChanging` o `Sorting`)
3. Tutti i dati vengono recuperati nuovamente da ObjectDataSource
4. L'evento di post-livello del controllo Web dati viene generato (`PageIndexChanged` o `Sorted`)

Quando si implementa il paging e l'ordinamento di base è una brezza, è necessario esercitare più sforzi per usare il paging personalizzato più efficiente o per migliorare ulteriormente l'interfaccia di paging o di ordinamento. Le esercitazioni future esamineranno questi argomenti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](efficiently-paging-through-large-amounts-of-data-cs.md)
