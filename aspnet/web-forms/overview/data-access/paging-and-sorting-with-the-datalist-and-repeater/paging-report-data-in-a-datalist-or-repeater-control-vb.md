---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paging dei dati del report in un controllo DataList o Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Sebbene né DataList né Repeater offrano supporto per il paging o l'ordinamento automatico, in questa esercitazione viene illustrato come aggiungere il supporto per il paging a DataList o Repeater,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c65ca1f263e41748d99323dbdf1c28fdd077246
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78620442"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Suddivisione in pagine dei dati dei report in un controllo DataList o Repeater (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) o [scaricare il file PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Sebbene né DataList né Repeater offrano il supporto automatico per il paging o l'ordinamento, in questa esercitazione viene illustrato come aggiungere il supporto per il paging a DataList o Repeater, che consente interfacce di visualizzazione dei dati e di paging molto più flessibili.

## <a name="introduction"></a>Introduzione

Il paging e l'ordinamento sono due funzionalità molto comuni quando si visualizzano i dati in un'applicazione online. Ad esempio, quando si cercano libri ASP.NET in una libreria online, è possibile che siano presenti centinaia di libri di questo tipo, ma il report che elenca i risultati della ricerca elenca solo dieci corrispondenze per pagina. Inoltre, i risultati possono essere ordinati in base a titolo, prezzo, conteggio delle pagine, nome dell'autore e così via. Come illustrato nell'esercitazione [paging e ordinamento dei dati dei report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , i controlli GridView, DetailsView e FormView forniscono il supporto per il paging incorporato che può essere abilitato al segno di spunta di una casella di controllo. GridView include anche il supporto per l'ordinamento.

Sfortunatamente, né DataList né Repeater offrono supporto automatico di paging o di ordinamento. In questa esercitazione verrà esaminato come aggiungere il supporto per il paging a DataList o Repeater. È necessario creare manualmente l'interfaccia di paging, visualizzare la pagina di record appropriata e ricordare la pagina visitata nei postback. Sebbene questa operazione imprenda più tempo e codice rispetto a GridView, DetailsView o FormView, DataList e Repeater consentono interfacce di visualizzazione dei dati e di paging molto più flessibili.

> [!NOTE]
> Questa esercitazione è dedicata esclusivamente al paging. Nell'esercitazione successiva verrà rivolto l'aggiunta di funzionalità di ordinamento.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Passaggio 1: aggiunta delle pagine Web dell'esercitazione per il paging e l'ordinamento

Prima di iniziare questa esercitazione, è necessario prima di tutto aggiungere le pagine ASP.NET necessarie per questa esercitazione e quella successiva. Per iniziare, creare una nuova cartella nel progetto denominata `PagingSortingDataListRepeater`. Aggiungere quindi le cinque pagine ASP.NET seguenti a questa cartella, in modo che tutte siano configurate per l'uso della pagina master `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Creare una cartella PagingSortingDataListRepeater e aggiungere le pagine dell'esercitazione ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: creare una cartella di `PagingSortingDataListRepeater` e aggiungere le pagine dell'esercitazione ASP.NET

Aprire quindi la pagina `Default.aspx` e trascinare il controllo utente `SectionLevelTutorialListing.ascx` dalla cartella `UserControls` nell'area di progettazione. Questo controllo utente, creato nell'esercitazione [pagine master e navigazione nel sito](../introduction/master-pages-and-site-navigation-vb.md) , enumera la mappa del sito e visualizza le esercitazioni nella sezione corrente di un elenco puntato.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))

Per fare in modo che l'elenco puntato visualizzi le esercitazioni di paging e ordinamento da creare, è necessario aggiungerle alla mappa del sito. Aprire il file di `Web.sitemap` e aggiungere il markup seguente dopo la modifica e l'eliminazione con il markup del nodo della mappa del sito di DataList:

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]

![Aggiornare la mappa del sito per includere le nuove pagine ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito per includere le nuove pagine ASP.NET

## <a name="a-review-of-paging"></a>Revisione del paging

Nelle esercitazioni precedenti è stato illustrato come eseguire il paging dei dati nei controlli GridView, DetailsView e FormView. Questi tre controlli offrono un semplice tipo di paging denominato *paging predefinito* che può essere implementato semplicemente selezionando l'opzione Abilita paging nello smart tag del controllo. Con il paging predefinito, ogni volta che viene richiesta una pagina di dati nella prima pagina visita o quando l'utente passa a un'altra pagina di dati, il controllo GridView, DetailsView o FormView richiede nuovamente *tutti* i dati da ObjectDataSource. Viene quindi eliminato il set specifico di record da visualizzare in base all'indice di pagina richiesto e al numero di record da visualizzare per ogni pagina. Il paging predefinito è stato descritto in dettaglio nell'esercitazione [paging e ordinamento dei dati del report](../paging-and-sorting/paging-and-sorting-report-data-vb.md) .

Poiché il paging predefinito richiede nuovamente tutti i record per ogni pagina, non è pratico quando si esegue il paging di quantità di dati sufficientemente grandi. Si supponga, ad esempio, di eseguire il paging di record 50.000 con una dimensione di pagina pari a 10. Ogni volta che l'utente passa a una nuova pagina, è necessario recuperare tutti i record 50.000 dal database, anche se ne vengono visualizzati solo dieci.

Il *paging personalizzato* consente di risolvere i problemi relativi alle prestazioni del paging predefinito, afferrando solo il subset preciso di record da visualizzare nella pagina richiesta. Quando si implementa il paging personalizzato, è necessario scrivere la query SQL per restituire in modo efficiente solo il set corretto di record. È stato illustrato come creare una query di questo tipo usando SQL Server nuova [parola chiave`ROW_NUMBER()`](http://www.4guysfromrolla.com/webtech/010406-1.shtml) di 2005 nel [paging in modo efficiente tramite un'esercitazione di grandi quantità di dati](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) .

Per implementare il paging predefinito nei controlli DataList o Repeater, è possibile utilizzare la [classe`PagedDataSource`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) come wrapper per la `ProductsDataTable` di cui viene eseguito il paging del contenuto. La classe `PagedDataSource` dispone di una proprietà `DataSource` che può essere assegnata a qualsiasi oggetto enumerabile, [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) proprietà che indicano il numero di record da visualizzare per pagina e l'indice della pagina corrente. Una volta impostate queste proprietà, il `PagedDataSource` può essere utilizzato come origine dati di qualsiasi controllo Web di dati. Il `PagedDataSource`, se enumerato, restituirà solo il subset appropriato di record della `DataSource` interna in base alle proprietà `PageSize` e `CurrentPageIndex`. Nella figura 4 viene illustrata la funzionalità della classe `PagedDataSource`.

![PagedDataSource esegue il wrapping di un oggetto enumerabile con un'interfaccia paginabile](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: il `PagedDataSource` esegue il wrapping di un oggetto enumerabile con un'interfaccia paginabile

L'oggetto `PagedDataSource` può essere creato e configurato direttamente dal livello della logica di business e associato a DataList o Repeater tramite ObjectDataSource oppure può essere creato e configurato direttamente nella classe code-behind della pagina ASP.NET. Se viene usato il secondo approccio, è necessario rinunciare all'uso di ObjectDataSource e associare invece i dati di paging a DataList o Repeater a livello di codice.

L'oggetto `PagedDataSource` dispone anche di proprietà per supportare il paging personalizzato. Tuttavia, è possibile ignorare l'uso di un `PagedDataSource` per il paging personalizzato perché i metodi BLL sono già presenti nella classe `ProductsBLL` progettata per il paging personalizzato che restituisce i record esatti da visualizzare.

Questa esercitazione illustra come implementare il paging predefinito in un DataList aggiungendo un nuovo metodo alla classe `ProductsBLL` che restituisce un oggetto `PagedDataSource` configurato in modo appropriato. Nell'esercitazione successiva si vedrà come usare il paging personalizzato.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Passaggio 2: aggiunta di un metodo di paging predefinito nel livello della logica di business

La classe `ProductsBLL` dispone attualmente di un metodo per la restituzione di tutte le informazioni sul prodotto `GetProducts()` e una per la restituzione di un particolare subset di prodotti a un indice iniziale `GetProductsPaged(startRowIndex, maximumRows)`. Con il paging predefinito, i controlli GridView, DetailsView e FormView utilizzano tutti il metodo `GetProducts()` per recuperare tutti i prodotti, ma utilizzano una `PagedDataSource` internamente per visualizzare solo il subset corretto di record. Per replicare questa funzionalità con i controlli DataList e Repeater, è possibile creare un nuovo metodo nell'oggetto BLL che simula questo comportamento.

Aggiungere un metodo alla classe `ProductsBLL` denominata `GetProductsAsPagedDataSource` che accetta due parametri di input di tipo Integer:

- `pageIndex` l'indice della pagina da visualizzare, indicizzato a zero e
- `pageSize` il numero di record da visualizzare per ogni pagina.

`GetProductsAsPagedDataSource` inizia a recuperare *tutti* i record da `GetProducts()`. Viene quindi creato un oggetto `PagedDataSource`, impostando le proprietà `CurrentPageIndex` e `PageSize` sui valori dei parametri `pageIndex` e `pageSize` passati. Il metodo termina restituendo questo `PagedDataSource`configurato:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Passaggio 3: visualizzazione delle informazioni sul prodotto in un oggetto DataList utilizzando il paging predefinito

Con il metodo `GetProductsAsPagedDataSource` aggiunto alla classe `ProductsBLL`, ora è possibile creare un oggetto DataList o Repeater che fornisce il paging predefinito. Per iniziare, aprire la pagina `Paging.aspx` nella cartella `PagingSortingDataListRepeater` e trascinare un oggetto DataList dalla casella degli strumenti nella finestra di progettazione, impostando la proprietà `ID` di DataList su `ProductsDefaultPaging`. Dallo smart tag di DataList, creare un nuovo ObjectDataSource denominato `ProductsDefaultPagingDataSource` e configurarlo in modo che recuperi i dati usando il metodo `GetProductsAsPagedDataSource`.

[![creare un ObjectDataSource e configurarlo per l'uso del metodo GetProductsAsPagedDataSource ()](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: creare un ObjectDataSource e configurarlo per l'uso del metodo di `()` `GetProductsAsPagedDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

Impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno).

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

Poiché il metodo `GetProductsAsPagedDataSource` prevede due parametri di input, la procedura guidata richiede l'origine di questi valori di parametro.

I valori relativi all'indice e alle dimensioni di pagina devono essere memorizzati nei postback. Possono essere archiviati in stato di visualizzazione, salvati in modo permanente in QueryString, archiviati in variabili di sessione o ricordati usando un'altra tecnica. Per questa esercitazione verrà usata la QueryString, che offre il vantaggio di consentire a una particolare pagina di dati di essere inserita in un segnalibro.

In particolare, usare i campi QueryString e pageSize per i parametri `pageIndex` e `pageSize`, rispettivamente (vedere la figura 7). Impostare i valori predefiniti per questi parametri in modo che i valori QueryString non siano presenti quando un utente visita la pagina per la prima volta. Per `pageIndex`, impostare il valore predefinito su 0 (che visualizzerà la prima pagina di dati) e `pageSize` il valore predefinito di 4.

[![utilizzare QueryString come origine per i parametri pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: usare QueryString come origine per il `pageIndex` e `pageSize` parametri ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))

Dopo aver configurato ObjectDataSource, Visual Studio crea automaticamente un `ItemTemplate` per DataList. Personalizzare il `ItemTemplate` in modo che vengano visualizzati solo il nome, la categoria e il fornitore del prodotto. Impostare anche la proprietà `RepeatColumns` di DataList su 2, il relativo `Width` su 100% e il `ItemStyle` s `Width` 50%. Queste impostazioni di larghezza forniranno la spaziatura uguale per le due colonne.

Dopo avere apportato queste modifiche, il markup di DataList e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Poiché in questa esercitazione non viene eseguita alcuna funzionalità di aggiornamento o eliminazione, è possibile disabilitare lo stato di visualizzazione di DataList per ridurre le dimensioni della pagina di cui è stato eseguito il rendering.

Quando inizialmente si visita questa pagina tramite un browser, non viene fornito né il `pageIndex` né `pageSize` parametri QueryString. Vengono pertanto utilizzati i valori predefiniti 0 e 4. Come illustrato nella figura 8, si ottiene un DataList che Visualizza i primi quattro prodotti.

[![vengono elencati i primi quattro prodotti](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: i primi quattro prodotti sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))

Senza un'interfaccia di paging, attualmente non esiste alcun modo semplice per un utente di passare alla seconda pagina di dati. Nel passaggio 4 verrà creata un'interfaccia di paging. Per il momento, tuttavia, il paging può essere eseguito solo specificando direttamente i criteri di paging nella QueryString. Per visualizzare la seconda pagina, ad esempio, modificare l'URL nella barra degli indirizzi del browser da `Paging.aspx` a `Paging.aspx?pageIndex=2` e premere INVIO. In questo modo viene visualizzata la seconda pagina di dati (vedere la figura 9).

[![viene visualizzata la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: viene visualizzata la seconda pagina di dati ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Passaggio 4: creazione dell'interfaccia di paging

Sono disponibili numerose interfacce di paging diverse che possono essere implementate. I controlli GridView, DetailsView e FormView forniscono quattro diverse interfacce tra cui scegliere:

- **Successivamente,** gli utenti precedenti possono spostare una pagina alla volta, fino a quella precedente o successiva.
- **Avanti, precedente, primo, ultimo** , oltre ai pulsanti avanti e indietro, questa interfaccia include il primo e l'ultimo pulsante per lo passaggio alla prima o all'ultima pagina.
- **Numeric** elenca i numeri di pagina nell'interfaccia di paging, consentendo a un utente di passare rapidamente a una pagina specifica.
- **Numeric, First, Last** , oltre ai numeri di pagina numerici, include i pulsanti per lo passaggio alla prima o all'ultima pagina.

Per DataList e Repeater, l'utente è responsabile della decisione su un'interfaccia di paging e di implementazione. Questa operazione comporta la creazione dei controlli Web necessari nella pagina e la visualizzazione della pagina richiesta quando si fa clic su un pulsante di interfaccia di paging particolare. Inoltre, è possibile che alcuni controlli dell'interfaccia di paging debbano essere disabilitati. Ad esempio, quando si visualizza la prima pagina di dati utilizzando l'interfaccia successiva, precedente, prima, ultima, i pulsanti primo e indietro verrebbero disabilitati.

Per questa esercitazione, è necessario usare l'interfaccia successiva, precedente, prima, ultima. Aggiungere i controlli Web di quattro pulsanti alla pagina e impostare i `ID` s su `FirstPage`, `PrevPage`, `NextPage`e `LastPage`. Impostare le proprietà del `Text` su &lt;&lt; prima, &lt; Prev, Next &gt;e Last &gt;&gt;.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Successivamente, creare un gestore dell'evento `Click` per ognuno di questi pulsanti. Nel momento in cui verrà aggiunto il codice necessario per visualizzare la pagina richiesta.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Ricordando il numero totale di record di cui è stato eseguito il paging

Indipendentemente dall'interfaccia di paging selezionata, è necessario calcolare e ricordare il numero totale di record di cui viene eseguito il paging. Il numero totale di righe, in combinazione con le dimensioni della pagina, determina il numero totale di pagine di dati di cui viene eseguito il paging, che determina i controlli dell'interfaccia di paging aggiunti o abilitati. Nella prossima, prima, ultima interfaccia che viene compilata, il conteggio delle pagine viene usato in due modi:

- Per determinare se viene visualizzata l'ultima pagina, nel qual caso i pulsanti avanti e ultimo sono disabilitati.
- Se l'utente fa clic sull'ultimo pulsante, è necessario inserirlo nell'ultima pagina, il cui indice è inferiore al numero di pagine.

Il conteggio delle pagine viene calcolato come il limite del numero totale di righe diviso per le dimensioni della pagina. Se, ad esempio, si esegue il paging di 79 record con quattro record per pagina, il numero di pagine è 20 (il limite di 79/4). Se si usa l'interfaccia di paging numerico, queste informazioni segnalano il numero di pulsanti di pagina numerici da visualizzare; Se l'interfaccia di paging include pulsanti avanti o ultimo, il conteggio delle pagine viene usato per determinare quando disabilitare i pulsanti avanti o ultimo.

Se l'interfaccia di paging include un ultimo pulsante, è fondamentale che il numero totale di record di cui viene eseguito il paging venga memorizzato nei postback in modo che, quando si fa clic sull'ultimo pulsante, sia possibile determinare l'ultimo indice di pagina. Per semplificare questa operazione, creare una proprietà `TotalRowCount` nella classe code-behind della pagina ASP.NET che rende permanente il valore per visualizzare lo stato:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Oltre a `TotalRowCount`, è possibile creare proprietà a livello di pagina di sola lettura per accedere facilmente all'indice della pagina, alle dimensioni della pagina e al numero di pagine:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinazione del numero totale di record di cui è stato eseguito il paging

Il `PagedDataSource` oggetto restituito dal metodo di `Select()` di ObjectDataSource contiene *tutti* i record del prodotto, anche se in DataList viene visualizzato solo un subset di tali record. La [proprietà`Count`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) `PagedDataSource` s restituisce solo il numero di elementi che verranno visualizzati nel DataList; la [proprietà`DataSourceCount`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) restituisce il numero totale di elementi all'interno del `PagedDataSource`. Pertanto, è necessario assegnare la proprietà della pagina ASP.NET `TotalRowCount` il valore della proprietà `DataSourceCount` `PagedDataSource`.

A tale scopo, creare un gestore eventi per l'evento `Selected` di ObjectDataSource. Nel gestore dell'evento `Selected` è possibile accedere al valore restituito del metodo di `Select()` ObjectDataSource, in questo caso, il `PagedDataSource`.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Visualizzazione della pagina di dati richiesta

Quando l'utente fa clic su uno dei pulsanti nell'interfaccia di paging, è necessario visualizzare la pagina di dati richiesta. Poiché i parametri di paging vengono specificati tramite QueryString, per visualizzare la pagina richiesta di dati usare `Response.Redirect(url)` per fare in modo che il browser utente richieda la pagina `Paging.aspx` con i parametri di paging appropriati. Ad esempio, per visualizzare la seconda pagina di dati, l'utente verrà reindirizzato a `Paging.aspx?pageIndex=1`.

Per semplificare questa operazione, creare un metodo di `RedirectUser(sendUserToPageIndex)` che reindirizza l'utente a `Paging.aspx?pageIndex=sendUserToPageIndex`. Chiamare quindi questo metodo dai quattro pulsanti `Click` gestori di eventi. Nel gestore dell'evento `FirstPage` `Click` chiamare `RedirectUser(0)`per inviarli alla prima pagina; nel gestore dell'evento `PrevPage` `Click`, utilizzare `PageIndex - 1` come indice della pagina; E così via.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Con i gestori di eventi di `Click` completati, è possibile eseguire il paging dei record di DataList facendo clic sui pulsanti. Provalo!

## <a name="disabling-paging-interface-controls"></a>Disabilitazione di controlli dell'interfaccia di paging

Attualmente, tutti e quattro i pulsanti sono abilitati indipendentemente dalla pagina visualizzata. Tuttavia, si desidera disabilitare i pulsanti primo e indietro quando si visualizza la prima pagina di dati e i pulsanti avanti e ultimo quando si visualizza l'ultima pagina. Il `PagedDataSource` oggetto restituito dal Metodo ObjectDataSource s `Select()` dispone di proprietà [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) che è possibile esaminare per determinare se si sta visualizzando la prima o l'ultima pagina di dati.

Aggiungere quanto segue al gestore dell'evento ObjectDataSource s `Selected`:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Con questa aggiunta, i pulsanti primo e indietro verranno disabilitati quando si visualizza la prima pagina, mentre i pulsanti avanti e ultimo verranno disabilitati durante la visualizzazione dell'ultima pagina.

Consente di completare l'interfaccia di paging indicando all'utente la pagina che attualmente Visualizza e il numero totale di pagine esistenti. Aggiungere un controllo Web Label alla pagina e impostare la relativa proprietà `ID` su `CurrentPageNumber`. Impostare la relativa proprietà `Text` nel gestore eventi selezionato di ObjectDataSource in modo che includa la pagina corrente visualizzata (`PageIndex + 1`) e il numero totale di pagine (`PageCount`).

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

La figura 10 Mostra `Paging.aspx` alla prima visita. Poiché la QueryString è vuota, il valore predefinito di DataList Mostra i primi quattro prodotti; i pulsanti primo e indietro sono disabilitati. Fare clic su Avanti per visualizzare i quattro record successivi (vedere la figura 11); i pulsanti primo e indietro sono ora abilitati.

[![viene visualizzata la prima pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: viene visualizzata la prima pagina di dati ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))

[![viene visualizzata la seconda pagina di dati](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: viene visualizzata la seconda pagina di dati ([fare clic per visualizzare l'immagine con dimensioni complete](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))

> [!NOTE]
> L'interfaccia di paging può essere ulteriormente migliorata consentendo all'utente di specificare il numero di pagine da visualizzare per ogni pagina. È ad esempio possibile aggiungere un oggetto DropDownList che elenca le opzioni relative alle dimensioni della pagina quali 5, 10, 25, 50 e tutte. Quando si selezionano le dimensioni della pagina, l'utente deve essere reindirizzato di nuovo a `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Lascio implementare questo miglioramento come esercizio per il lettore.

## <a name="using-custom-paging"></a>Uso del paging personalizzato

Le pagine di DataList passano i dati utilizzando la tecnica di paging predefinita inefficiente. Quando si esegue il paging in quantità sufficientemente elevate di dati, è fondamentale usare il paging personalizzato. Sebbene i dettagli di implementazione differiscano leggermente, i concetti alla base dell'implementazione del paging personalizzato in un DataList corrispondono a quelli del paging predefinito. Con il paging personalizzato, usare il metodo `ProductBLL` Class s `GetProductsPaged` (anziché `GetProductsAsPagedDataSource`). Come illustrato nell'esercitazione relativa al [paging efficiente in grandi quantità di dati](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) , `GetProductsPaged` necessario passare l'indice della riga iniziale e il numero massimo di righe da restituire. Questi parametri possono essere gestiti tramite la QueryString esattamente come i parametri `pageIndex` e `pageSize` usati nel paging predefinito.

Poiché non esistono `PagedDataSource` con il paging personalizzato, è necessario utilizzare tecniche alternative per determinare il numero totale di record di cui è stato eseguito il paging e la visualizzazione della prima o dell'ultima pagina di dati. Il metodo `TotalNumberOfProducts()` nella classe `ProductsBLL` restituisce il numero totale di prodotti di cui è stato eseguito il paging. Per determinare se la prima pagina di dati viene visualizzata, esaminare l'indice della riga iniziale se è pari a zero, quindi viene visualizzata la prima pagina. L'ultima pagina viene visualizzata se l'indice della riga iniziale e il numero massimo di righe da restituire sono maggiori o uguali al numero totale di record di cui è stato eseguito il paging.

L'implementazione del paging personalizzato verrà illustrata più dettagliatamente nell'esercitazione successiva.

## <a name="summary"></a>Riepilogo

Sebbene né DataList né Repeater offrono il supporto predefinito per il paging disponibile nei controlli GridView, DetailsView e FormView, tale funzionalità può essere aggiunta con il minimo sforzo. Il modo più semplice per implementare il paging predefinito consiste nell'eseguire il wrapping dell'intero set di prodotti all'interno di una `PagedDataSource`, quindi associare il `PagedDataSource` a DataList o Repeater. In questa esercitazione è stato aggiunto il metodo `GetProductsAsPagedDataSource` alla classe `ProductsBLL` per restituire il `PagedDataSource`. La classe `ProductsBLL` contiene già i metodi necessari per il paging personalizzato `GetProductsPaged` e `TotalNumberOfProducts`.

Oltre a recuperare il set preciso di record da visualizzare per il paging personalizzato o per tutti i record in una `PagedDataSource` per il paging predefinito, è necessario aggiungere manualmente l'interfaccia di paging. Per questa esercitazione è stata creata un'interfaccia successiva, precedente, prima e ultima con quattro controlli Web Button. Inoltre, è stato aggiunto un controllo Label che Visualizza il numero di pagina corrente e il numero totale di pagine.

Nell'esercitazione successiva verrà illustrato come aggiungere il supporto di ordinamento a DataList e Repeater. Verrà inoltre illustrato come creare un oggetto DataList che può essere sottoposto a paging e ordinato (con esempi che usano il paging predefinito e personalizzato).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Liz Shulok, Ken Pespisa e Bernadette Leigh. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Successivo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
