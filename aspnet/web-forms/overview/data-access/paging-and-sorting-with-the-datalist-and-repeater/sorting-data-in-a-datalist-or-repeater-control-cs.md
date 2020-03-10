---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Ordinamento dei dati in un controllo DataList o Repeater (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come includere il supporto di ordinamento in DataList e Repeater, nonché come creare un DataList o un ripetitore i cui dati possono...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 66a09c637d33c812b39e0ce85a552bd71665a2e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78620281"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Ordinamento dei dati in un controllo DataList o Repeater (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) o [scaricare il file PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In questa esercitazione verrà illustrato come includere il supporto di ordinamento in DataList e Repeater, nonché come creare un oggetto DataList o Repeater i cui dati possono essere sottoposto a paging e ordinati.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) è stato esaminato come aggiungere il supporto per il paging a un DataList. È stato creato un nuovo metodo nella classe `ProductsBLL` (`GetProductsAsPagedDataSource`) che ha restituito un oggetto `PagedDataSource`. Quando viene associato a un oggetto DataList o Repeater, DataList o Repeater Visualizza solo la pagina di dati richiesta. Questa tecnica è simile a quella utilizzata internamente dai controlli GridView, DetailsView e FormView per fornire la funzionalità predefinita di paging predefinita.

Oltre a offrire supporto per il paging, GridView include anche il supporto per l'ordinamento predefinito. Né DataList né Repeater forniscono funzionalità di ordinamento predefinite; Tuttavia, le funzionalità di ordinamento possono essere aggiunte con un po' di codice. In questa esercitazione verrà illustrato come includere il supporto di ordinamento in DataList e Repeater, nonché come creare un oggetto DataList o Repeater i cui dati possono essere sottoposto a paging e ordinati.

## <a name="a-review-of-sorting"></a>Verifica dell'ordinamento

Come si è visto nell'esercitazione [paging e ordinamento dei dati del report](../paging-and-sorting/paging-and-sorting-report-data-cs.md) , il controllo GridView fornisce il supporto per l'ordinamento predefinito. A ogni campo GridView può essere associato un `SortExpression`, che indica il campo dati in base al quale ordinare i dati. Quando la proprietà `AllowSorting` di GridView è impostata su `true`, il rendering dell'intestazione di ogni campo GridView con un valore della proprietà `SortExpression` viene eseguito come LinkButton. Quando un utente fa clic su un'intestazione del Campo GridView specifica, viene eseguito un postback e i dati vengono ordinati in base al campo `SortExpression`selezionato.

Il controllo GridView dispone anche di una proprietà `SortExpression`, che archivia la `SortExpression` del Campo GridView in base alla quale vengono ordinati i dati. Inoltre, una proprietà `SortDirection` indica se i dati devono essere ordinati in ordine crescente o decrescente (se un utente fa clic due volte sul collegamento di un'intestazione del Campo GridView) in successione, il tipo di ordinamento viene attivato o disattivato.

Quando GridView è associato al controllo origine dati, passa le relative `SortExpression` e `SortDirection` proprietà al controllo origine dati. Il controllo origine dati recupera i dati e li ordina in base alle proprietà `SortExpression` e `SortDirection` fornite. Dopo aver ordinato i dati, il controllo origine dati lo restituisce a GridView.

Per replicare questa funzionalità con i controlli DataList o Repeater, è necessario:

- Creare un'interfaccia di ordinamento
- Ricordare il campo dati in base a cui eseguire l'ordinamento e se eseguire l'ordinamento in ordine crescente o decrescente
- Indicare a ObjectDataSource di ordinare i dati in base a un campo dati specifico

Queste tre attività verranno affrontate nei passaggi 3 e 4. In seguito verrà illustrato come includere il supporto per il paging e l'ordinamento in un oggetto DataList o Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Passaggio 2: visualizzazione dei prodotti in un Repeater

Prima di preoccuparsi di implementare una delle funzionalità relative all'ordinamento, è necessario innanzitutto elencare i prodotti in un controllo Repeater. Per iniziare, aprire la pagina `Sorting.aspx` nella cartella `PagingSortingDataListRepeater`. Aggiungere un controllo Repeater alla pagina Web, impostando la relativa proprietà `ID` su `SortableProducts`. Dallo smart tag del Repeater, creare un nuovo ObjectDataSource denominato `ProductsDataSource` e configurarlo per il recupero dei dati dal metodo `GetProducts()` della classe `ProductsBLL`. Selezionare l'opzione (nessuno) dagli elenchi a discesa nelle schede Inserisci, aggiorna ed Elimina.

[![creare un ObjectDataSource e configurarlo per l'uso del metodo GetProductsAsPagedDataSource ()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: creare un ObjectDataSource e configurarlo per l'uso del metodo `GetProductsAsPagedDataSource()` ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figura 2**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))

A differenza di DataList, Visual Studio non crea automaticamente un `ItemTemplate` per il controllo Repeater dopo l'associazione a un'origine dati. Inoltre, è necessario aggiungere questo `ItemTemplate` in modo dichiarativo, perché lo smart tag del controllo Repeater non dispone dell'opzione modifica modelli disponibile in DataList. Consente di utilizzare la stessa `ItemTemplate` dell'esercitazione precedente, che Visualizza il nome, il fornitore e la categoria del prodotto.

Dopo aver aggiunto il `ItemTemplate`, il markup dichiarativo Repeater e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Nella figura 3 Questa pagina viene visualizzata in un browser.

[![viene visualizzato il nome, il fornitore e la categoria di ogni prodotto](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 3**: viene visualizzato il nome, il fornitore e la categoria di ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Passaggio 3: istruzioni su ObjectDataSource per ordinare i dati

Per ordinare i dati visualizzati nel Repeater, è necessario informare ObjectDataSource dell'espressione di ordinamento in base alla quale i dati devono essere ordinati. Prima che ObjectDataSource recuperi i dati, genera prima di tutto il relativo [evento`Selecting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), che offre la possibilità di specificare un'espressione di ordinamento. Al gestore dell'evento `Selecting` viene passato un oggetto di tipo [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), che dispone di una proprietà denominata [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) di tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). La classe `DataSourceSelectArguments` è progettata per passare richieste correlate ai dati da un consumer di dati al controllo origine dati e include una [proprietà`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Per passare le informazioni di ordinamento dalla pagina ASP.NET a ObjectDataSource, creare un gestore eventi per l'evento `Selecting` e usare il codice seguente:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Al valore *SortExpression* deve essere assegnato il nome del campo dati in base al quale ordinare i dati, ad esempio ProductName. Non esiste alcuna proprietà relativa alla direzione di ordinamento, pertanto se si desidera ordinare i dati in ordine decrescente, aggiungere la stringa DESC al valore *SortExpression* (ad esempio ProductName DESC).

Procedere e provare alcuni valori hardcoded diversi per *SortExpression* e testare i risultati in un browser. Come illustrato nella figura 4, quando si usa ProductName DESC come *SortExpression*, i prodotti vengono ordinati in base al nome in ordine alfabetico inverso.

[![i prodotti sono ordinati in base al nome in ordine alfabetico inverso](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 4**: i prodotti sono ordinati in base al nome in ordine alfabetico inverso ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Passaggio 4: creare l'interfaccia di ordinamento e ricordare l'espressione e la direzione di ordinamento

L'attivazione del supporto di ordinamento in GridView converte ogni testo dell'intestazione del campo ordinabile in un LinkButton che, quando selezionato, Ordina i dati di conseguenza. Un'interfaccia di ordinamento di questo tipo è utile per GridView, in cui i dati vengono disposti in colonne in modo accurato. Per i controlli DataList e Repeater, tuttavia, è necessaria un'interfaccia di ordinamento diversa. Un'interfaccia di ordinamento comune per un elenco di dati (in contrapposizione a una griglia di dati) è un elenco a discesa che fornisce i campi in base ai quali è possibile ordinare i dati. Consente di implementare tale interfaccia per questa esercitazione.

Aggiungere un controllo Web DropDownList sopra il `SortableProducts` Repeater e impostare la relativa proprietà `ID` su `SortBy`. Dal Finestra Proprietà fare clic sui puntini di sospensione nella proprietà `Items` per visualizzare l'editor della raccolta ListItem. Aggiungere `ListItem` s per ordinare i dati in base ai campi `ProductName`, `CategoryName`e `SupplierName`. Aggiungere inoltre un `ListItem` per ordinare i prodotti in base al nome in ordine alfabetico inverso.

Le proprietà `ListItem` `Text` possono essere impostate su qualsiasi valore, ad esempio nome, ma le proprietà `Value` devono essere impostate sul nome del campo dati, ad esempio ProductName. Per ordinare i risultati in ordine decrescente, aggiungere la stringa DESC al nome del campo dati, ad esempio ProductName DESC.

![Aggiungere un oggetto ListItem per ogni campo dati ordinabile](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 5**: aggiungere un `ListItem` per ogni campo dati ordinabile

Infine, aggiungere un controllo Web Button a destra di DropDownList. Impostare la relativa `ID` su `RefreshRepeater` e la relativa proprietà `Text` da aggiornare.

Dopo aver creato il `ListItem` e aver aggiunto il pulsante Aggiorna, la sintassi dichiarativa DropDownList e Button s dovrebbe essere simile alla seguente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Al termine dell'ordinamento, è necessario aggiornare il gestore dell'evento ObjectDataSource s `Selecting` in modo che utilizzi la proprietà `Value` `SortBy``ListItem` s selezionata anziché un'espressione di ordinamento hardcoded.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

A questo punto, quando si visita la pagina per la prima volta, i prodotti verranno ordinati inizialmente in base al campo dati `ProductName`, perché il `SortBy` `ListItem` selezionato per impostazione predefinita (vedere la figura 6). Se si seleziona un'opzione di ordinamento diversa, ad esempio Category e si fa clic su Refresh, verrà generato un postback e i dati verranno ordinati di nuovo in base al nome della categoria, come illustrato nella figura 7.

[![i prodotti vengono inizialmente ordinati in base al nome](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figura 6**: i prodotti vengono inizialmente ordinati in base al nome ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))

[![i prodotti sono ora ordinati per categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figura 7**: i prodotti sono ora ordinati per categoria ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))

> [!NOTE]
> Se si fa clic sul pulsante Aggiorna, i dati verranno automaticamente riordinati perché lo stato di visualizzazione del Repeater è stato disabilitato, causando la riassociazione del ripetitore alla relativa origine dati in ogni postback. Se lo stato di visualizzazione del Repeater è abilitato, la modifica dell'elenco a discesa dell'ordinamento non avrà alcun effetto sull'ordinamento. Per risolvere questo problema, creare un gestore eventi per l'evento `Click` pulsante Aggiorna e riassociare il ripetitore alla relativa origine dati (chiamando il metodo Repeater s `DataBind()`).

## <a name="remembering-the-sort-expression-and-direction"></a>Ricordando l'espressione e la direzione di ordinamento

Quando si crea un oggetto DataList o Repeater ordinabile in una pagina in cui possono verificarsi postback non correlati all'ordinamento, è imperativo che l'espressione e la direzione di ordinamento vengano memorizzate nei postback. Si supponga, ad esempio, di aver aggiornato il ripetitore in questa esercitazione per includere un pulsante Elimina con ogni prodotto. Quando l'utente fa clic sul pulsante Elimina, viene eseguito il codice per eliminare il prodotto selezionato, quindi i dati vengono riassociati al ripetitore. Se i dettagli di ordinamento non sono salvati in modo permanente durante il postback, i dati visualizzati sullo schermo verranno ripristinati nell'ordinamento originale.

Per questa esercitazione, il DropDownList Salva in modo implicito l'espressione e la direzione di ordinamento nello stato di visualizzazione. Se si utilizzava un'interfaccia di ordinamento diversa con, ad dirsi, gli LinkButton che fornivano le varie opzioni di ordinamento necessarie per ricordare il tipo di ordinamento nei postback. Questa operazione può essere eseguita archiviando i parametri di ordinamento nello stato di visualizzazione della pagina, includendo il parametro sort in QueryString o tramite un'altra tecnica di persistenza dello stato.

Negli esempi futuri in questa esercitazione viene illustrato come salvare in modo permanente i dettagli di ordinamento nello stato di visualizzazione della pagina.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Passaggio 5: aggiunta del supporto per l'ordinamento a un DataList che usa il paging predefinito

Nell' [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) è stato esaminato come implementare il paging predefinito con un DataList. È possibile estendere questo esempio precedente per includere la possibilità di ordinare i dati di paging. Per iniziare, aprire le pagine `SortingWithDefaultPaging.aspx` e `Paging.aspx` nella cartella `PagingSortingDataListRepeater`. Nella pagina `Paging.aspx` fare clic sul pulsante origine per visualizzare il markup dichiarativo della pagina. Copiare il testo selezionato (vedere la figura 8) e incollarlo nel markup dichiarativo di `SortingWithDefaultPaging.aspx` tra i tag di `<asp:Content>`.

[![replicare il markup dichiarativo nei tag &lt;ASP: content&gt; da paging. aspx a SortingWithDefaultPaging. aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figura 8**: replicare il markup dichiarativo nei tag `<asp:Content>` da `Paging.aspx` a `SortingWithDefaultPaging.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))

Dopo aver copiato il markup dichiarativo, copiare i metodi e le proprietà nella classe code-behind della pagina `Paging.aspx` nella classe code-behind per `SortingWithDefaultPaging.aspx`. Successivamente, è necessario visualizzare la pagina `SortingWithDefaultPaging.aspx` in un browser. Dovrebbe presentare la stessa funzionalità e l'aspetto di `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Miglioramento di ProductsBLL per includere un metodo di paging e ordinamento predefinito

Nell'esercitazione precedente è stato creato un metodo di `GetProductsAsPagedDataSource(pageIndex, pageSize)` nella classe `ProductsBLL` che ha restituito un oggetto `PagedDataSource`. Questo `PagedDataSource` oggetto è stato popolato con *tutti* i prodotti (tramite il metodo BLL s `GetProducts()`), ma quando associato a DataList sono stati visualizzati solo i record corrispondenti ai parametri di input *pageIndex* e *pageSize* specificati.

In precedenza in questa esercitazione è stato aggiunto il supporto per l'ordinamento specificando l'espressione di ordinamento dal gestore dell'evento ObjectDataSource s `Selecting`. Questo funziona bene quando ObjectDataSource restituisce un oggetto che può essere ordinato, ad esempio il `ProductsDataTable` restituito dal metodo `GetProducts()`. Tuttavia, l'oggetto `PagedDataSource` restituito dal metodo `GetProductsAsPagedDataSource` non supporta l'ordinamento della relativa origine dati interna. Al contrario, è necessario ordinare i risultati restituiti dal metodo `GetProducts()` *prima* di inserirli nel `PagedDataSource`.

A tale scopo, creare un nuovo metodo nella classe `ProductsBLL` `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Per ordinare le `ProductsDataTable` restituite dal metodo `GetProducts()`, specificare la proprietà `Sort` del `DataTableView`predefinito:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Il metodo `GetProductsSortedAsPagedDataSource` differisce solo leggermente dal metodo `GetProductsAsPagedDataSource` creato nell'esercitazione precedente. In particolare, `GetProductsSortedAsPagedDataSource` accetta un parametro di input aggiuntivo `sortExpression` e assegna questo valore alla proprietà `Sort` del `DefaultView``ProductDataTable`. Alcune righe di codice in un secondo momento, l'origine dati dell'oggetto `PagedDataSource` viene assegnata al `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Chiamata del metodo GetProductsSortedAsPagedDataSource e specifica del valore per il parametro di input SortExpression

Con il metodo `GetProductsSortedAsPagedDataSource` completo, il passaggio successivo consiste nel fornire il valore per questo parametro. ObjectDataSource in `SortingWithDefaultPaging.aspx` è attualmente configurato per chiamare il metodo `GetProductsAsPagedDataSource` e passa i due parametri di input attraverso i due `QueryStringParameters`specificati nella raccolta `SelectParameters`. Queste due `QueryStringParameters` indicano che l'origine per i parametri del metodo `GetProductsAsPagedDataSource` s *pageIndex* e *pageSize* proviene dai campi QueryString `pageIndex` e `pageSize`.

Aggiornare la proprietà `SelectMethod` di ObjectDataSource in modo che richiami il nuovo metodo `GetProductsSortedAsPagedDataSource`. Aggiungere quindi un nuovo `QueryStringParameter` in modo da accedere al parametro di input *SortExpression* dal campo QueryString `sortExpression`. Impostare la `QueryStringParameter` s `DefaultValue` su ProductName.

Dopo queste modifiche, il markup dichiarativo di ObjectDataSource dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

A questo punto, nella pagina `SortingWithDefaultPaging.aspx` i risultati vengono ordinati in ordine alfabetico in base al nome del prodotto (vedere la figura 9). Questo perché, per impostazione predefinita, un valore di ProductName viene passato come il parametro *SortExpression* del metodo `GetProductsSortedAsPagedDataSource`.

[![per impostazione predefinita, i risultati vengono ordinati in base a ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figura 9**: per impostazione predefinita, i risultati vengono ordinati in base `ProductName` ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))

Se si aggiunge manualmente un `sortExpression` campo QueryString, ad esempio `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` i risultati verranno ordinati in base al `sortExpression`specificato. Tuttavia, questo parametro `sortExpression` non è incluso nella QueryString quando si passa a una pagina di dati diversa. Infatti, facendo clic sui pulsanti Next o Last page ci si torna a `Paging.aspx`. Attualmente non è presente alcuna interfaccia di ordinamento. L'unico modo in cui un utente può modificare l'ordinamento dei dati di paging consiste nel manipolare direttamente la QueryString.

## <a name="creating-the-sorting-interface"></a>Creazione dell'interfaccia di ordinamento

Prima di tutto è necessario aggiornare il metodo `RedirectUser` per inviare l'utente a `SortingWithDefaultPaging.aspx` (anziché `Paging.aspx`) e includere il valore `sortExpression` in QueryString. È anche necessario aggiungere una proprietà `SortExpression` di sola lettura a livello di pagina. Questa proprietà, simile alle proprietà `PageIndex` e `PageSize` create nell'esercitazione precedente, restituisce il valore del campo `sortExpression` QueryString, se esistente, e il valore predefinito (ProductName) in caso contrario.

Attualmente il metodo `RedirectUser` accetta solo un singolo parametro di input, ovvero l'indice della pagina da visualizzare. Tuttavia, in alcuni casi potrebbe essere necessario reindirizzare l'utente a una particolare pagina di dati usando un'espressione di ordinamento diversa da quella specificata nella QueryString. Nel momento in cui verrà creata l'interfaccia di ordinamento per questa pagina, che includerà una serie di controlli Web dei pulsanti per ordinare i dati in base a una colonna specificata. Quando si fa clic su uno di questi pulsanti, si desidera reindirizzare l'utente passando il valore dell'espressione di ordinamento appropriato. Per fornire questa funzionalità, creare due versioni del metodo `RedirectUser`. Il primo deve accettare solo l'indice della pagina da visualizzare, mentre il secondo accetta l'indice della pagina e l'espressione di ordinamento.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Nel primo esempio di questa esercitazione è stata creata un'interfaccia di ordinamento usando un oggetto DropDownList. Per questo esempio, è necessario usare i controlli Web a tre pulsanti posizionati sopra DataList uno per l'ordinamento in base `ProductName`, uno per `CategoryName`e uno per `SupplierName`. Aggiungere i controlli Web dei tre pulsanti, impostando le proprietà `ID` e `Text` in modo appropriato:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Successivamente, creare un gestore dell'evento `Click` per ogni. I gestori di eventi devono chiamare il metodo `RedirectUser`, restituendo l'utente alla prima pagina usando l'espressione di ordinamento appropriata.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Quando si visita la pagina per la prima volta, i dati vengono ordinati in base al nome del prodotto in ordine alfabetico (fare riferimento alla figura 9). Fare clic sul pulsante Avanti per passare alla seconda pagina di dati, quindi fare clic sul pulsante Ordina per categoria. Viene restituito alla prima pagina di dati, ordinata in base al nome della categoria (vedere la figura 10). Analogamente, facendo clic sul pulsante Ordina per fornitore, i dati vengono ordinati in base al fornitore iniziando dalla prima pagina di dati. La scelta dell'ordinamento viene memorizzata quando i dati vengono sottopaginati. La figura 11 Mostra la pagina dopo l'ordinamento in base alla categoria e quindi avanza alla tredicesima pagina di dati.

[![i prodotti sono ordinati in base alla categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figura 10**: i prodotti sono ordinati per categoria ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))

[![l'espressione di ordinamento viene memorizzata durante il paging dei dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figura 11**: l'espressione di ordinamento viene memorizzata durante il paging dei dati ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Passaggio 6: paging personalizzato tramite record in un Repeater

L'esempio di DataList esaminato nel passaggio 5 pagine attraverso i dati utilizzando la tecnica di paging predefinita inefficiente. Quando si esegue il paging in quantità sufficientemente elevate di dati, è fondamentale usare il paging personalizzato. Tornando al [paging efficiente con grandi quantità di dati](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) e ordinando le esercitazioni [personalizzate sui dati](../paging-and-sorting/sorting-custom-paged-data-cs.md) di paging, sono state esaminate le differenze tra il paging predefinito e quello personalizzato e i metodi creati in BLL per l'utilizzo del paging personalizzato e l'ordinamento dei dati personalizzati di paging. In particolare, in queste due esercitazioni precedenti sono stati aggiunti i tre metodi seguenti alla classe `ProductsBLL`:

- `GetProductsPaged(startRowIndex, maximumRows)` restituisce un particolare subset di record a partire da *StartRowIndex* senza superare *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` restituisce un particolare subset di record ordinati in base al parametro di input *SortExpression* specificato.
- `TotalNumberOfProducts()` fornisce il numero totale di record nella tabella di database `Products`.

Questi metodi possono essere utilizzati per eseguire in modo efficiente la pagina e l'ordinamento tramite i dati utilizzando un controllo DataList o Repeater. Per illustrare questo problema, è possibile iniziare creando un controllo Repeater con supporto per il paging personalizzato; verranno quindi aggiunte le funzionalità di ordinamento.

Aprire la pagina `SortingWithCustomPaging.aspx` nella cartella `PagingSortingDataListRepeater` e aggiungere un Repeater alla pagina, impostando la relativa proprietà `ID` su `Products`. Dallo smart tag del Repeater, creare un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurarlo per la selezione dei dati dal metodo `GetProductsPaged` della classe `ProductsBLL`.

[![configurare ObjectDataSource per l'uso del metodo GetProductsPaged della classe ProductsBLL](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per l'uso del metodo `ProductsBLL` Class s `GetProductsPaged` ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))

Impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina in (nessuno), quindi fare clic sul pulsante Avanti. La configurazione guidata origine dati richiede ora le origini dei parametri di input *StartRowIndex* e *maximumRows* del metodo `GetProductsPaged`. In realtà, questi parametri di input vengono ignorati. Al contrario, i valori *StartRowIndex* e *MaximumRows* vengono passati tramite la proprietà `Arguments` nel gestore dell'evento ObjectDataSource s `Selecting`, proprio come in questa demo di questa esercitazione è stata specificata la *SortExpression* . Lasciare quindi gli elenchi a discesa di origine parametri nella procedura guidata impostata su nessuno.

[![lasciare le origini parametro impostate su None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figura 13**: lasciare le origini dei parametri impostate su None ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))

> [!NOTE]
> *Non* impostare la proprietà `EnablePaging` di ObjectDataSource su `true`. In questo modo, ObjectDataSource includerà automaticamente i propri parametri *StartRowIndex* e *MaximumRows* all'elenco di parametri esistente `SelectMethod`. La proprietà `EnablePaging` è utile quando si associano dati di paging personalizzati a un controllo GridView, DetailsView o FormView perché questi controlli prevedono un determinato comportamento da ObjectDataSource che è disponibile solo quando `EnablePaging` proprietà è `true`. Poiché è necessario aggiungere manualmente il supporto per il paging per DataList e Repeater, lasciare questa proprietà impostata su `false` (impostazione predefinita), in quanto verrà preparato nella funzionalità necessaria direttamente nella pagina ASP.NET.

Definire infine le `ItemTemplate` del Repeater in modo che vengano visualizzati il nome, la categoria e il fornitore del prodotto. Una volta apportate queste modifiche, la sintassi dichiarativa Repeater e ObjectDataSource dovrebbe essere simile alla seguente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

È possibile visitare la pagina tramite un browser e notare che non viene restituito alcun record. Questo perché è ancora necessario specificare i valori dei parametri *StartRowIndex* e *MaximumRows* . Pertanto, i valori 0 vengono passati per entrambi. Per specificare questi valori, creare un gestore eventi per l'evento `Selecting` di ObjectDataSource e impostare i valori dei parametri a livello di codice su valori hardcoded rispettivamente 0 e 5:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Con questa modifica, la pagina, se visualizzata tramite un browser, Mostra i primi cinque prodotti.

[![vengono visualizzati i primi cinque record](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Figura 14**: vengono visualizzati i primi cinque record ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))

> [!NOTE]
> I prodotti elencati nella figura 14 sono ordinati in base al nome del prodotto perché la `GetProductsPaged` stored procedure che esegue la query di paging personalizzata efficiente Ordina i risultati per `ProductName`.

Per consentire all'utente di scorrere le pagine, è necessario tenere traccia dell'indice della riga iniziale e del numero massimo di righe e ricordare questi valori nei postback. Nell'esempio di paging predefinito sono stati usati i campi QueryString per salvare in modo permanente questi valori. per questa demo, le informazioni vengono mantenute nello stato di visualizzazione della pagina. Creare le due proprietà seguenti:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Successivamente, aggiornare il codice nel gestore eventi Selecting in modo che usi le proprietà `StartRowIndex` e `MaximumRows` invece dei valori hardcoded 0 e 5:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

A questo punto la pagina mostra ancora solo i primi cinque record. Tuttavia, con queste proprietà sul posto, è possibile creare l'interfaccia di paging.

## <a name="adding-the-paging-interface"></a>Aggiunta dell'interfaccia di paging

Consente di utilizzare la stessa prima, precedente, successiva, ultima interfaccia di paging utilizzata nell'esempio di paging predefinito, incluso il controllo Web Label che visualizza la pagina di dati visualizzata e il numero totale di pagine disponibili. Aggiungere i controlli Web e l'etichetta dei quattro pulsanti sotto il ripetitore.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Successivamente, creare `Click` gestori eventi per i quattro pulsanti. Quando si fa clic su uno di questi pulsanti, è necessario aggiornare il `StartRowIndex` e riassociare i dati al ripetitore. Il codice per i pulsanti primo, precedente e successivo è abbastanza semplice, ma per l'ultimo pulsante come si determina l'indice della riga iniziale per l'ultima pagina di dati? Per calcolare questo indice e poter determinare se i pulsanti avanti e ultimo devono essere abilitati, è necessario sapere il numero totale di record in cui viene eseguito il paging. È possibile determinare questo problema chiamando il metodo `ProductsBLL` Class s `TotalNumberOfProducts()`. Consente di creare una proprietà di sola lettura a livello di pagina denominata `TotalRowCount` che restituisce i risultati del metodo `TotalNumberOfProducts()`:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Con questa proprietà è ora possibile determinare l'ultimo indice della riga iniziale della pagina. In particolare, è il risultato Integer del `TotalRowCount` meno 1 diviso per `MaximumRows`, moltiplicato per `MaximumRows`. È ora possibile scrivere i gestori eventi `Click` per i quattro pulsanti dell'interfaccia di paging:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Infine, è necessario disabilitare i pulsanti primo e indietro nell'interfaccia di paging quando si visualizza la prima pagina di dati e i pulsanti avanti e ultimo quando si visualizza l'ultima pagina. A tale scopo, aggiungere il codice seguente al gestore dell'evento ObjectDataSource s `Selecting`:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Dopo aver aggiunto i gestori eventi `Click` e il codice per abilitare o disabilitare gli elementi dell'interfaccia di paging in base all'indice della riga iniziale corrente, testare la pagina in un browser. Come illustrato nella figura 15, quando si visita per la prima volta la pagina i pulsanti primo e indietro saranno disabilitati. Facendo clic su Avanti viene visualizzata la seconda pagina di dati, mentre se si fa clic su Last viene visualizzata la pagina finale (vedere le figure 16 e 17). Quando si visualizza l'ultima pagina di dati, i pulsanti avanti e ultimo sono disabilitati.

[![i pulsanti precedente e ultimo sono disabilitati quando si visualizza la prima pagina di prodotti](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figura 15**: i pulsanti precedente e ultimo sono disabilitati quando si visualizza la prima pagina di prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))

[![viene visualizzata la seconda pagina di prodotti](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figura 16**: viene visualizzata la seconda pagina di prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))

[![clic per ultimo Visualizza la pagina finale dei dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figura 17**: fare clic su ultima per visualizzare la pagina finale dei dati ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Passaggio 7: inclusione del supporto per l'ordinamento con il ripetitore di paging personalizzato

Ora che è stato implementato il paging personalizzato, è possibile includere il supporto per l'ordinamento. Il metodo `ProductsBLL` Class s `GetProductsPagedAndSorted` ha gli stessi parametri di input *StartRowIndex* e *MaximumRows* di `GetProductsPaged`, ma consente un parametro di input *SortExpression* aggiuntivo. Per usare il metodo `GetProductsPagedAndSorted` da `SortingWithCustomPaging.aspx`, è necessario eseguire i passaggi seguenti:

1. Modificare la proprietà `SelectMethod` di ObjectDataSource da `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Aggiungere un oggetto `Parameter` *SortExpression* alla raccolta di `SelectParameters` di ObjectDataSource.
3. Creare una proprietà di `SortExpression` a livello di pagina privata che rende permanente il relativo valore nei postback tramite lo stato di visualizzazione della pagina.
4. Aggiornare il gestore dell'evento ObjectDataSource s `Selecting` per assegnare il parametro *SortExpression* di ObjectDataSource al valore della proprietà `SortExpression` a livello di pagina.
5. Creare l'interfaccia di ordinamento.

Per iniziare, è necessario aggiornare la proprietà `SelectMethod` di ObjectDataSource e aggiungere una `Parameter`*SortExpression* . Verificare che la proprietà *sortExpression* `Parameter` s `Type` sia impostata su `String`. Al termine di queste prime due attività, il markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Quindi, è necessaria una proprietà di `SortExpression` a livello di pagina il cui valore viene serializzato per visualizzare lo stato. Se non è stato impostato alcun valore dell'espressione di ordinamento, utilizzare ProductName come valore predefinito:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Prima che ObjectDataSource richiami il metodo `GetProductsPagedAndSorted` è necessario impostare il valore di *sortExpression* `Parameter` sul valore della proprietà `SortExpression`. Nel gestore dell'evento `Selecting` aggiungere la riga di codice seguente:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Non resta che implementare l'interfaccia di ordinamento. Come nell'ultimo esempio, l'interfaccia di ordinamento è implementata usando i controlli Web di tre pulsanti che consentono all'utente di ordinare i risultati in base al nome del prodotto, alla categoria o al fornitore.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Creare `Click` gestori di eventi per questi tre controlli Button. Nel gestore dell'evento reimpostare il `StartRowIndex` su 0, impostare il `SortExpression` sul valore appropriato e riassociare i dati al Repeater:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Tutto qui! Sebbene sia stata eseguita una serie di passaggi per ottenere il paging e l'ordinamento personalizzati, i passaggi sono molto simili a quelli necessari per il paging predefinito. La figura 18 Mostra i prodotti quando si visualizza l'ultima pagina di dati quando è ordinata per categoria.

[![viene visualizzata l'ultima pagina di dati, ordinata per categoria.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figura 18**: viene visualizzata l'ultima pagina di dati, ordinata per categoria, ([fare clic per visualizzare l'immagine con dimensioni complete](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))

> [!NOTE]
> Negli esempi precedenti, quando l'ordinamento in base al Supplier suppliname è stato usato come espressione di ordinamento. Tuttavia, per l'implementazione del paging personalizzato, è necessario utilizzare CompanyName. Questo perché il stored procedure responsabile dell'implementazione del paging personalizzato `GetProductsPagedAndSorted` passa l'espressione di ordinamento alla parola chiave `ROW_NUMBER()`, la parola chiave `ROW_NUMBER()` richiede il nome della colonna effettivo anziché un alias. Pertanto, è necessario utilizzare `CompanyName` (il nome della colonna nella tabella `Suppliers`) anziché l'alias utilizzato nella query `SELECT` (`SupplierName`) per l'espressione di ordinamento.

## <a name="summary"></a>Riepilogo

Né DataList né Repeater offrono il supporto per l'ordinamento integrato, ma con un po' di codice e un'interfaccia di ordinamento personalizzata, è possibile aggiungere tale funzionalità. Quando si implementa l'ordinamento, ma non il paging, l'espressione di ordinamento può essere specificata tramite l'oggetto `DataSourceSelectArguments` passato al Metodo ObjectDataSource s `Select`. È possibile assegnare questo `DataSourceSelectArguments` oggetto `SortExpression` proprietà nel gestore dell'evento ObjectDataSource s `Selecting`.

Per aggiungere funzionalità di ordinamento a un oggetto DataList o Repeater che fornisce già il supporto per il paging, l'approccio più semplice consiste nel personalizzare il livello della logica di business in modo da includere un metodo che accetta un'espressione di ordinamento. Queste informazioni possono quindi essere passate tramite un parametro in ObjectDataSource s `SelectParameters`.

Questa esercitazione completa l'analisi del paging e dell'ordinamento con i controlli DataList e Repeater. Nell'esercitazione successiva e finale verrà illustrato come aggiungere controlli Web dei pulsanti ai modelli DataList e Repeater per fornire alcune funzionalità personalizzate e avviate dall'utente per ogni singolo elemento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato David Suru. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Successivo](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
