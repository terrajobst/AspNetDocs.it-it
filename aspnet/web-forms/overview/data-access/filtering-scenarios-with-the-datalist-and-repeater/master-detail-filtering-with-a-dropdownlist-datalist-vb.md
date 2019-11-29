---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Filtro master/dettagli con un oggetto DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene illustrato come visualizzare i report master/dettaglio in una singola pagina Web tramite DropDownList per visualizzare i record ' Master ' e DataList su displ...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 537f8e76bc0cbfa759a014b63ae5f68b5d3ca64d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74630021"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Applicazione di filtri al report master o di dettaglio usando un controllo DropDownList (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) o [scaricare il file PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> In questa esercitazione viene illustrato come visualizzare i report master/dettaglio in una singola pagina Web tramite DropDownList per visualizzare i record "Master" e DataList per visualizzare i "dettagli".

## <a name="introduction"></a>Introduzione

Il report master/dettagli, che è stato creato per la prima volta usando un controllo GridView nel [filtro master/dettaglio precedente con un'esercitazione DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) , inizia mostrando un set di record "Master". L'utente può quindi eseguire il drill-down in uno dei record master, visualizzando quindi i "dettagli" del record master. I report master/dettagli rappresentano la scelta ideale per la visualizzazione di relazioni uno-a-molti e per la visualizzazione di informazioni dettagliate da tabelle in particolare "wide" (quelle che contengono molte colonne). È stata illustrata la procedura per implementare i report master/dettagli utilizzando i controlli GridView e DetailsView nelle esercitazioni precedenti. In questa esercitazione e nei prossimi due verranno riesaminati questi concetti, ma si concentreranno invece sull'uso dei controlli DataList e Repeater.

In questa esercitazione verrà esaminato l'utilizzo di un controllo DropDownList per contenere i record "Master", con i record "Details" visualizzati in un DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Passaggio 1: aggiunta delle pagine Web dell'esercitazione master/dettagli

Prima di iniziare questa esercitazione, è necessario prima di tutto aggiungere la cartella e le pagine di ASP.NET necessarie per questa esercitazione e i due prossimi che trattano i report master/dettagli con i controlli DataList e Repeater. Per iniziare, creare una nuova cartella nel progetto denominata `DataListRepeaterFiltering`. Aggiungere quindi le cinque pagine ASP.NET seguenti a questa cartella, in modo che tutte siano configurate per l'uso della pagina master `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Creare una cartella DataListRepeaterFiltering e aggiungere le pagine dell'esercitazione ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Figura 1**: creare una cartella di `DataListRepeaterFiltering` e aggiungere le pagine dell'esercitazione ASP.NET

Aprire quindi la pagina `Default.aspx` e trascinare il controllo utente `SectionLevelTutorialListing.ascx` dalla cartella `UserControls` nell'area di progettazione. Questo controllo utente, creato nell'esercitazione [pagine master e navigazione nel sito](../introduction/master-pages-and-site-navigation-vb.md) , enumera la mappa del sito e visualizza le esercitazioni dalla sezione corrente in un elenco puntato.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))

Per fare in modo che l'elenco puntato visualizzi le esercitazioni Master/Details da creare, è necessario aggiungerle alla mappa del sito. Aprire il file di `Web.sitemap` e aggiungere il markup seguente dopo il markup del nodo della mappa del sito "visualizzazione dei dati con DataList e Repeater":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]

![Aggiornare la mappa del sito per includere le nuove pagine ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Figura 3**: aggiornare la mappa del sito per includere le nuove pagine ASP.NET

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 2: visualizzazione delle categorie in un oggetto DropDownList

Il report master/dettagli elenca le categorie in un oggetto DropDownList, con i prodotti dell'elemento di elenco selezionati visualizzati più in basso nella pagina di un controllo DataList. La prima attività, quindi, consiste nel fare in modo che le categorie vengano visualizzate in un oggetto DropDownList. Per iniziare, aprire la pagina `FilterByDropDownList.aspx` nella cartella `DataListRepeaterFiltering` e trascinare un oggetto DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina. Impostare quindi la proprietà `ID` di DropDownList su `Categories`. Fare clic sul collegamento Scegli origine dati dallo smart tag di DropDownList e creare un nuovo ObjectDataSource denominato `CategoriesDataSource`.

[![aggiungere un nuovo ObjectDataSource denominato CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Figura 4**: aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))

Configurare il nuovo ObjectDataSource in modo che richiami il metodo `GetCategories()` della classe `CategoriesBLL`. Dopo aver configurato ObjectDataSource, è necessario specificare il campo dell'origine dati da visualizzare nel DropDownList e quale deve essere associato come valore per ogni elemento dell'elenco. Fare in modo che il campo `CategoryName` sia visualizzato e `CategoryID` come valore per ogni elemento dell'elenco.

[![fare in modo che DropDownList visualizzi il campo CategoryName e usare CategoryID come valore](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Figura 5**: fare in modo che l'oggetto DropDownList visualizzi il campo `CategoryName` e usare `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))

A questo punto è presente un controllo DropDownList popolato con i record della tabella `Categories` (tutti eseguiti in circa sei secondi). La figura 6 Mostra lo stato di avanzamento fino a questo punto quando viene visualizzato tramite un browser.

[![elenco A discesa elenca le categorie correnti](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Figura 6**: nell'elenco a discesa sono elencate le categorie correnti ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Passaggio 2: aggiunta del DataList Products

L'ultimo passaggio del report master/dettagli consiste nell'elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un oggetto DataList alla pagina e creare un nuovo ObjectDataSource denominato `ProductsByCategoryDataSource`. Chiedere al controllo `ProductsByCategoryDataSource` di recuperare i dati dal metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`. Poiché il report master/dettagli è di sola lettura, scegliere l'opzione (nessuno) nelle schede Inserisci, aggiorna ed Elimina.

[![selezionare il metodo GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Figura 7**: selezionare il metodo `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))

Dopo aver fatto clic su Avanti, la procedura guidata di ObjectDataSource richiede l'origine del valore per il parametro di *`categoryID`* del metodo `GetProductsByCategoryID(categoryID)`. Per utilizzare il valore dell'elemento `categories` DropDownList selezionato, impostare l'origine del parametro su Control e il ControlID su `Categories`.

[![impostare il parametro categoryID sul valore del DropDownList categorie](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Figura 8**: impostare il parametro *`categoryID`* sul valore dell'oggetto DropDownList `Categories` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))

Al termine della configurazione guidata origine dati, Visual Studio genererà automaticamente un `ItemTemplate` per DataList che Visualizza il nome e il valore di ogni campo dati. Viene ora migliorato il DataList per usare invece un `ItemTemplate` che visualizza solo il nome del prodotto, la categoria, il fornitore, la quantità per unità e il prezzo insieme a una `SeparatorTemplate` che inserisce un elemento `<hr>` tra ogni elemento. Utilizzerò la `ItemTemplate` da un esempio nell'esercitazione [visualizzazione dei dati con i controlli DataList e Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) , ma è possibile usare qualsiasi markup del modello individuato in modo visivo.

Dopo aver apportato queste modifiche, DataList e il markup di ObjectDataSource dovrebbero avere un aspetto simile al seguente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Esaminare lo stato di avanzamento in un browser. Quando si visita la pagina per la prima volta, vengono visualizzati i prodotti appartenenti alla categoria selezionata (bevande), come illustrato nella figura 9. Tuttavia, la modifica del DropDownList non comporta l'aggiornamento dei dati. Questo perché è necessario che venga eseguito un postback per l'aggiornamento di DataList. A tale scopo, è possibile impostare la proprietà `AutoPostBack` di DropDownList su `true` o aggiungere un controllo Web Button alla pagina. Per questa esercitazione si è scelto di impostare la proprietà `AutoPostBack` di DropDownList su `true`.

Le figure 9 e 10 illustrano il report master/dettagli in azione.

[![quando si visita la pagina per la prima volta, vengono visualizzati i prodotti Beverage](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Figura 9**: la prima volta che si visita la pagina, vengono visualizzati i prodotti Beverage ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))

[![la selezione di un nuovo prodotto (produzione) causa automaticamente un PostBack, aggiornando DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Figura 10**: la selezione di un nuovo prodotto (produzione) causa automaticamente un postback, aggiornando DataList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di una voce di elenco "--Choose a Category--"

Per la prima volta che si visita la pagina `FilterByDropDownList.aspx`, per impostazione predefinita viene selezionata la prima voce di elenco (bevande) del DropDownList delle categorie, che mostra i prodotti Beverage nel DataList. Nell'esercitazione sul *filtro master/dettagli con un controllo DropDownList* è stata aggiunta un'opzione "--Choose a Category--" per l'oggetto DropDownList selezionato per impostazione predefinita e, quando selezionato, sono visualizzati *tutti* i prodotti del database. Un approccio di questo tipo è gestibile quando si elencano i prodotti in un controllo GridView, perché ogni riga di prodotto ha occupato una piccola quantità di spazio su schermo. Con DataList, tuttavia, le informazioni di ogni prodotto utilizzano un blocco molto più grande dello schermo. Aggiungere ancora un'opzione "--Choose a Category--" e selezionarla per impostazione predefinita, ma invece di visualizzare tutti i prodotti quando è selezionata, è necessario configurarla in modo che non mostri alcun prodotto.

Per aggiungere un nuovo elemento elenco al DropDownList, passare al Finestra Proprietà e fare clic sui puntini di sospensione nella proprietà `Items`. Aggiungere una nuova voce di elenco con la `Text` "--scegliere una categoria--" e il `0``Value`.

![Aggiungere una](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Figura 11**: aggiungere una voce di elenco "--Choose a Category--"

In alternativa, è possibile aggiungere l'elemento dell'elenco aggiungendo il markup seguente al DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Inoltre, è necessario impostare la `AppendDataBoundItems` del controllo DropDownList su `true` perché se è impostata su `false` (impostazione predefinita), quando le categorie sono associate a DropDownList da ObjectDataSource, sovrascriveranno gli elementi elenco aggiunti manualmente.

![Impostare la proprietà AppendDataBoundItems su true](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Figura 12**: impostare la proprietà `AppendDataBoundItems` su true

Il motivo per cui è stato scelto il valore `0` per la voce di elenco "--Choose a Category--" è perché nel sistema non sono presenti categorie con un valore `0`, pertanto non viene restituito alcun record di prodotto quando viene selezionata la voce di elenco "--Choose a Category--". Per confermare questo problema, visitare la pagina tramite un browser. Come illustrato nella figura 13, durante la visualizzazione iniziale della pagina è selezionata l'opzione "--Scegli una categoria--" e non viene visualizzato alcun prodotto.

[![quando il](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Figura 13**: quando è selezionata la voce di elenco "--Choose a Category--", non viene visualizzato alcun prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))

Se si preferisce visualizzare *tutti* i prodotti quando si seleziona l'opzione "--Choose a Category--", usare invece il valore `-1`. Il lettore astuto ricorderà che nel *filtro master/dettagli con un'esercitazione DropDownList* è stato aggiornato il metodo di `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL` in modo che se è stato passato un valore *`categoryID`* di `-1`, vengono restituiti tutti i record di prodotto.

## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati gerarchicamente, spesso è utile presentare i dati utilizzando i report master/dettagli, da cui l'utente può iniziare a esaminare i dati dalla parte superiore della gerarchia ed eseguire il drill-down nei dettagli. In questa esercitazione è stata esaminata la creazione di un semplice report master/dettagli che mostra i prodotti di una categoria selezionata. Questa operazione è stata eseguita utilizzando un oggetto DropDownList per l'elenco di categorie e un oggetto DataList per i prodotti appartenenti alla categoria selezionata.

Nell'esercitazione successiva si esaminerà la separazione dei record master e Details in due pagine. Nella prima pagina verrà visualizzato un elenco di record "Master" con un collegamento per visualizzare i dettagli. Facendo clic sul collegamento, l'utente viene sbattuto sulla seconda pagina, in cui vengono visualizzati i dettagli per il record master selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Randy Schmidt. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [Successivo](master-detail-filtering-acess-two-pages-datalist-vb.md)
