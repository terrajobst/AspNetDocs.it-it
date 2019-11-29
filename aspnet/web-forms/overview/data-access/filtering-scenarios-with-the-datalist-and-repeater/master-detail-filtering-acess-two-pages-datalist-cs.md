---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtro master/dettagli in due pagine (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene illustrato come separare un report master/dettagli in due pagine. Nella pagina ' Master ' viene usato un controllo Repeater per eseguire il rendering di un elenco di categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 545b24a66476c55aff88ac62d3a6528105fea6c0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631350"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Applicazione di filtri a report master o di dettaglio su due pagine (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) o [scaricare il file PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> In questa esercitazione viene illustrato come separare un report master/dettagli in due pagine. Nella pagina "Master" viene usato un controllo Repeater per eseguire il rendering di un elenco di categorie che, quando selezionate, porteranno l'utente alla pagina "Details", in cui un DataList a due colonne Mostra i prodotti appartenenti alla categoria selezionata.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) è stato illustrato come visualizzare i report master/dettaglio in una singola pagina Web tramite DropDownList per visualizzare i record "Master" e DataList per visualizzare i "dettagli". Un altro modello comune usato per i report master/dettagli consiste nel disporre dei record master in una pagina Web e nei dettagli di un altro modello. Nell'esercitazione precedente [filtro master/dettagli in due pagine](../masterdetail/master-detail-filtering-across-two-pages-cs.md) , abbiamo esaminato questo modello usando GridView per visualizzare tutti i fornitori nel sistema. In GridView è incluso un HyperLinkField, che è stato sottoposto a rendering come collegamento a una seconda pagina, passando il `SupplierID` nella QueryString. Nella seconda pagina è stato utilizzato un controllo GridView per elencare i prodotti forniti dal fornitore selezionato.

Tali report master/dettagli a due pagine possono essere eseguiti anche tramite i controlli DataList e Repeater. L'unica differenza è che né DataList né Repeater forniscono il supporto per il controllo HyperLinkField. Al contrario, è necessario aggiungere un controllo Web collegamento ipertestuale o un elemento HTML di ancoraggio (`<a>`) nell'`ItemTemplate`del controllo. La proprietà `NavigateUrl` del collegamento ipertestuale o l'attributo `href` dell'ancoraggio può quindi essere personalizzato usando approcci dichiarativi o programmatici.

In questa esercitazione verrà esaminato un esempio in cui sono elencate le categorie in un elenco puntato in una pagina usando un controllo Repeater. Ogni voce di elenco includerà il nome e la descrizione della categoria e il nome della categoria verrà visualizzato come collegamento a una seconda pagina. Se si fa clic su questo collegamento, l'utente viene sbattuto sulla seconda pagina, in cui in un DataList verranno visualizzati i prodotti che appartengono alla categoria selezionata.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Passaggio 1: visualizzazione delle categorie in un elenco puntato

Il primo passaggio nella creazione di un report master/dettagli consiste nell'iniziare visualizzando i record "Master". Pertanto, la prima attività consiste nel visualizzare le categorie nella pagina "Master". Aprire la pagina `CategoryListMaster.aspx` nella cartella `DataListRepeaterFiltering`, aggiungere un controllo Repeater e, dallo smart tag, scegliere di aggiungere un nuovo ObjectDataSource. Configurare il nuovo ObjectDataSource in modo che acceda ai dati dal metodo `GetCategories` della classe `CategoriesBLL` (vedere la figura 1).

[![configurare ObjectDataSource per l'uso del metodo GetCategories della classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figura 1**: configurare ObjectDataSource per l'uso del metodo `GetCategories` della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))

Definire quindi i modelli del Repeater in modo da visualizzare il nome e la descrizione di ogni categoria come un elemento in un elenco puntato. Non è ancora necessario che ogni categoria sia collegata alla pagina dei dettagli. Di seguito viene illustrato il markup dichiarativo per Repeater e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Con questo markup completo, dedicare un po' di tempo alla visualizzazione dello stato di avanzamento tramite un browser. Come illustrato nella figura 2, il ripetitore esegue il rendering come elenco puntato che mostra il nome e la descrizione di ogni categoria.

[![ogni categoria viene visualizzata come elemento elenco puntato](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figura 2**: ogni categoria viene visualizzata come elemento elenco puntato ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Passaggio 2: trasformare il nome della categoria in un collegamento alla pagina dei dettagli

Per consentire a un utente di visualizzare le informazioni "dettagli" per una determinata categoria, è necessario aggiungere un collegamento a ogni elemento dell'elenco puntato che, quando selezionato, consentirà all'utente di passare alla seconda pagina (`ProductsForCategoryDetails.aspx`). Questa seconda pagina visualizzerà quindi i prodotti per la categoria selezionata usando un DataList. Per determinare la categoria di cui è stato fatto clic sul collegamento, è necessario passare la `CategoryID` della categoria con clic alla seconda pagina tramite un meccanismo. Il modo più semplice e diretto per trasferire i dati scalari da una pagina all'altra avviene tramite la QueryString, che è l'opzione che verrà usata in questa esercitazione. In particolare, nella pagina `ProductsForCategoryDetails.aspx` si prevede che il valore *`categoryID`* selezionato venga passato tramite un campo querystring denominato `CategoryID`. Per visualizzare, ad esempio, i prodotti per la categoria bevande, che ha un `CategoryID` di 1, un utente può visitare `ProductsForCategoryDetails.aspx?CategoryID=1`.

Per creare un collegamento ipertestuale per ogni elemento dell'elenco puntato nel ripetitore, è necessario aggiungere un controllo Web collegamento ipertestuale o un elemento ancoraggio HTML (`<a>`) al `ItemTemplate`. Negli scenari in cui il collegamento ipertestuale viene visualizzato allo stesso modo per ogni riga, è sufficiente uno dei due approcci. Per i ripetitori, preferisco usare l'elemento Anchor. Per usare l'elemento anchor, aggiornare ItemTemplate del Repeater a:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Si noti che è possibile inserire il `CategoryID` direttamente all'interno dell'attributo `href` dell'elemento Anchor. Tuttavia, a tale scopo, è necessario delimitare il valore dell'attributo `href` con apostrofi (e virgolette) poiché il metodo `Eval` all'interno dell'attributo `href` delimita la relativa stringa (`"CategoryID"`) con virgolette. In alternativa, è possibile utilizzare un controllo Web collegamento ipertestuale:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Si noti il modo in cui la parte statica dell'URL, `ProductsForCategoryDetails.aspx?CategoryID`, viene aggiunta al risultato di `Eval("CategoryID")` direttamente all'interno della sintassi di associazione dati tramite la concatenazione di stringhe.

Un vantaggio dell'uso del controllo HyperLink è che è possibile accedervi a livello di codice dal gestore dell'evento `ItemDataBound` del Repeater, se necessario. Ad esempio, potrebbe essere necessario visualizzare il nome della categoria come testo anziché come collegamento per le categorie senza prodotti associati. Un controllo di questo tipo può essere eseguito a livello di codice nel gestore dell'evento `ItemDataBound`; per le categorie senza prodotti associati, la proprietà `NavigateUrl` del collegamento ipertestuale può essere impostata su una stringa vuota, causando in tal modo il rendering del nome di categoria specifico come testo normale, anziché come collegamento. Per ulteriori informazioni sulla formattazione del contenuto di DataList e Repeater in base alla logica a livello di codice tramite il gestore dell'evento `ItemDataBound`, vedere l'esercitazione [formattare l'oggetto DataList e Repeater basato sui dati](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) .

Se si sta seguendo la procedura, è possibile usare l'approccio dell'elemento ancoraggio o del controllo collegamento ipertestuale nella pagina. Indipendentemente dall'approccio, quando si visualizza la pagina tramite un browser è necessario eseguire il rendering di ogni nome di categoria come collegamento a `ProductsForCategoryDetails.aspx`, passando il valore `CategoryID` applicabile (vedere la figura 3).

[![il collegamento dei nomi di categoria ora a ProductsForCategoryDetails. aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figura 3**: i nomi di categoria ora si collegano a `ProductsForCategoryDetails.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Passaggio 3: elenco dei prodotti che appartengono alla categoria selezionata

Una volta completata la `CategoryListMaster.aspx` pagina, è possibile attirare l'attenzione sull'implementazione della pagina "dettagli", `ProductsForCategoryDetails.aspx`. Aprire questa pagina, trascinare un oggetto DataList dalla casella degli strumenti nella finestra di progettazione e impostare la relativa proprietà `ID` su `ProductsInCategory`. Successivamente, dallo smart tag di DataList scegliere di aggiungere un nuovo ObjectDataSource alla pagina, denominarlo `ProductsInCategoryDataSource`. Configurarlo in modo che chiami il metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`; impostare gli elenchi a discesa nelle schede Inserisci, aggiorna ed Elimina su (nessuno).

[![configurare ObjectDataSource per l'utilizzo del metodo GetProductsByCategoryID (categoryID) della classe ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per l'uso del metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))

Poiché il metodo `GetProductsByCategoryID(categoryID)` accetta un parametro di input ( *`categoryID`* ), la scelta guidata origine dati offre l'opportunità di specificare l'origine del parametro. Impostare l'origine del parametro su QueryString usando il `CategoryID`QueryStringField.

[![usare CategoryID il campo QueryString come origine del parametro](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figura 5**: usare il campo QueryString `CategoryID` come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))

Come illustrato nelle esercitazioni precedenti, dopo aver completato la scelta guidata origine dati, Visual Studio crea automaticamente un `ItemTemplate` per DataList che elenca ogni nome e valore del campo dati. Sostituire il modello con uno che elenca solo il nome, il fornitore e il prezzo del prodotto. Inoltre, impostare la proprietà `RepeatColumns` di DataList su 2. Una volta apportate queste modifiche, il markup dichiarativo di DataList e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Per visualizzare questa pagina in azione, iniziare dalla pagina `CategoryListMaster.aspx`; Fare quindi clic su un collegamento nell'elenco elenchi puntati categorie. Questa operazione consentirà di `ProductsForCategoryDetails.aspx`, passando il `CategoryID` attraverso la QueryString. Il `ProductsInCategoryDataSource` ObjectDataSource in `ProductsForCategoryDetails.aspx` otterrà quindi solo i prodotti per la categoria specificata e li visualizzerà nell'oggetto DataList, che esegue il rendering di due prodotti per riga. Nella figura 6 è illustrata una schermata di `ProductsForCategoryDetails.aspx` quando si visualizzano le bevande.

[![vengono visualizzate le bevande, due per riga](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figura 6**: vengono visualizzate le bevande, due per riga ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Passaggio 4: visualizzazione delle informazioni sulle categorie in ProductsForCategoryDetails. aspx

Quando un utente fa clic su una categoria in `CategoryListMaster.aspx`, viene portato a `ProductsForCategoryDetails.aspx` e Mostra i prodotti che appartengono alla categoria selezionata. Tuttavia, in `ProductsForCategoryDetails.aspx` non sono presenti segnali visivi per la categoria selezionata. Un utente che ha fatto clic su bevande, ma ha fatto clic su condimenti accidentalmente, non ha alcun modo per realizzare il proprio errore una volta raggiunta `ProductsForCategoryDetails.aspx`. Per risolvere questo problema potenziale, è possibile visualizzare le informazioni sulla categoria selezionata, ovvero il nome e la descrizione, nella parte superiore della pagina `ProductsForCategoryDetails.aspx`.

A tale scopo, aggiungere un controllo FormView sopra il controllo Repeater in `ProductsForCategoryDetails.aspx`. Successivamente, aggiungere un nuovo ObjectDataSource alla pagina dallo smart tag di FormView denominato `CategoryDataSource` e configurarlo per l'uso del metodo `GetCategoryByCategoryID(categoryID)` della classe `CategoriesBLL`.

[![accedere alle informazioni sulla categoria tramite il metodo GetCategoryByCategoryID (categoryID) della classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figura 7**: accedere alle informazioni sulla categoria tramite il metodo di `GetCategoryByCategoryID(categoryID)` della classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))

Come per il `ProductsInCategoryDataSource` ObjectDataSource aggiunto nel passaggio 3, la procedura guidata Configura origine dati del `CategoryDataSource`richiede di specificare un'origine per il parametro di input del metodo di `GetCategoryByCategoryID(categoryID)`. Usare esattamente le stesse impostazioni di prima, impostando il parametro source su QueryString e il valore QueryStringField su `CategoryID` (fare riferimento alla figura 5).

Dopo aver completato la procedura guidata, Visual Studio crea automaticamente un `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate` per FormView. Poiché viene offerta un'interfaccia di sola lettura, è possibile rimuovere il `EditItemTemplate` e `InsertItemTemplate`. Inoltre, è possibile personalizzare la `ItemTemplate`di FormView. Dopo la rimozione dei modelli superflui e la personalizzazione di ItemTemplate, il markup dichiarativo di FormView e ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

La figura 8 Mostra uno screenshot quando si visualizza questa pagina tramite un browser.

> [!NOTE]
> Oltre a FormView, ho aggiunto anche un controllo collegamento ipertestuale sopra l'oggetto FormView che riporterà l'utente all'elenco di categorie (`CategoryListMaster.aspx`). È possibile inserire questo collegamento altrove oppure ometterlo completamente.

[![informazioni sulle categorie sono ora visualizzate nella parte superiore della pagina](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figura 8**: le informazioni sulle categorie sono ora visualizzate nella parte superiore della pagina ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Passaggio 5: visualizzazione di un messaggio se nessun prodotto appartiene alla categoria selezionata

Nella pagina `CategoryListMaster.aspx` sono elencate tutte le categorie del sistema, indipendentemente dal fatto che siano presenti prodotti associati. Se un utente fa clic su una categoria senza prodotti associati, non verrà eseguito il rendering del DataList in `ProductsForCategoryDetails.aspx`, perché l'origine dati non avrà alcun elemento. Come illustrato nelle esercitazioni precedenti, GridView fornisce una proprietà di `EmptyDataText` che può essere usata per specificare un messaggio di testo da visualizzare se non sono presenti record nell'origine dati. Sfortunatamente, né DataList né Repeater hanno tale proprietà.

Per visualizzare un messaggio che informa l'utente che non sono presenti prodotti corrispondenti per la categoria selezionata, è necessario aggiungere un controllo etichetta alla pagina il cui `Text` proprietà viene assegnato il messaggio da visualizzare nel caso in cui non siano presenti prodotti corrispondenti. È quindi necessario impostare a livello di codice la relativa proprietà `Visible` a seconda che DataList contenga o meno elementi.

A tale scopo, iniziare aggiungendo un'etichetta sotto DataList. Impostarne la proprietà `ID` su `NoProductsMessage` e la relativa proprietà `Text` su "non sono presenti prodotti per la categoria selezionata..." A questo punto, è necessario impostare a livello di codice la proprietà `Visible` dell'etichetta a seconda che i dati siano stati associati al `ProductsInCategory` DataList. Questa assegnazione deve essere effettuata dopo che i dati sono stati associati a DataList. Per GridView, DetailsView e FormView, è possibile creare un gestore eventi per l'evento `DataBound` del controllo, che viene attivato dopo il completamento dell'associazione dati. Tuttavia, né DataList né Repeater hanno un evento `DataBound` disponibile.

Per questo particolare esempio è possibile assegnare la proprietà `Visible` dell'etichetta nel gestore dell'evento `Page_Load`, perché i dati sono stati assegnati a DataList prima dell'evento `Load` della pagina. Tuttavia, questo approccio non funziona nel caso generale, perché i dati di ObjectDataSource potrebbero essere associati a DataList più avanti nel ciclo di vita della pagina. Se, ad esempio, i dati visualizzati sono basati sul valore di un altro controllo, ad esempio quando si visualizza un report master/dettagli utilizzando un oggetto DropDownList per conservare i record "Master", è possibile che i dati non vengano riassociati al controllo Web dei dati fino alla fase di `PreRender` nel ciclo di vita della pagina.

Una soluzione che funzionerà per tutti i casi consiste nell'assegnare la proprietà `Visible` `False` nel gestore eventi `ItemDataBound` o `ItemCreated`di DataList quando si associa un tipo di elemento `Item` o `AlternatingItem`. In tal caso, è possibile che sia presente almeno un elemento di dati nell'origine dati, pertanto è possibile nascondere l'etichetta `NoProductsMessage`. Oltre a questo gestore eventi, è necessario anche un gestore eventi per l'evento `DataBinding` di DataList, in cui si inizializza la proprietà `Visible` dell'etichetta su `True`. Poiché l'evento `DataBinding` viene generato prima degli eventi `ItemDataBound`, la proprietà `Visible` dell'etichetta viene inizialmente impostata su `True`; Se sono presenti elementi di dati, tuttavia, verrà impostato su `False`. Il codice seguente implementa questa logica:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Tutte le categorie del database Northwind sono associate a uno o più prodotti. Per eseguire il test di questa funzionalità, ho modificato manualmente il database Northwind per questa esercitazione, riassegnando tutti i prodotti associati alla categoria produce (`CategoryID` = 7) alla categoria Seafood (`CategoryID` = 8). Questa operazione può essere eseguita dall'Esplora server scegliendo nuova query e utilizzando l'istruzione `UPDATE` seguente:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Al termine dell'aggiornamento del database, tornare alla pagina `CategoryListMaster.aspx` e fare clic sul collegamento genera. Poiché non sono più presenti prodotti appartenenti alla categoria produce, viene visualizzato il "non sono presenti prodotti per la categoria selezionata..." messaggio, come illustrato nella figura 9.

[![viene visualizzato un messaggio se non sono presenti prodotti appartenenti alla categoria selezionata](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figura 9**: viene visualizzato un messaggio se non sono presenti prodotti appartenenti alla categoria selezionata ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))

## <a name="summary"></a>Riepilogo

Mentre i report master/dettagli possono visualizzare sia i record master che i record di dettaglio in una singola pagina, in molti siti Web vengono separati tra due pagine Web. In questa esercitazione è stato illustrato come implementare un report master/dettagli con le categorie elencate in un elenco puntato usando un Repeater nella pagina Web "Master" e i prodotti associati elencati nella pagina "dettagli". Ogni voce di elenco nella pagina Web master contiene un collegamento alla pagina dei dettagli passata lungo il valore `CategoryID` della riga.

Nella pagina dei dettagli il recupero dei prodotti per il fornitore specificato è stato eseguito tramite il metodo di `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`. Il valore del parametro *`categoryID`* è stato specificato in modo dichiarativo usando il `CategoryID` valore QueryString come origine del parametro. È stato inoltre illustrato come visualizzare i dettagli di categoria nella pagina dei dettagli utilizzando un FormView e come visualizzare un messaggio se non sono presenti prodotti appartenenti alla categoria selezionata.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones e Liz Shulok. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Successivo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
