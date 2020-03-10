---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Filtro master/dettagli con un oggetto DropDownListC#() | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 3ec549f9da7a2b3a021e77827f0039e6ae60b5c5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528819"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Applicazione di filtri al report master o di dettaglio usando un controllo DropDownList (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) o [scaricare il file PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> In questa esercitazione verrà illustrato come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in GridView.

## <a name="introduction"></a>Introduzione

Un tipo comune di report è il *report master/dettagli*, in cui il report inizia mostrando un set di record "Master". L'utente può quindi eseguire il drill-down in uno dei record master, visualizzando quindi i "dettagli" del record master. I report master/dettagli rappresentano la scelta ideale per la visualizzazione di relazioni uno-a-molti, ad esempio un report che Mostra tutte le categorie e consente quindi a un utente di selezionare una determinata categoria e visualizzare i prodotti associati. Inoltre, i report master/dettagli sono utili per visualizzare informazioni dettagliate da tabelle in particolare "wide" (quelle con numerose colonne). Ad esempio, il livello "Master" di un report master/dettagli può mostrare solo il nome del prodotto e il prezzo unitario dei prodotti nel database e il drill-down in un prodotto specifico indicherà i campi di prodotto aggiuntivi (categoria, fornitore, quantità per unità e così via).

È possibile implementare un report master/detail in molti modi. In questa e nelle tre esercitazioni successive verranno esaminati diversi report master/dettagli. In questa esercitazione verrà illustrato come visualizzare i record master in un [controllo DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) e i dettagli dell'elemento di elenco selezionato in GridView. In particolare, in questa esercitazione viene elencata la categoria e le informazioni sul prodotto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 1: visualizzazione delle categorie in un oggetto DropDownList

Il report master/dettagli elenca le categorie in un oggetto DropDownList, con i prodotti dell'elemento di elenco selezionati visualizzati più in basso nella pagina in un controllo GridView. La prima attività, quindi, consiste nel fare in modo che le categorie vengano visualizzate in un oggetto DropDownList. Aprire la pagina `FilterByDropDownList.aspx` nella cartella `Filtering`, trascinare un oggetto DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina e impostare la relativa proprietà `ID` su `Categories`. Fare quindi clic sul collegamento Scegli origine dati dallo smart tag di DropDownList. Verrà visualizzata la configurazione guidata origine dati.

[![specificare l'origine dati dell'oggetto DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Figura 1**: specificare l'origine dati dell'oggetto DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))

Scegliere di aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` che richiama il metodo `GetCategories()` della classe `CategoriesBLL`.

[![aggiungere un nuovo ObjectDataSource denominato CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))

[![scegliere di utilizzare la classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Figura 3**: scegliere di usare la classe `CategoriesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))

[![configurare ObjectDataSource per l'utilizzo del metodo GetCategories ()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per l'uso del metodo `GetCategories()` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))

Dopo la configurazione di ObjectDataSource, è ancora necessario specificare il campo origine dati da visualizzare in DropDownList e quale deve essere associato come valore per l'elemento elenco. Fare in modo che il campo `CategoryName` sia visualizzato e `CategoryID` come valore per ogni elemento dell'elenco.

[![fare in modo che DropDownList visualizzi il campo CategoryName e usare CategoryID come valore](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Figura 5**: fare in modo che l'oggetto DropDownList visualizzi il campo `CategoryName` e usare `CategoryID` come valore ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))

A questo punto è presente un controllo DropDownList popolato con i record della tabella `Categories` (tutti eseguiti in circa sei secondi). La figura 6 Mostra lo stato di avanzamento fino a questo punto quando viene visualizzato tramite un browser.

[![elenco A discesa elenca le categorie correnti](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Figura 6**: nell'elenco a discesa sono elencate le categorie correnti ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Passaggio 2: aggiunta dei prodotti GridView

L'ultimo passaggio del report master/dettagli consiste nell'elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un controllo GridView alla pagina e creare un nuovo ObjectDataSource denominato `productsDataSource`. Fare in modo che il controllo `productsDataSource` Abbatti i dati dal metodo `GetProductsByCategoryID(categoryID)` della classe `ProductsBLL`.

[![selezionare il metodo GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Figura 7**: selezionare il metodo `GetProductsByCategoryID(categoryID)` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))

Dopo aver scelto questo metodo, la procedura guidata di ObjectDataSource richiede il valore per il parametro *`categoryID`* del metodo. Per utilizzare il valore dell'elemento `categories` DropDownList selezionato, impostare l'origine del parametro su Control e il ControlID su `Categories`.

[![impostare il parametro categoryID sul valore del DropDownList categorie](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Figura 8**: impostare il parametro *`categoryID`* sul valore dell'oggetto DropDownList `Categories` ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))

Esaminare lo stato di avanzamento in un browser. Quando si visita la pagina, vengono visualizzati i prodotti appartenenti alla categoria selezionata (bevande), come illustrato nella figura 9, ma la modifica del DropDownList non aggiorna i dati. Questo perché è necessario che venga eseguito un postback per l'aggiornamento di GridView. A tale scopo, sono disponibili due opzioni (nessuna delle quali richiede la scrittura di codice):

- **Impostare la**[proprietà AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)di DropDownList categorie**su true.** Per eseguire questa operazione, selezionare l'opzione Abilita AutoPostBack nello smart tag di DropDownList. Verrà attivato un postback ogni volta che l'utente seleziona l'elemento dell'oggetto DropDownList modificato. Pertanto, quando l'utente seleziona una nuova categoria da DropDownList, sarà seguito da un postback e il controllo GridView verrà aggiornato con i prodotti per la categoria appena selezionata. (Questo è l'approccio usato in questa esercitazione).
- **Aggiungere un controllo Web Button accanto al controllo DropDownList.** Impostarne la proprietà `Text` su Refresh o un valore simile. Con questo approccio, l'utente dovrà selezionare una nuova categoria e quindi fare clic sul pulsante. Se si fa clic sul pulsante, viene generato un postback e viene aggiornato GridView per elencare i prodotti della categoria selezionata.

Le figure 9 e 10 illustrano il report master/dettagli in azione.

[![quando si visita la pagina per la prima volta, vengono visualizzati i prodotti Beverage](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Figura 9**: la prima volta che si visita la pagina, vengono visualizzati i prodotti Beverage ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))

[![la selezione di un nuovo prodotto (produzione) causa automaticamente un PostBack, aggiornando GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Figura 10**: la selezione di un nuovo prodotto (produzione) causa automaticamente un postback, aggiornando GridView ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))

## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di una voce di elenco "--Choose a Category--"

Per la prima volta che si visita la pagina `FilterByDropDownList.aspx`, per impostazione predefinita viene selezionata la prima voce di elenco (bevande) del controllo DropDownList delle categorie, che mostra i prodotti Beverage in GridView. Invece di visualizzare i prodotti della prima categoria, potrebbe essere necessario selezionare un elemento DropDownList con una voce simile a "--Choose a Category--".

Per aggiungere un nuovo elemento elenco al DropDownList, passare al Finestra Proprietà e fare clic sui puntini di sospensione nella proprietà `Items`. Aggiungere una nuova voce di elenco con la `Text` "--scegliere una categoria--" e il `-1``Value`.

[![Aggiungi a--scegliere una categoria--elemento elenco](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Figura 11**: aggiungere una--scegliere una categoria--elemento elenco ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))

In alternativa, è possibile aggiungere l'elemento dell'elenco aggiungendo il markup seguente al DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Inoltre, è necessario impostare la `AppendDataBoundItems` del controllo DropDownList su true perché quando le categorie sono associate a DropDownList da ObjectDataSource, sovrascriveranno gli elementi dell'elenco aggiunti manualmente se `AppendDataBoundItems` non è true.

![Impostare la proprietà AppendDataBoundItems su true](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Figura 12**: impostare la proprietà `AppendDataBoundItems` su true

Dopo queste modifiche, quando si visita la pagina "--scegliere una categoria--" è selezionata e non viene visualizzato alcun prodotto.

[![al caricamento iniziale della pagina non vengono visualizzati prodotti](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Figura 13**: nel caricamento iniziale della pagina non viene visualizzato alcun prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))

Il motivo per cui non viene visualizzato alcun prodotto quando è selezionato l'elemento dell'elenco "--Choose a Category--" è perché il relativo valore è `-1` e non sono presenti prodotti nel database con una `CategoryID` di `-1`. Se questo è il comportamento desiderato, è possibile procedere in questo momento. Se tuttavia si desidera visualizzare *tutte* le categorie quando viene selezionata l'opzione "--Choose a Category--", tornare alla classe `ProductsBLL` e personalizzare il `GetProductsByCategoryID(categoryID)` metodo in modo che richiami il metodo `GetProducts()` se il parametro passato nel *`categoryID`* è minore di zero:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

La tecnica usata in questo esempio è simile all'approccio usato per visualizzare tutti i fornitori nell'esercitazione [sui parametri dichiarativi](../basic-reporting/declarative-parameters-cs.md) , sebbene in questo esempio venga usato il valore `-1` per indicare che tutti i record devono essere recuperati anziché `null`. Questo perché il parametro *`categoryID`* del metodo `GetProductsByCategoryID(categoryID)` prevede che venga passato un valore integer, mentre nell'esercitazione sui parametri dichiarativi è stato passato un parametro di input di stringa.

Nella figura 14 viene illustrata una schermata di `FilterByDropDownList.aspx` quando è selezionata l'opzione "--Scegli una categoria--". In questo caso, tutti i prodotti vengono visualizzati per impostazione predefinita e l'utente può restringere lo schermo scegliendo una categoria specifica.

[![tutti i prodotti sono ora elencati per impostazione predefinita](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Figura 14**: tutti i prodotti sono ora elencati per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni complete](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))

## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati gerarchicamente, spesso è utile presentare i dati utilizzando i report master/dettagli, da cui l'utente può iniziare a esaminare i dati dalla parte superiore della gerarchia ed eseguire il drill-down nei dettagli. In questa esercitazione è stata esaminata la creazione di un semplice report master/dettagli che mostra i prodotti di una categoria selezionata. Questa operazione è stata eseguita utilizzando un oggetto DropDownList per l'elenco di categorie e un GridView per i prodotti appartenenti alla categoria selezionata.

Nell' [esercitazione successiva](master-detail-filtering-with-two-dropdownlists-cs.md) l'interfaccia DropDownList verrà rilevata un passo avanti, usando due DropDownList.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](master-detail-filtering-with-two-dropdownlists-cs.md)
