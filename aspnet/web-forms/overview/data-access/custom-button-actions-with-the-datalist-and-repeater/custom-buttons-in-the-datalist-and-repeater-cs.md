---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Pulsanti personalizzati in DataList e Repeater (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà compilata un'interfaccia che usa un Repeater per elencare le categorie nel sistema, con ogni categoria che fornisce un pulsante per visualizzare il relativo associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: e8cb1054068327c25e057b6df1cc7506feec8d37
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576391"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Pulsanti personalizzati con DataList e Repeater (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) o [scaricare il file PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> In questa esercitazione verrà compilata un'interfaccia che usa un Repeater per elencare le categorie nel sistema, con ogni categoria che fornisce un pulsante per mostrare i prodotti associati usando un controllo BulletedList.

## <a name="introduction"></a>Introduzione

Negli ultimi diciassette esercitazioni di DataList e Repeater sono stati creati esempi di sola lettura e la modifica e l'eliminazione degli esempi. Per semplificare la modifica e l'eliminazione delle funzionalità all'interno di un DataList, sono stati aggiunti pulsanti al `ItemTemplate` di DataList che, quando si fa clic, causavano un postback e generava un evento DataList corrispondente alla proprietà `CommandName` dei pulsanti. Se ad esempio si aggiunge un pulsante al `ItemTemplate` con un valore `CommandName` proprietà modifica, il `EditCommand` di DataList viene attivato durante il postback; una con l'eliminazione `CommandName` genera l'`DeleteCommand`.

Oltre ai pulsanti modifica ed Elimina, i controlli DataList e Repeater possono includere anche pulsanti, LinkButton o ImageButton che, quando vengono selezionate, eseguono una logica personalizzata sul lato server. In questa esercitazione verrà compilata un'interfaccia che usa un Repeater per elencare le categorie nel sistema. Per ogni categoria, il ripetitore includerà un pulsante per mostrare i prodotti associati alla categoria usando un controllo BulletedList (vedere la figura 1).

[![facendo clic sul collegamento Mostra prodotti vengono visualizzati i prodotti della categoria in un elenco puntato](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: facendo clic sul collegamento Mostra prodotti vengono visualizzati i prodotti della categoria in un elenco puntato ([fare clic per visualizzare l'immagine con dimensioni complete](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Passaggio 1: aggiungere le pagine Web dell'esercitazione sui pulsanti personalizzati

Prima di esaminare come aggiungere un pulsante personalizzato, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `CustomButtonsDataListRepeater`. Aggiungere quindi le due pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni correlate ai pulsanti personalizzati](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni correlate ai pulsanti personalizzati

Analogamente alle altre cartelle, `Default.aspx` nella cartella `CustomButtonsDataListRepeater` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))

Aggiungere infine le pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo il paging e l'ordinamento con il `<siteMapNode>`DataList e Repeater:

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica, inserimento ed eliminazione.

![La mappa del sito include ora la voce per l'esercitazione sui pulsanti personalizzati](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: la mappa del sito include ora la voce per l'esercitazione sui pulsanti personalizzati

## <a name="step-2-adding-the-list-of-categories"></a>Passaggio 2: aggiunta dell'elenco di categorie

Per questa esercitazione è necessario creare un ripetitore in cui sono elencate tutte le categorie insieme a un LinkButton Mostra prodotti che, quando si fa clic, Visualizza i prodotti associati di categoria in un elenco puntato. Per prima cosa, creiamo un semplice ripetitore che elenca le categorie nel sistema. Per iniziare, aprire la pagina `CustomButtons.aspx` nella cartella `CustomButtonsDataListRepeater`. Trascinare un Repeater dalla casella degli strumenti nella finestra di progettazione e impostare la relativa proprietà `ID` su `Categories`. Successivamente, creare un nuovo controllo origine dati dallo smart tag Repeater s. In particolare, creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` che seleziona i dati dal metodo `GetCategories()` della classe `CategoriesBLL`.

[![configurare ObjectDataSource per l'uso del metodo CategoriesBLL della classe s GetCategories ()](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per l'uso del metodo `CategoriesBLL` Class s `GetCategories()` ([fare clic per visualizzare l'immagine con dimensioni complete](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))

A differenza del controllo DataList, per cui Visual Studio crea una `ItemTemplate` predefinita basata sull'origine dati, è necessario definire manualmente i modelli del ripetitore. Inoltre, i modelli del ripetitore devono essere creati e modificati in modo dichiarativo, ovvero non è disponibile alcuna opzione modifica modelli nello smart tag Repeater.

Fare clic sulla scheda origine nell'angolo in basso a sinistra e aggiungere un `ItemTemplate` che Visualizza il nome della categoria in un elemento `<h3>` e la relativa descrizione in un tag di paragrafo. includere un `SeparatorTemplate` che visualizza una regola orizzontale (`<hr />`) tra le singole categorie. Aggiungere inoltre un LinkButton con la relativa proprietà `Text` impostata per mostrare i prodotti. Dopo aver completato questi passaggi, il markup dichiarativo della pagina deve essere simile al seguente:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

La figura 6 Mostra la pagina quando viene visualizzata tramite un browser. Viene elencato il nome e la descrizione di ogni categoria. Quando si fa clic sul pulsante Mostra prodotti, viene generato un postback, ma non viene ancora eseguita alcuna azione.

[![viene visualizzato il nome e la descrizione di ogni categoria, insieme a un LinkButton Mostra i prodotti](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: viene visualizzato il nome e la descrizione di ogni categoria, insieme a un LinkButton Mostra i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Passaggio 3: esecuzione della logica sul lato server quando si fa clic su Visualizza prodotti LinkButton

Quando si fa clic su un pulsante, un LinkButton o un ImageButton all'interno di un modello in un oggetto DataList o Repeater, viene eseguito un postback e viene generato l'evento DataList o Repeater `ItemCommand`. Oltre all'evento `ItemCommand`, il controllo DataList può generare anche un altro evento più specifico se la proprietà `CommandName` del pulsante è impostata su una delle stringhe riservate (Delete, Edit, Cancel, Update o Select), ma l'evento `ItemCommand` viene *sempre* attivato.

Quando si fa clic su un pulsante all'interno di un oggetto DataList o Repeater, spesso è necessario passare il pulsante che è stato selezionato (nel caso in cui siano presenti più pulsanti all'interno del controllo, ad esempio un pulsante modifica ed Elimina) ed eventualmente alcune informazioni aggiuntive (ad esempio valore della chiave primaria dell'elemento il cui pulsante è stato selezionato. Il pulsante, LinkButton e ImageButton forniscono due proprietà i cui valori vengono passati al gestore dell'evento `ItemCommand`:

- `CommandName` una stringa utilizzata in genere per identificare ogni pulsante nel modello
- `CommandArgument` comunemente utilizzato per memorizzare il valore di un campo dati, ad esempio il valore della chiave primaria

Per questo esempio, impostare la proprietà LinkButton s `CommandName` su ShowProducts e associare il valore della chiave primaria del record corrente `CategoryID` alla proprietà `CommandArgument` usando la sintassi DataBinding `CategoryArgument='<%# Eval("CategoryID") %>'`. Dopo aver specificato queste due proprietà, la sintassi dichiarativa di LinkButton sarà simile alla seguente:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Quando si fa clic sul pulsante, viene eseguito un postback e viene generato l'evento DataList o Repeater `ItemCommand`. Al gestore eventi vengono passati i valori `CommandName` e `CommandArgument` dei pulsanti.

Creare un gestore eventi per l'evento Repeater s `ItemCommand` e prendere nota del secondo parametro passato nel gestore eventi (denominato `e`). Questo secondo parametro è di tipo [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e dispone delle quattro proprietà seguenti:

- `CommandArgument` il valore della proprietà `CommandArgument` del pulsante selezionato
- `CommandName` il valore della proprietà `CommandName` del pulsante
- `CommandSource` un riferimento al controllo Button selezionato
- `Item` un riferimento al [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) contenente il pulsante su cui è stato fatto clic; ogni record associato al ripetitore viene manifestato come `RepeaterItem`

Poiché la `CategoryID` della categoria selezionata viene passata tramite la proprietà `CommandArgument`, è possibile ottenere il set di prodotti associati alla categoria selezionata nel gestore dell'evento `ItemCommand`. Questi prodotti possono quindi essere associati a un controllo BulletedList nel `ItemTemplate` (che è ancora necessario aggiungere). Tutto ciò che rimane, quindi, consiste nell'aggiungere l'oggetto BulletedList, farvi riferimento nel gestore dell'evento `ItemCommand` ed eseguire il binding al set di prodotti per la categoria selezionata, che verrà affrontata nel passaggio 4.

> [!NOTE]
> Al gestore dell'evento DataList s `ItemCommand` viene passato un oggetto di tipo [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), che offre le stesse quattro proprietà della classe `RepeaterCommandEventArgs`.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Passaggio 4: visualizzazione dei prodotti selezionati della categoria in un elenco puntato

È possibile visualizzare i prodotti della categoria selezionati all'interno del `ItemTemplate` Repeater utilizzando un numero qualsiasi di controlli. È possibile aggiungere un altro Repeater annidato, un DataList, un oggetto DropDownList, un GridView e così via. Poiché si desidera visualizzare i prodotti come elenco puntato, tuttavia, verrà utilizzato il controllo BulletedList. Tornando al markup dichiarativo della pagina `CustomButtons.aspx`, aggiungere un controllo BulletedList al `ItemTemplate` dopo l'oggetto LinkButton Mostra prodotti. Impostare la `ID` di BulletedLists su `ProductsInCategory`. In BulletedList viene visualizzato il valore del campo dati specificato tramite la proprietà `DataTextField`; Poiché a questo controllo vengono associati informazioni sul prodotto, impostare la proprietà `DataTextField` su `ProductName`.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

Nel gestore dell'evento `ItemCommand` fare riferimento a questo controllo utilizzando `e.Item.FindControl("ProductsInCategory")` e associarlo al set di prodotti associato alla categoria selezionata.

[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Prima di eseguire qualsiasi azione nel gestore dell'evento `ItemCommand`, è prudente controllare innanzitutto il valore del `CommandName`in ingresso. Poiché il gestore dell'evento `ItemCommand` viene attivato quando si fa clic su *un* pulsante, se nel modello sono presenti più pulsanti, utilizzare il valore `CommandName` per discernere l'azione da eseguire. Il controllo della `CommandName` qui è discutibile, perché è presente un solo pulsante, ma è una scelta ideale da creare. Successivamente, il `CategoryID` della categoria selezionata viene recuperato dalla proprietà `CommandArgument`. Viene quindi fatto riferimento al controllo BulletedList nel modello e associato ai risultati del metodo `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`.

Nelle esercitazioni precedenti che usavano i pulsanti all'interno di un DataList, ad esempio [una panoramica della modifica e dell'eliminazione dei dati in DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), il valore della chiave primaria di un determinato elemento veniva determinato tramite la raccolta di `DataKeys`. Sebbene questo approccio funzioni correttamente con DataList, il ripetitore non dispone di una proprietà `DataKeys`. Al contrario, è necessario usare un approccio alternativo per fornire il valore della chiave primaria, ad esempio tramite il pulsante s `CommandArgument` proprietà o assegnando il valore della chiave primaria a un controllo Web etichetta nascosta all'interno del modello e rileggendone il valore nel gestore eventi `ItemCommand` utilizzando `e.Item.FindControl("LabelID")`.

Dopo aver completato il gestore dell'evento `ItemCommand`, provare a eseguire il test di questa pagina in un browser. Come illustrato nella figura 7, facendo clic sul collegamento Mostra prodotti viene generato un postback e vengono visualizzati i prodotti per la categoria selezionata in un oggetto BulletedList. Si noti inoltre che le informazioni sul prodotto restano, anche se le altre categorie mostrano i collegamenti ai prodotti.

> [!NOTE]
> Se si desidera modificare il comportamento di questo report, in modo che gli unici prodotti di una categoria siano elencati alla volta, è sufficiente impostare la proprietà `EnableViewState` del controllo BulletedList su `False`.

[![viene utilizzato un BulletedList per visualizzare i prodotti della categoria selezionata](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: viene usato un oggetto BulletedList per visualizzare i prodotti della categoria selezionata ([fare clic per visualizzare l'immagine con dimensioni complete](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))

## <a name="summary"></a>Riepilogo

I controlli DataList e Repeater possono includere un numero qualsiasi di pulsanti, LinkButton o ImageButton all'interno dei modelli. Tali pulsanti, quando si fa clic, generano un postback e generano l'evento `ItemCommand`. Per associare un'azione personalizzata lato server a un pulsante selezionato, creare un gestore eventi per l'evento `ItemCommand`. In questo gestore eventi controllare innanzitutto il valore `CommandName` in arrivo per determinare il pulsante selezionato. Facoltativamente, è possibile fornire informazioni aggiuntive tramite il pulsante s `CommandArgument` proprietà.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Dennis Patterson. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](custom-buttons-in-the-datalist-and-repeater-vb.md)
