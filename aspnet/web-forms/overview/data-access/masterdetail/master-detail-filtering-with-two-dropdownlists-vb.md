---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Filtro master/dettagli con due DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione espande la relazione Master/Detail per aggiungere un terzo livello, usando due controlli DropDownList per selezionare le recor padre e padre desiderate...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 166d6a7664a326361dc2a3f115eddb988cd39d20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528497"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Applicazione di filtri al report master o di dettaglio usando due controlli DropDownList (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) o [scaricare il file PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Questa esercitazione espande la relazione Master/Detail per aggiungere un terzo livello, usando due controlli DropDownList per selezionare i record padre e nonni desiderati.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](master-detail-filtering-with-a-dropdownlist-vb.md) è stato illustrato come visualizzare un semplice report master/dettagli utilizzando un singolo oggetto DropDownList popolato con le categorie e un controllo GridView che mostra i prodotti che appartengono alla categoria selezionata. Questo modello di report funziona correttamente quando si visualizzano i record che hanno una relazione uno-a-molti e possono essere facilmente estesi per funzionare per gli scenari che includono più relazioni uno-a-molti. Ad esempio, un sistema di immissione degli ordini avrebbe tabelle che corrispondono a clienti, ordini e voci dell'ordine. Un determinato cliente può disporre di più ordini con ogni ordine costituito da più elementi. Tali dati possono essere presentati all'utente con due DropDownList e un controllo GridView. Il primo DropDownList avrà una voce di elenco per ogni cliente nel database il cui contenuto è quello del secondo, ovvero gli ordini effettuati dal cliente selezionato. Un controllo GridView elenca le voci dell'ordine selezionato.

Sebbene il database Northwind includa le informazioni canoniche relative a clienti/ordini/ordini nelle tabelle `Customers`, `Orders`e `Order Details`, queste tabelle non vengono acquisite nell'architettura. Tuttavia, è comunque possibile illustrare l'utilizzo di due DropDownList dipendenti. Il primo DropDownList elenca le categorie e la seconda i prodotti appartenenti alla categoria selezionata. In un DetailsView vengono elencati i dettagli del prodotto selezionato.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Passaggio 1: creazione e popolamento del DropDownList categorie

Il primo obiettivo consiste nell'aggiungere il DropDownList che elenca le categorie. Questi passaggi sono stati esaminati in dettaglio nell'esercitazione precedente, ma sono riepilogati qui per completezza.

Aprire la pagina `MasterDetailsDetails.aspx` nella cartella `Filtering`, aggiungere un oggetto DropDownList alla pagina, impostare la relativa proprietà `ID` su `Categories`e quindi fare clic sul collegamento Configura origine dati nello smart tag. Dalla configurazione guidata origine dati scegliere di aggiungere una nuova origine dati.

[![aggiungere una nuova origine dati per il DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Figura 1**: aggiungere una nuova origine dati per il DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))

La nuova origine dati deve essere naturalmente un ObjectDataSource. Assegnare al nuovo ObjectDataSource il nome `CategoriesDataSource` e fare in modo che richiami il metodo `GetCategories()` dell'oggetto `CategoriesBLL`.

[![scegliere di utilizzare la classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Figura 2**: scegliere di usare la classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))

[![configurare ObjectDataSource per l'utilizzo del metodo GetCategories ()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per l'uso del metodo `GetCategories()` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))

Dopo aver configurato ObjectDataSource, è ancora necessario specificare il campo dell'origine dati da visualizzare nel `Categories` DropDownList e quale deve essere configurato come valore per l'elemento dell'elenco. Impostare il campo `CategoryName` come visualizzato e `CategoryID` come valore per ogni elemento dell'elenco.

[![fare in modo che DropDownList visualizzi il campo CategoryName e usare CategoryID come valore](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Figura 4**: fare in modo che l'oggetto DropDownList visualizzi il campo `CategoryName` e usare `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))

A questo punto è presente un controllo DropDownList (`Categories`) popolato con i record della tabella `Categories`. Quando l'utente sceglie una nuova categoria dal DropDownList, è necessario che si verifichi un postback per aggiornare il prodotto DropDownList che verrà creato nel passaggio 2. Quindi, selezionare l'opzione Abilita AutoPostBack dallo smart tag del `categories` DropDownList.

[![abilitare il PostBack automatico per le categorie (DropDownList)](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Figura 5**: abilitare il postback automatico per il `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Passaggio 2: visualizzazione dei prodotti della categoria selezionata in un secondo DropDownList

Con la `Categories` DropDownList completata, il passaggio successivo consiste nel visualizzare un oggetto DropDownList dei prodotti appartenenti alla categoria selezionata. A tale scopo, aggiungere un altro DropDownList alla pagina denominata `ProductsByCategory`. Come per il `Categories` DropDownList, creare un nuovo ObjectDataSource per il `ProductsByCategory` DropDownList denominato `ProductsByCategoryDataSource`.

[![aggiungere una nuova origine dati per l'oggetto DropDownList ProductsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Figura 6**: aggiungere una nuova origine dati per il `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))

[![creare un nuovo ObjectDataSource denominato ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Figura 7**: creare un nuovo ObjectDataSource denominato `ProductsByCategoryDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))

Poiché il `ProductsByCategory` DropDownList deve visualizzare solo i prodotti appartenenti alla categoria selezionata, fare in modo che ObjectDataSource richiami il metodo `GetProductsByCategoryID(categoryID)` dall'oggetto `ProductsBLL`.

[![scegliere di utilizzare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Figura 8**: scegliere di usare la classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))

[![configurare ObjectDataSource per l'utilizzo del metodo GetProductsByCategoryID (categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Figura 9**: configurare ObjectDataSource per l'uso del metodo `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))

Nel passaggio finale della procedura guidata è necessario specificare il valore del parametro *`categoryID`* . Assegnare questo parametro all'elemento selezionato dal `Categories` DropDownList.

[![effettuare il pull del valore del parametro categoryID dall'oggetto DropDownList categorie](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Figura 10**: eseguire il pull del valore del parametro *`categoryID`* dal `Categories` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))

Con ObjectDataSource configurato, rimane solo specificare i campi dell'origine dati da utilizzare per la visualizzazione e il valore degli elementi dell'oggetto DropDownList. Visualizzare il campo `ProductName` e utilizzare il campo `ProductID` come valore.

[![specificare i campi dell'origine dati utilizzati per le proprietà Text e value degli ListItem di DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Figura 11**: specificare i campi dell'origine dati usati per le proprietà `Text` e `Value` di DropDownList `ListItem` s ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))

Con ObjectDataSource e `ProductsByCategory` DropDownList configurati nella pagina verranno visualizzati due DropDownList: la prima elenca tutte le categorie, mentre la seconda elenca i prodotti appartenenti alla categoria selezionata. Quando l'utente seleziona una nuova categoria dal primo DropDownList, sarà seguito da un postback e il secondo DropDownList verrà riassociato, mostrando i prodotti che appartengono alla categoria appena selezionata. Le figure 12 e 13 mostrano `MasterDetailsDetails.aspx` in azione quando vengono visualizzate tramite un browser.

[![quando si visita la pagina per la prima volta, viene selezionata la categoria bevande](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Figura 12**: la prima volta che si visita la pagina, la categoria bevande è selezionata ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))

[![la scelta di una categoria diversa Visualizza i prodotti della nuova categoria](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Figura 13**: la scelta di una categoria diversa Visualizza i prodotti della nuova categoria ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))

Attualmente il `productsByCategory` DropDownList *, se modificato, non provoca un* postback. Tuttavia, si desidera che venga eseguito un postback una volta aggiunto un DetailsView per visualizzare i dettagli del prodotto selezionato (passaggio 3). Quindi, selezionare la casella di controllo Abilita AutoPostBack dallo smart tag del `productsByCategory` DropDownList.

[![abilitare la funzionalità AutoPostBack per l'oggetto DropDownList productsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Figura 14**: abilitare la funzionalità di postback automatico per il `productsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Passaggio 3: uso di un oggetto DetailsView per visualizzare i dettagli del prodotto selezionato

Il passaggio finale consiste nel visualizzare i dettagli relativi al prodotto selezionato in un oggetto DetailsView. A tale scopo, aggiungere un oggetto DetailsView alla pagina, impostare la relativa proprietà `ID` su `ProductDetails`e creare un nuovo oggetto ObjectDataSource. Configurare questo ObjectDataSource per eseguire il pull dei dati dal metodo `GetProductByProductID(productID)` della classe `ProductsBLL` usando il valore selezionato del `ProductsByCategory` DropDownList per il valore del parametro *`productID`* .

[![scegliere di utilizzare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Figura 15**: scegliere di usare la classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))

[![configurare ObjectDataSource per l'utilizzo del metodo GetProductByProductID (productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Figura 16**: configurare ObjectDataSource per l'uso del metodo `GetProductByProductID(productID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))

[![effettuare il pull del valore del parametro productID dall'oggetto DropDownList ProductsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Figura 17**: effettuare il pull del valore del parametro *`productID`* dal `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))

È possibile scegliere di visualizzare i campi disponibili nell'`ProductDetails` DetailsView. Si è scelto di rimuovere i campi `ProductID`, `SupplierID`e `CategoryID` e di riordinare e formattare i campi rimanenti. Sono state inoltre cancellate le proprietà `Height` e `Width` di DetailsView, consentendo a DetailsView di espandersi in base alla larghezza necessaria per visualizzare i dati in modo ottimale anziché limitarli a una dimensione specificata. Il markup completo viene visualizzato di seguito:

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Provare la pagina `MasterDetailsDetails.aspx` in un browser. A prima vista può sembrare che tutto funzioni come desiderato, ma c'è un problema sottile. Quando si sceglie una nuova categoria, il `ProductsByCategory` DropDownList viene aggiornato in modo da includere i prodotti per la categoria selezionata, ma il `ProductDetails` DetailsView continua a visualizzare le informazioni sul prodotto precedenti. Il DetailsView viene aggiornato quando si sceglie un prodotto diverso per la categoria selezionata. Inoltre, se si esegue un test sufficientemente approfondito, si noterà che se si scelgono continuamente nuove categorie, ad esempio scegliendo bevande dal `Categories` DropDownList, quindi condimenti, quindi confezioni, ogni altra selezione di categoria causa l'aggiornamento del `ProductDetails` DetailsView.

Per contribuire a concretizzare la questo problema, esaminiamo un esempio specifico. Quando si visita la pagina per la prima volta, viene selezionata la categoria bevande e i prodotti correlati vengono caricati nel `ProductsByCategory` DropDownList. Chai è il prodotto selezionato e i relativi dettagli vengono visualizzati nel `ProductDetails` DetailsView, come illustrato nella figura 18.

[![i dettagli del prodotto selezionato vengono visualizzati in un oggetto DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Figura 18**: i dettagli del prodotto selezionato vengono visualizzati in un DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))

Se si modifica la selezione della categoria da bevande a condimenti, viene eseguito un postback e il `ProductsByCategory` DropDownList viene aggiornato di conseguenza, ma il DetailsView visualizza comunque i dettagli per Chai.

[![vengono comunque visualizzati i dettagli del prodotto selezionato in precedenza](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Figura 19**: i dettagli del prodotto selezionato in precedenza sono ancora visualizzati ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))

Se si seleziona un nuovo prodotto dall'elenco, il DetailsView viene aggiornato come previsto. Se si seleziona una nuova categoria dopo aver modificato il prodotto, il servizio DetailsView non verrà aggiornato. Tuttavia, se invece di scegliere un nuovo prodotto è stata selezionata una nuova categoria, l'oggetto DetailsView verrà aggiornato. Che cosa succede in questo mondo?

Il problema è un problema di temporizzazione nel ciclo di vita della pagina. Ogni volta che viene richiesta una pagina, viene eseguito un certo numero di passaggi come rendering. In uno di questi passaggi i controlli ObjectDataSource verificano se uno dei valori `SelectParameters` è stato modificato. In tal caso, il controllo Web dei dati associato a ObjectDataSource sa che è necessario aggiornare la visualizzazione. Ad esempio, quando si seleziona una nuova categoria, il `ProductsByCategoryDataSource` ObjectDataSource rileva che i valori dei parametri sono stati modificati e il `ProductsByCategory` DropDownList viene riassociato, ottenendo i prodotti per la categoria selezionata.

Il problema che si verifica in questa situazione è che il punto del ciclo di vita della pagina controllato da ObjectDataSource per i parametri modificati si verifica *prima* della riassociazione dei controlli Web dei dati associati. Pertanto, quando si seleziona una nuova categoria, il `ProductsByCategoryDataSource` ObjectDataSource rileva una modifica nel valore del relativo parametro. Il ObjectDataSource utilizzato dal `ProductDetails` DetailsView, tuttavia, non annotare tali modifiche perché il `ProductsByCategory` DropDownList deve ancora essere riassociato. Più avanti nel ciclo di vita, il `ProductsByCategory` DropDownList viene riassociato al relativo ObjectDataSource, afferrando i prodotti per la categoria appena selezionata. Mentre il valore del `ProductsByCategory` DropDownList è stato modificato, ObjectDataSource ha già eseguito il controllo `ProductDetails` del valore del parametro. Pertanto, DetailsView Visualizza i risultati precedenti. Questa interazione è illustrata nella figura 20.

[![il valore di DropDownList ProductsByCategory cambia dopo che ObjectDataSource di ProductDetails controlla la presenza di modifiche](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Figura 20**: il valore della `ProductsByCategory` DropDownList cambia dopo che l'oggetto ObjectDataSource di `ProductDetails` controlla le modifiche ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))

Per risolvere questo problema, è necessario riassociare in modo esplicito il `ProductDetails` DetailsView dopo che è stato associato il `ProductsByCategory` DropDownList. A tale scopo, è possibile chiamare il metodo `DataBind()` di `ProductDetails` DetailsView quando viene generato l'evento `DataBound` dell'oggetto DropDownList `ProductsByCategory`. Aggiungere il seguente codice del gestore eventi alla classe code-behind della pagina `MasterDetailsDetails.aspx` (per una discussione su come aggiungere un gestore eventi, vedere l'articolo relativo all'[impostazione a livello di programmazione dei valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)):

[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Una volta aggiunta la chiamata esplicita al metodo `DataBind()` di `ProductDetails` DetailsView, l'esercitazione funziona come previsto. La figura 21 evidenzia il modo in cui questa modifica ha risolto il problema precedente.

[![l'oggetto DetailsView ProductDetails viene aggiornato in modo esplicito quando viene generato l'evento di associazione a un oggetto di ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Figura 21**: il `ProductDetails` DetailsView viene aggiornato in modo esplicito quando viene generato l'evento di `DataBound` di `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))

## <a name="summary"></a>Riepilogo

Il controllo DropDownList funge da elemento dell'interfaccia utente ideale per i report master/dettagli in cui è presente una relazione uno-a-molti tra i record master e dettagli. Nell'esercitazione precedente è stato illustrato come utilizzare un singolo DropDownList per filtrare i prodotti visualizzati dalla categoria selezionata. In questa esercitazione è stato sostituito il GridView dei prodotti con un DropDownList ed è stato usato un oggetto DetailsView per visualizzare i dettagli del prodotto selezionato. I concetti illustrati in questa esercitazione possono essere facilmente estesi ai modelli di dati che coinvolgono più relazioni uno-a-molti, ad esempio clienti, ordini e elementi dell'ordine. In generale, è sempre possibile aggiungere un oggetto DropDownList per ciascuna entità "One" nelle relazioni uno-a-molti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Successivo](master-detail-filtering-across-two-pages-vb.md)
