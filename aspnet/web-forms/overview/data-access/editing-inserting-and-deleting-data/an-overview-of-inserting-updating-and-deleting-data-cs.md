---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Panoramica di inserimento, aggiornamento ed eliminazione di dati (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come eseguire il mapping dei metodi Insert (), Update () e Delete () di ObjectDataSource ai metodi delle classi BLL, nonché come configurare...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e26b8e841f86272a158b0c09b62ab2790d01d191
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592743"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Panoramica di inserimento, aggiornamento ed eliminazione di dati (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) o [scaricare il file PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> In questa esercitazione verrà illustrato come eseguire il mapping dei metodi Insert (), Update () e Delete () di ObjectDataSource ai metodi delle classi BLL, nonché come configurare i controlli GridView, DetailsView e FormView per fornire funzionalità di modifica dei dati.

## <a name="introduction"></a>Introduzione

Nelle esercitazioni precedenti sono state esaminate le procedure per visualizzare i dati in una pagina ASP.NET usando i controlli GridView, DetailsView e FormView. Questi controlli funzionano semplicemente con i dati forniti. In genere, questi controlli accedono ai dati tramite l'uso di un controllo origine dati, ad esempio ObjectDataSource. Abbiamo visto come ObjectDataSource funge da proxy tra la pagina ASP.NET e i dati sottostanti. Quando un controllo GridView deve visualizzare i dati, richiama il metodo di `Select()` di ObjectDataSource, che a sua volta richiama un metodo dal livello di logica di business (BLL), che chiama un metodo nell'oggetto TableAdapter del livello di accesso ai dati (DAL) appropriato, che a sua volta invia una query di `SELECT` al database Northwind.

Tenere presente che, durante la creazione degli oggetti TableAdapter nella [prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), Visual Studio ha aggiunto automaticamente i metodi per l'inserimento, l'aggiornamento e l'eliminazione dei dati dalla tabella di database sottostante. Inoltre, durante la [creazione di un livello di logica di business](../introduction/creating-a-business-logic-layer-cs.md) sono stati progettati metodi in BLL che hanno chiamato i metodi di modifica dei dati dal.

Oltre al metodo `Select()`, ObjectDataSource dispone anche di metodi `Insert()`, `Update()`e `Delete()`. Analogamente al metodo `Select()`, è possibile eseguire il mapping di questi tre metodi ai metodi di un oggetto sottostante. Quando è configurato per inserire, aggiornare o eliminare dati, i controlli GridView, DetailsView e FormView forniscono un'interfaccia utente per la modifica dei dati sottostanti. Questa interfaccia utente chiama i metodi `Insert()`, `Update()`e `Delete()` di ObjectDataSource, che quindi richiamano i metodi associati dell'oggetto sottostante (vedere la figura 1).

[![i metodi Insert (), Update () e Delete () di ObjectDataSource vengono usati come proxy nel BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: i metodi di `Insert()`, `Update()`e `Delete()` di ObjectDataSource vengono usati come proxy nel BLL ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))

In questa esercitazione verrà illustrato come eseguire il mapping dei metodi di `Insert()`, `Update()`e `Delete()` di ObjectDataSource ai metodi delle classi in BLL, nonché come configurare i controlli GridView, DetailsView e FormView per fornire funzionalità di modifica dei dati.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Passaggio 1: creazione delle pagine Web delle esercitazioni di inserimento, aggiornamento ed eliminazione

Prima di iniziare a esplorare come inserire, aggiornare ed eliminare i dati, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le versioni successive. Per iniziare, aggiungere una nuova cartella denominata `EditInsertDelete`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica dei dati](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative alla modifica dei dati

Analogamente alle altre cartelle, `Default.aspx` nella cartella `EditInsertDelete` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))

Aggiungere infine le pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo il `<siteMapNode>`di formattazione personalizzato:

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica, inserimento ed eliminazione.

![La mappa del sito include ora le voci per le esercitazioni di modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: la mappa del sito include ora le voci per le esercitazioni di modifica, inserimento ed eliminazione

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Passaggio 2: aggiunta e configurazione del controllo ObjectDataSource

Poiché GridView, DetailsView e FormView si differenziano per le funzionalità di modifica dei dati e il layout, esaminiamo singolarmente ognuno di essi. Anziché fare in modo che ogni controllo usi il proprio ObjectDataSource, tuttavia, creiamo un solo ObjectDataSource che tutti e tre gli esempi di controllo possono condividere.

Aprire la pagina `Basics.aspx`, trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione, quindi fare clic sul collegamento Configura origine dati dallo smart tag. Poiché il `ProductsBLL` è l'unica classe BLL che fornisce i metodi di modifica, inserimento ed eliminazione, configurare ObjectDataSource per l'utilizzo di questa classe.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per l'uso della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))

Nella schermata successiva è possibile specificare i metodi della classe `ProductsBLL` mappati ai `Select()`, `Insert()`, `Update()`e `Delete()` di ObjectDataSource selezionando la scheda appropriata e scegliendo il metodo dall'elenco a discesa. Figura 6, che dovrebbe avere un aspetto familiare da ora, esegue il mapping del metodo `Select()` di ObjectDataSource al metodo `GetProducts()` della classe `ProductsBLL`. È possibile configurare i metodi `Insert()`, `Update()`e `Delete()` selezionando la scheda appropriata dall'elenco lungo la parte superiore.

[![che ObjectDataSource restituisca tutti i prodotti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: fare in modo che ObjectDataSource restituisca tutti i prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))

Figure 7, 8 e 9 mostrano le schede di aggiornamento, inserimento ed eliminazione di ObjectDataSource. Configurare queste schede in modo che i metodi `Insert()`, `Update()`e `Delete()` richiamino rispettivamente i metodi `UpdateProduct`, `AddProduct`e `DeleteProduct` della classe `ProductsBLL`.

[![mappare il metodo Update () di ObjectDataSource al metodo UpdateProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: eseguire il mapping del metodo di `Update()` di ObjectDataSource al metodo `UpdateProduct` della classe `ProductBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))

[![mappare il metodo Insert () di ObjectDataSource al metodo AddProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: eseguire il mapping del metodo di `Insert()` di ObjectDataSource al metodo Add `Product` della classe `ProductBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))

[![mappare il metodo Delete () di ObjectDataSource al metodo DeleteProduct della classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: eseguire il mapping del metodo di `Delete()` di ObjectDataSource al metodo `DeleteProduct` della classe `ProductBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))

È possibile che si sia notato che questi metodi sono già stati selezionati negli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione. Questo è grazie all'uso del `DataObjectMethodAttribute` che decora i metodi di `ProductsBLL`. Il metodo DeleteProduct, ad esempio, ha la firma seguente:

[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

L'attributo `DataObjectMethodAttribute` indica lo scopo di ogni metodo se è per la selezione, l'inserimento, l'aggiornamento o l'eliminazione e se è il valore predefinito. Se questi attributi sono stati omessi quando si creano le classi BLL, sarà necessario selezionare manualmente i metodi dalle schede Aggiorna, Inserisci ed Elimina.

Dopo aver verificato che sia stato eseguito il mapping dei metodi di `ProductsBLL` appropriati ai metodi `Insert()`, `Update()`e `Delete()` di ObjectDataSource, fare clic su fine per completare la procedura guidata.

## <a name="examining-the-objectdatasources-markup"></a>Esame del markup di ObjectDataSource

Dopo aver configurato ObjectDataSource tramite la procedura guidata, passare alla visualizzazione origine per esaminare il markup dichiarativo generato. Il tag `<asp:ObjectDataSource>` specifica l'oggetto sottostante e i metodi da richiamare. Sono inoltre disponibili `DeleteParameters`, `UpdateParameters`e `InsertParameters` che eseguono il mapping ai parametri di input per i metodi `AddProduct`, `UpdateProduct`e `DeleteProduct` della classe `ProductsBLL`:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource include un parametro per ognuno dei parametri di input per i metodi associati, analogamente a un elenco di `SelectParameter` s è presente quando ObjectDataSource è configurato per chiamare un metodo Select che prevede un parametro di input, ad esempio `GetProductsByCategoryID(categoryID)`. Come si vedrà a breve, i valori per questi `DeleteParameters`, `UpdateParameters`e `InsertParameters` vengono impostati automaticamente da GridView, DetailsView e FormView prima di richiamare il metodo di `Insert()`, `Update()`o `Delete()` di ObjectDataSource. Questi valori possono essere impostati anche a livello di codice in base alle esigenze, come verrà illustrato in un'esercitazione futura.

Un effetto collaterale dell'utilizzo della procedura guidata per la configurazione di ObjectDataSource è che Visual Studio imposta la [proprietà OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) su `original_{0}`. Il valore di questa proprietà viene utilizzato per includere i valori originali dei dati modificati ed è utile in due scenari:

- Se, durante la modifica di un record, gli utenti sono in grado di modificare il valore della chiave primaria. In questo caso, è necessario specificare sia il nuovo valore di chiave primaria che quello originale della chiave primaria, in modo che sia possibile trovare il record con il valore di chiave primaria originale e che il relativo valore sia aggiornato di conseguenza.
- Quando si usa la concorrenza ottimistica. La concorrenza ottimistica è una tecnica per garantire che due utenti simultanei non sovrascrivano le modifiche di un altro e che sia l'argomento per un'esercitazione futura.

La proprietà `OldValuesParameterFormatString` indica il nome dei parametri di input nei metodi Update e Delete dell'oggetto sottostante per i valori originali. Questa proprietà e il suo scopo verranno illustrati in modo più dettagliato quando si Esplora la concorrenza ottimistica. Ora, tuttavia, poiché i metodi del BLL non prevedono i valori originali ed è quindi importante che questa proprietà venga rimossa. Se si lascia la proprietà `OldValuesParameterFormatString` impostata su un valore diverso da quello predefinito (`{0}`), si verificherà un errore quando un controllo Web dei dati tenta di richiamare i metodi di `Update()` o `Delete()` di ObjectDataSource perché ObjectDataSource tenterà di passare sia il `UpdateParameters` sia il `DeleteParameters` specificato, nonché i parametri del valore originale.

Se questo non è molto chiaro in questo frangente, si esamineranno questa proprietà e la relativa utilità in un'esercitazione futura. Per il momento, è sufficiente assicurarsi di rimuovere questa dichiarazione di proprietà interamente dalla sintassi dichiarativa o di impostare il valore predefinito ({0}).

> [!NOTE]
> Se si deseleziona semplicemente il valore della proprietà `OldValuesParameterFormatString` dal Finestra Proprietà nella visualizzazione progettazione, la proprietà continuerà a esistere nella sintassi dichiarativa, ma sarà impostata su una stringa vuota. Questo, sfortunatamente, provocherà comunque lo stesso problema descritto in precedenza. Pertanto, rimuovere completamente la proprietà dalla sintassi dichiarativa oppure, dal Finestra Proprietà, impostare il valore predefinito, `{0}`.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Passaggio 3: aggiunta di un controllo Web dati e configurazione per la modifica dei dati

Una volta che ObjectDataSource è stato aggiunto alla pagina e configurato, è possibile aggiungere i controlli Web di dati alla pagina per visualizzare i dati e fornire agli utenti finali la possibilità di modificarli. I controlli GridView, DetailsView e FormView verranno esaminati separatamente, poiché questi controlli Web dei dati differiscono in base alle funzionalità di modifica dei dati e alla configurazione.

Come si vedrà nella parte restante di questo articolo, l'aggiunta del supporto di modifica, inserimento ed eliminazione di base tramite i controlli GridView, DetailsView e FormView è molto semplice quanto la verifica di un paio di caselle di controllo. Nel mondo reale sono presenti molte sottigliezze e casi limite che rendono più complessa la fornitura di funzionalità di questo tipo. Questa esercitazione, tuttavia, si concentra esclusivamente sulla dimostrazione di semplici funzionalità di modifica dei dati. Nelle esercitazioni future vengono esaminati i problemi che indubbiamente si verificano in un'impostazione del mondo reale.

## <a name="deleting-data-from-the-gridview"></a>Eliminazione di dati da GridView

Per iniziare, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Quindi, associare ObjectDataSource a GridView selezionandola dall'elenco a discesa nello smart tag di GridView. A questo punto, il markup dichiarativo di GridView sarà:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

L'associazione di GridView a ObjectDataSource tramite il relativo smart tag offre due vantaggi:

- I BoundField e CheckBoxFields vengono creati automaticamente per ogni campo restituito da ObjectDataSource. Inoltre, le proprietà BoundField e CheckBoxField vengono impostate in base ai metadati del campo sottostante. Ad esempio, i campi `ProductID`, `CategoryName`e `SupplierName` sono contrassegnati come di sola lettura nel `ProductsDataTable` e pertanto non devono essere aggiornabili durante la modifica. Per risolvere questo problema, queste proprietà di sola [lettura](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) dei BoundField sono impostate su `true`.
- La [proprietà al DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) viene assegnata ai campi di chiave primaria dell'oggetto sottostante. Questo è essenziale quando si usa GridView per la modifica o l'eliminazione di dati, in quanto questa proprietà indica il campo (o set di campi) che identifica in modo univoco ogni record. Per altre informazioni sulla proprietà `DataKeyNames`, fare riferimento al [master o al dettaglio usando un controllo GridView Master selezionabile con un'esercitazione relativa a controllo DetailView per dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

Mentre GridView può essere associato a ObjectDataSource attraverso la Finestra Proprietà o la sintassi dichiarativa, in questo modo è necessario aggiungere manualmente il BoundField e `DataKeyNames` markup appropriati.

Il controllo GridView fornisce supporto incorporato per la modifica e l'eliminazione a livello di riga. La configurazione di GridView per supportare l'eliminazione consente di aggiungere una colonna di pulsanti Delete. Quando l'utente finale fa clic sul pulsante Elimina per una determinata riga, si verifica un postback e GridView esegue i passaggi seguenti:

1. I valori `DeleteParameters` di ObjectDataSource sono assegnati
2. Viene richiamato il metodo di `Delete()` di ObjectDataSource, che elimina il record specificato
3. GridView viene riassociato a ObjectDataSource richiamando il relativo metodo `Select()`

I valori assegnati all'`DeleteParameters` sono i valori dei campi `DataKeyNames` per la riga di cui è stato fatto clic sul pulsante Elimina. È pertanto fondamentale impostare correttamente la proprietà `DataKeyNames` di GridView. Se non è presente, all'`DeleteParameters` verrà assegnato un valore `null` nel passaggio 1, che a sua volta non comporterà alcun record eliminato nel passaggio 2.

> [!NOTE]
> La raccolta di `DataKeys` viene archiviata nello stato di controllo di GridView, ovvero i valori di `DataKeys` verranno memorizzati durante il postback anche se lo stato di visualizzazione di GridView s è stato disabilitato. Tuttavia, è molto importante che lo stato di visualizzazione rimanga abilitato per GridView che supportano la modifica o l'eliminazione (comportamento predefinito). Se si imposta la proprietà `EnableViewState` GridView s su `false`, il comportamento di modifica ed eliminazione funzionerà in modo corretto per un singolo utente, ma se sono presenti utenti simultanei che eliminano i dati, è possibile che questi utenti simultanei eliminino o modifichino i record non desiderati. Vedere il mio intervento sul Blog, [Avviso: problema di concorrenza con ASP.NET 2,0 GridView/DetailsView/formviews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx). per altre informazioni.

Questo stesso avviso si applica anche a DetailsView e FormView.

Per aggiungere funzionalità di eliminazione a GridView, è sufficiente passare al relativo smart tag e selezionare la casella di controllo Abilita eliminazione.

![Selezionare la casella di controllo Abilita eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: selezionare la casella di controllo Abilita eliminazione

Se si seleziona la casella di controllo Abilita eliminazione dallo smart tag, viene aggiunto un oggetto CommandField al controllo GridView. CommandField esegue il rendering di una colonna in GridView con i pulsanti per l'esecuzione di una o più delle attività seguenti: selezione di un record, modifica di un record ed eliminazione di un record. In precedenza, il CommandField è stato visto in azione con la selezione di record in [Master/Detail usando un'esercitazione GridView selezionabile con controllo DetailView per](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

CommandField contiene una serie di `ShowXButton` proprietà che indicano la serie di pulsanti visualizzati in CommandField. Selezionando la casella di controllo Abilita eliminazione di un oggetto CommandField il cui `ShowDeleteButton` proprietà è `true` è stato aggiunto alla raccolta Columns di GridView.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

A questo punto, è possibile ritenerlo o meno. l'aggiunta del supporto per l'eliminazione a GridView è stata eseguita. Come illustrato nella figura 11, quando si visita questa pagina tramite un browser è presente una colonna di pulsanti Delete.

[![CommandField aggiunge una colonna di pulsanti Delete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: CommandField aggiunge una colonna di pulsanti Delete ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))

Se questa esercitazione è stata compilata autonomamente, quando si esegue il test della pagina facendo clic sul pulsante Elimina verrà generata un'eccezione. Continuare la lettura per informazioni sul motivo per cui queste eccezioni sono state generate e su come risolverle.

> [!NOTE]
> Se si sta seguendo le istruzioni riportate in questa esercitazione, è già stato preso in considerazione questi problemi. Tuttavia, si consiglia di leggere i dettagli elencati di seguito per identificare i problemi che possono verificarsi e le soluzioni alternative appropriate.

Se, quando si tenta di eliminare un prodotto, viene generata un'eccezione il cui messaggio è simile a "ObjectDataSource" ObjectDataSource1 "non è in*grado di trovare un metodo non generico" DeleteProduct "con parametri: ProductID, original\_ProductID*", probabilmente si dimentica di rimuovere la proprietà `OldValuesParameterFormatString` da ObjectDataSource. Con la proprietà `OldValuesParameterFormatString` specificata, ObjectDataSource tenta di passare i parametri di input `productID` e `original_ProductID` al metodo `DeleteProduct`. `DeleteProduct`, tuttavia, accetta solo un singolo parametro di input, di conseguenza l'eccezione. Se si rimuove la proprietà `OldValuesParameterFormatString` o la si imposta su `{0}`, l'oggetto ObjectDataSource non tenterà di passare il parametro di input originale.

[![assicurarsi che la proprietà OldValuesParameterFormatString sia stata eliminata](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: assicurarsi che la proprietà `OldValuesParameterFormatString` sia stata cancellata ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))

Anche se è stata rimossa la proprietà `OldValuesParameterFormatString`, viene comunque generata un'eccezione quando si tenta di eliminare un prodotto con il messaggio: "*l'istruzione DELETE è in conflitto con il vincolo di riferimento ' FK\_Order\_Details\_Products '* ". Il database Northwind contiene un vincolo FOREIGN KEY tra il `Order Details` e la tabella `Products`, ovvero non è possibile eliminare un prodotto dal sistema se sono presenti uno o più record nella tabella `Order Details`. Poiché tutti i prodotti del database Northwind hanno almeno un record in `Order Details`, non è possibile eliminare prodotti fino a quando non vengono eliminati prima i record dei dettagli degli ordini associati al prodotto.

[![un vincolo di chiave esterna impedisce l'eliminazione di prodotti](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: un vincolo di chiave esterna impedisce l'eliminazione di prodotti ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))

Per questa esercitazione, è sufficiente eliminare tutti i record dalla tabella `Order Details`. In un'applicazione reale è necessario effettuare una delle operazioni seguenti:

- Disporre di un'altra schermata per gestire le informazioni sui dettagli dell'ordine
- Aumentare il metodo `DeleteProduct` per includere la logica per eliminare i dettagli dell'ordine del prodotto specificato
- Modificare la query SQL utilizzata dal TableAdapter per includere l'eliminazione dei dettagli dell'ordine del prodotto specificato

È sufficiente eliminare tutti i record dalla tabella `Order Details` per aggirare il vincolo FOREIGN KEY. Passare alla Esplora server in Visual Studio, fare clic con il pulsante destro del mouse sul nodo `NORTHWND.MDF` e scegliere nuova query. Quindi, nella finestra query eseguire l'istruzione SQL seguente: `DELETE FROM [Order Details]`

[![eliminare tutti i record dalla tabella Order Details](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: eliminare tutti i record dalla tabella `Order Details` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))

Dopo aver cancellato il `Order Details` tabella facendo clic sul pulsante Elimina, il prodotto verrà eliminato senza errori. Se facendo clic sul pulsante Elimina il prodotto non viene eliminato, assicurarsi che la proprietà `DataKeyNames` di GridView sia impostata sul campo chiave primaria (`ProductID`).

> [!NOTE]
> Quando si fa clic sul pulsante Elimina, viene seguito un postback e il record viene eliminato. Questa operazione può risultare pericolosa poiché è facile fare clic accidentalmente sul pulsante Elimina della riga sbagliata. In un'esercitazione futura verrà illustrato come aggiungere una conferma lato client durante l'eliminazione di un record.

## <a name="editing-data-with-the-gridview"></a>Modifica di dati con GridView

Oltre all'eliminazione, il controllo GridView fornisce anche il supporto predefinito per la modifica a livello di riga. La configurazione di un controllo GridView per supportare la modifica aggiunge una colonna di pulsanti di modifica. Dal punto di vista dell'utente finale, se si fa clic sul pulsante modifica di una riga, la riga diventa modificabile, trasformando le celle in caselle di testo contenenti i valori esistenti e sostituendo il pulsante modifica con i pulsanti Aggiorna e Annulla. Dopo aver apportato le modifiche desiderate, l'utente finale può fare clic sul pulsante Aggiorna per eseguire il commit delle modifiche o sul pulsante Annulla per ignorarle. In entrambi i casi, dopo aver fatto clic su Aggiorna o Annulla, il controllo GridView torna allo stato di pre-modifica.

Dal punto di vista dello sviluppatore della pagina, quando l'utente finale fa clic sul pulsante modifica per una determinata riga, si verifica un postback e GridView esegue i passaggi seguenti:

1. La proprietà `EditItemIndex` di GridView viene assegnata all'indice della riga di cui è stato fatto clic sul pulsante modifica
2. GridView viene riassociato a ObjectDataSource richiamando il relativo metodo `Select()`
3. Viene eseguito il rendering dell'indice di riga che corrisponde alla `EditItemIndex` in "modalità di modifica". In questa modalità, il pulsante modifica viene sostituito dai pulsanti Aggiorna e Annulla e dai BoundField il cui `ReadOnly` proprietà è false (impostazione predefinita) viene sottoposto a rendering come controlli Web TextBox le cui proprietà `Text` sono assegnate ai valori dei campi dati.

A questo punto, il markup viene restituito al browser, consentendo all'utente finale di apportare eventuali modifiche ai dati della riga. Quando l'utente fa clic sul pulsante Aggiorna, viene eseguito un postback e GridView esegue i passaggi seguenti:

1. Ai valori `UpdateParameters` di ObjectDataSource vengono assegnati i valori immessi dall'utente finale nell'interfaccia di modifica di GridView
2. Viene richiamato il metodo di `Update()` di ObjectDataSource, che aggiorna il record specificato
3. GridView viene riassociato a ObjectDataSource richiamando il relativo metodo `Select()`

I valori di chiave primaria assegnati al `UpdateParameters` nel passaggio 1 derivano dai valori specificati nella proprietà `DataKeyNames`, mentre i valori di chiave non primaria provengono dal testo dei controlli Web TextBox per la riga modificata. Come per l'eliminazione di, è fondamentale che la proprietà `DataKeyNames` di GridView sia impostata correttamente. In caso contrario, al valore della chiave primaria `UpdateParameters` verrà assegnato un valore `null` nel passaggio 1, che a sua volta non comporterà alcun record aggiornato nel passaggio 2.

La funzionalità di modifica può essere attivata semplicemente selezionando la casella di controllo Abilita modifica nello smart tag di GridView.

![Selezionare la casella di controllo Abilita modifica](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: selezionare la casella di controllo Abilita modifica

Se si seleziona la casella di controllo Abilita modifica, viene aggiunto un oggetto CommandField (se necessario) e viene impostata la relativa proprietà `ShowEditButton` su `true`. Come illustrato in precedenza, CommandField contiene una serie di `ShowXButton` proprietà che indicano quali serie di pulsanti sono visualizzati in CommandField. Selezionando la casella di controllo Abilita modifica, viene aggiunta la proprietà `ShowEditButton` all'oggetto CommandField esistente:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Questo è tutto per l'aggiunta del supporto per la modifica rudimentale. Come illustrato in Figure16, l'interfaccia di modifica è piuttosto rudimentale ogni BoundField la cui proprietà `ReadOnly` è impostata su `false` (impostazione predefinita) viene sottoposta a rendering come casella di testo. Sono inclusi i campi come `CategoryID` e `SupplierID`, ovvero chiavi per altre tabelle.

[![facendo clic sul pulsante di modifica di Chai viene visualizzata la riga in modalità di modifica](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: facendo clic sul pulsante di modifica di Chai, viene visualizzata la riga in modalità di modifica ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))

Oltre a chiedere agli utenti di modificare direttamente i valori di chiave esterna, l'interfaccia dell'interfaccia di modifica risulta mancante nei modi seguenti:

- Se l'utente immette un `CategoryID` o `SupplierID` che non esiste nel database, il `UPDATE` violerà un vincolo di chiave esterna, causando la generazione di un'eccezione.
- L'interfaccia di modifica non include alcuna convalida. Se non si specifica un valore obbligatorio, ad esempio `ProductName`, oppure si immette un valore stringa in cui è previsto un valore numerico, ad esempio immettendo "troppo!" nella casella di testo `UnitPrice`) verrà generata un'eccezione. Un'esercitazione futura analizzerà come aggiungere controlli di convalida all'interfaccia utente di modifica.
- Attualmente, *tutti* i campi dei prodotti che non sono di sola lettura devono essere inclusi in GridView. Se dovessimo rimuovere un campo da GridView, ad `UnitPrice`, quando si aggiornano i dati, GridView non imposterà il valore del `UpdateParameters` `UnitPrice`, che cambierebbe il `UnitPrice` del record del database in un valore di `NULL`. Analogamente, se un campo obbligatorio, ad esempio `ProductName`, viene rimosso da GridView, l'aggiornamento avrà esito negativo con la stessa eccezione "*Column ' ProductName ' non consente valori null*" menzionati in precedenza.
- La formattazione dell'interfaccia di modifica lascia molto da desiderare. Il `UnitPrice` viene visualizzato con quattro punti decimali. Idealmente, i valori `CategoryID` e `SupplierID` contengono DropDownList che elencano le categorie e i fornitori nel sistema.

Questi sono tutti gli svantaggi che saranno disponibili per il momento, ma verranno trattati nelle esercitazioni future.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserimento, modifica ed eliminazione di dati con DetailsView

Come illustrato nelle esercitazioni precedenti, il controllo DetailsView visualizza un record alla volta e, come GridView, consente la modifica e l'eliminazione del record attualmente visualizzato. L'esperienza dell'utente finale con la modifica e l'eliminazione di elementi da DetailsView e dal flusso di lavoro dal lato ASP.NET è identica a quella di GridView. Se DetailsView differisce da GridView, fornisce anche il supporto incorporato per l'inserimento.

Per illustrare le funzionalità di modifica dei dati di GridView, iniziare aggiungendo un oggetto DetailsView alla pagina `Basics.aspx` sopra il GridView esistente e associarlo all'oggetto ObjectDataSource esistente tramite lo smart tag di DetailsView. Cancellare quindi le proprietà `Height` e `Width` di DetailsView e selezionare l'opzione Abilita paging dallo smart tag. Per abilitare la modifica, l'inserimento e l'eliminazione del supporto, è sufficiente selezionare le caselle di controllo Abilita modifica, Abilita inserimento e Abilita eliminazione nello smart tag.

![Configurare DetailsView per supportare le operazioni di modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurare DetailsView per supportare le operazioni di modifica, inserimento ed eliminazione

Come per GridView, l'aggiunta del supporto per la modifica, l'inserimento o l'eliminazione aggiunge un oggetto CommandField a DetailsView, come illustrato nella sintassi dichiarativa seguente:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Si noti che per l'oggetto DetailsView il CommandField viene visualizzato alla fine della raccolta Columns per impostazione predefinita. Poiché i campi di DetailsView vengono sottoposti a rendering come righe, CommandField viene visualizzato come una riga con i pulsanti Inserisci, modifica ed Elimina nella parte inferiore di DetailsView.

[![configurare DetailsView per supportare le operazioni di modifica, inserimento ed eliminazione](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configurare DetailsView per supportare le operazioni di modifica, inserimento ed eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))

Facendo clic sul pulsante Elimina viene avviata la stessa sequenza di eventi di GridView: un postback; seguito da DetailsView che popola il `DeleteParameters` di ObjectDataSource in base ai valori di `DataKeyNames`. e completato con una chiamata al metodo di `Delete()` di ObjectDataSource, che rimuove effettivamente il prodotto dal database. La modifica in DetailsView funziona anche in modo identico a quello di GridView.

Per l'inserimento, l'utente finale viene visualizzato con un nuovo pulsante che, se selezionato, esegue il rendering del DetailsView in "modalità di inserimento". Con "modalità di inserimento" il pulsante nuovo viene sostituito dai pulsanti Inserisci e Annulla e solo i BoundField la cui proprietà `InsertVisible` è impostata su `true` (impostazione predefinita). I campi dati identificati come campi a incremento automatico, ad esempio `ProductID`, hanno la [proprietà InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) impostata su `false` quando il DetailsView viene associato all'origine dati tramite lo smart tag.

Quando si associa un'origine dati a un oggetto DetailsView tramite lo smart tag, Visual Studio imposta la proprietà `InsertVisible` su `false` solo per i campi con incremento automatico. I campi di sola lettura, ad esempio `CategoryName` e `SupplierName`, verranno visualizzati nell'interfaccia utente "modalità di inserimento", a meno che la relativa proprietà `InsertVisible` non venga impostata in modo esplicito su `false`. Impostare le proprietà `InsertVisible` di questi due campi su `false`, tramite la sintassi dichiarativa di DetailsView o tramite il collegamento Modifica campi nello smart tag. Nella figura 19 viene illustrato come impostare le proprietà `InsertVisible` su `false` facendo clic sul collegamento Modifica campi.

[![Northwind Traders offre ora Acme Tea](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Northwind Traders offre ora il tè Acme ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))

Dopo aver impostato le proprietà `InsertVisible`, visualizzare la pagina `Basics.aspx` in un browser e fare clic sul pulsante nuovo. La figura 20 Mostra DetailsView durante l'aggiunta di una nuova bevanda, Acme Tea, alla nostra linea di prodotti.

[![Northwind Traders offre ora Acme Tea](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Northwind Traders ora offre il tè Acme ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))

Dopo aver immesso i dettagli per il tè Acme e aver fatto clic sul pulsante Inserisci, viene eseguito un postback e il nuovo record viene aggiunto alla tabella di database `Products`. Poiché questo DetailsView elenca i prodotti in ordine con cui sono presenti nella tabella di database, è necessario eseguire il paging dell'ultimo prodotto per visualizzare il nuovo prodotto.

[![dettagli per il tè Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: dettagli per il tè Acme ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))

> [!NOTE]
> La [proprietà CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) di DetailsView indica l'interfaccia da visualizzare. i possibili valori sono i seguenti: `Edit`, `Insert`o `ReadOnly`. La [proprietà DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica la modalità di ritorno di DetailsView dopo il completamento di un'operazione di modifica o inserimento ed è utile per la visualizzazione di un oggetto DetailsView permanente in modalità di modifica o inserimento.

Il punto e fare clic sulle funzionalità di inserimento e modifica di DetailsView subiscono le stesse limitazioni di GridView: l'utente deve immettere i valori `CategoryID` e `SupplierID` esistenti tramite una casella di testo. l'interfaccia non dispone di alcuna logica di convalida. tutti i campi prodotto che non consentono valori `NULL` o non hanno un valore predefinito specificato a livello di database devono essere inclusi nell'interfaccia di inserimento e così via.

Le tecniche che verranno esaminate per estendere e migliorare l'interfaccia di modifica di GridView negli articoli futuri possono essere applicate anche alle interfacce di modifica e inserimento del controllo DetailsView.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Uso di FormView per un'interfaccia utente per la modifica dei dati più flessibile

FormView offre supporto incorporato per l'inserimento, la modifica e l'eliminazione dei dati, ma poiché usa i modelli anziché i campi, non è possibile aggiungere i BoundField o CommandField usati dai controlli GridView e DetailsView per fornire i dati interfaccia di modifica. Al contrario, questa interfaccia i controlli Web per la raccolta di input utente quando si aggiunge un nuovo elemento o la modifica di una esistente insieme ai pulsanti nuovo, modifica, Elimina, Inserisci, aggiorna e Annulla deve essere aggiunto manualmente ai modelli appropriati. Fortunatamente, Visual Studio creerà automaticamente l'interfaccia necessaria quando il FormView viene associato a un'origine dati tramite l'elenco a discesa nello smart tag.

Per illustrare queste tecniche, iniziare aggiungendo un FormView alla pagina `Basics.aspx` e, dallo smart tag di FormView, associarlo a ObjectDataSource già creato. Verrà generato un `EditItemTemplate`, `InsertItemTemplate`e `ItemTemplate` per l'oggetto FormView con i controlli Web TextBox per la raccolta dei controlli Web di input e pulsanti dell'utente per i pulsanti nuovo, modifica, Elimina, Inserisci, aggiorna e Annulla. Inoltre, la proprietà `DataKeyNames` di FormView viene impostata sul campo di chiave primaria (`ProductID`) dell'oggetto restituito da ObjectDataSource. Infine, selezionare l'opzione Abilita paging nello smart tag di FormView.

Di seguito viene illustrato il markup dichiarativo per la `ItemTemplate` di FormView dopo che il FormView è stato associato a ObjectDataSource. Per impostazione predefinita, ogni campo prodotto valore non booleano viene associato alla proprietà `Text` di un controllo Web Label mentre ogni campo di valore booleano (`Discontinued`) viene associato alla proprietà `Checked` di un controllo Web CheckBox disabilitato. Per consentire ai pulsanti nuovo, modifica ed Elimina di attivare determinati comportamenti FormView quando si fa clic, è fondamentale che i valori `CommandName` siano impostati rispettivamente su `New`, `Edit`e `Delete`.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

La figura 22 Mostra la `ItemTemplate` di FormView quando viene visualizzata tramite un browser. Ogni campo prodotto è elencato con i pulsanti nuovo, modifica ed Elimina in basso.

[![predefinita FormView ItemTemplate elenca ogni campo prodotto insieme ai pulsanti nuovo, modifica ed Elimina](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: il `ItemTemplate` FormView predefinita elenca ogni campo prodotto insieme ai pulsanti nuovo, modifica ed Elimina ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))

Analogamente a GridView e DetailsView, facendo clic sul pulsante Elimina o su qualsiasi pulsante, LinkButton o ImageButton la cui proprietà `CommandName` è impostata su Elimina causa un postback, popola l'`DeleteParameters` di ObjectDataSource in base al valore `DataKeyNames` di FormView e richiama il metodo di `Delete()` di ObjectDataSource.

Quando si fa clic sul pulsante modifica, viene eseguito un postback e i dati vengono riassociati al `EditItemTemplate`, che è responsabile del rendering dell'interfaccia di modifica. Questa interfaccia include i controlli Web per la modifica dei dati insieme ai pulsanti Aggiorna e Annulla. Il `EditItemTemplate` predefinito generato da Visual Studio contiene un'etichetta per tutti i campi di incremento automatico (`ProductID`), una casella di testo per ogni campo valore non booleano e una casella di controllo per ogni campo di valore booleano. Questo comportamento è molto simile ai BoundField generati automaticamente nei controlli GridView e DetailsView.

> [!NOTE]
> Un piccolo problema con la generazione automatica del `EditItemTemplate` di FormView è che esegue il rendering dei controlli Web TextBox per i campi di sola lettura, ad esempio `CategoryName` e `SupplierName`. A breve verrà illustrato come considerare questo problema.

I controlli TextBox nella `EditItemTemplate` hanno la proprietà `Text` associata al valore del campo dati corrispondente usando l'associazione dati *bidirezionale*. L'associazione dati bidirezionale, indicata da `<%# Bind("dataField") %>`, esegue l'associazione dati sia durante l'associazione dei dati al modello che durante il popolamento dei parametri di ObjectDataSource per l'inserimento o la modifica di record. Ovvero, quando l'utente fa clic sul pulsante modifica dal `ItemTemplate`, il metodo `Bind()` restituisce il valore del campo dati specificato. Dopo che l'utente ha apportato le modifiche e fa clic su Aggiorna, i valori di cui è stato eseguito il postback corrispondenti ai campi dati specificati con `Bind()` vengono applicati al `UpdateParameters`di ObjectDataSource. In alternativa, l'associazione dati unidirezionale, indicata da `<%# Eval("dataField") %>`, recupera i valori dei campi dati solo quando si associano i dati al modello e *non* restituisce i valori immessi dall'utente ai parametri dell'origine dati durante il postback.

Il markup dichiarativo seguente mostra la `EditItemTemplate`di FormView. Si noti che il metodo `Bind()` viene usato nella sintassi di associazione dei dati e che i controlli Web dei pulsanti Aggiorna e Annulla hanno le proprietà `CommandName` impostate di conseguenza.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

L'`EditItemTemplate`, a questo punto, causerà la generazione di un'eccezione se si tenta di usarla. Il problema è che i campi `CategoryName` e `SupplierName` vengono sottoposti a rendering come controlli Web TextBox nell'`EditItemTemplate`. È necessario modificare queste caselle di testo in etichette o rimuoverle completamente. È sufficiente eliminarli completamente dal `EditItemTemplate`.

La figura 23 Mostra il FormView in un browser dopo aver fatto clic sul pulsante modifica per Chai. Si noti che i campi `SupplierName` e `CategoryName` visualizzati nel `ItemTemplate` non sono più presenti, perché sono stati appena rimossi dal `EditItemTemplate`. Quando si fa clic sul pulsante Aggiorna, FormView continua con la stessa sequenza di passaggi dei controlli GridView e DetailsView.

[![per impostazione predefinita EditItemTemplate Mostra ogni campo prodotto modificabile come casella di testo o casella di controllo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: per impostazione predefinita, il `EditItemTemplate` Mostra ogni campo prodotto modificabile come casella di testo o casella di controllo ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))

Quando si fa clic sul pulsante Inserisci per il `ItemTemplate` di FormView viene seguito un postback. Tuttavia, nessun dato è associato a FormView perché è in corso l'aggiunta di un nuovo record. L'interfaccia `InsertItemTemplate` include i controlli Web per l'aggiunta di un nuovo record insieme ai pulsanti Inserisci e Annulla. Il `InsertItemTemplate` predefinito generato da Visual Studio contiene una casella di testo per ogni campo valore non booleano e una casella di controllo per ogni campo di valore booleano, in modo analogo all'interfaccia `EditItemTemplate`generata automaticamente. I controlli TextBox hanno la proprietà `Text` associata al valore del campo dati corrispondente usando DataBinding bidirezionale.

Il markup dichiarativo seguente mostra la `InsertItemTemplate`di FormView. Si noti che il metodo `Bind()` viene usato nella sintassi di associazione dei dati e che i controlli Web del pulsante Inserisci e Annulla hanno le proprietà `CommandName` impostate di conseguenza.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

C'è una sottigliezza con la generazione automatica del `InsertItemTemplate`di FormView. In particolare, i controlli Web della casella di testo vengono creati anche per i campi di sola lettura, ad esempio `CategoryName` e `SupplierName`. Analogamente alla `EditItemTemplate`, è necessario rimuovere queste caselle di testo dall'`InsertItemTemplate`.

La figura 24 Mostra il FormView in un browser quando si aggiunge un nuovo prodotto, Acme coffee. Si noti che i campi `SupplierName` e `CategoryName` visualizzati nel `ItemTemplate` non sono più presenti, perché sono stati appena rimossi. Quando si fa clic sul pulsante Inserisci, FormView procede attraverso la stessa sequenza di passaggi del controllo DetailsView, aggiungendo un nuovo record alla tabella `Products`. La figura 25 Mostra i dettagli del prodotto di Acme coffee in FormView dopo l'inserimento.

[![InsertItemTemplate impone l'interfaccia di inserimento di FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: il `InsertItemTemplate` impone l'interfaccia di inserimento di FormView ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))

[![i dettagli relativi al nuovo prodotto, il caffè Acme, viene visualizzato in FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: i dettagli per il nuovo prodotto, il caffè Acme, vengono visualizzati in FormView ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))

Separando le interfacce di sola lettura, la modifica e l'inserimento in tre modelli distinti, FormView consente un livello di controllo più preciso su queste interfacce rispetto a DetailsView e GridView.

> [!NOTE]
> Analogamente a DetailsView, la proprietà `CurrentMode` di FormView indica l'interfaccia da visualizzare e la relativa proprietà `DefaultMode` indica la modalità di ritorno di FormView dopo il completamento di un'operazione di modifica o inserimento.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate le nozioni di base per l'inserimento, la modifica e l'eliminazione di dati tramite GridView, DetailsView e FormView. Tutti e tre questi controlli forniscono un certo livello di funzionalità di modifica dei dati predefinite che possono essere utilizzate senza scrivere una sola riga di codice nella pagina ASP.NET, grazie ai controlli Web dei dati e a ObjectDataSource. Tuttavia, le tecniche semplici per il punto e il clic eseguono il rendering di un'interfaccia utente di modifica dei dati piuttosto fragile e ingenua. Per fornire la convalida, inserire i valori a livello di codice, gestire normalmente le eccezioni, personalizzare l'interfaccia utente e così via, è necessario affidarsi a una serie di tecniche che verranno discusse nelle varie esercitazioni successive.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
