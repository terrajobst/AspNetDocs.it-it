---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Aggiunta di una colonna GridView di caselle di controllo (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo intuitivo per selezionare più righe di G...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: c620b2eac5844d4030c1309b45e7d6a72d1f386a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607352"
---
# <a name="adding-a-gridview-column-of-checkboxes-vb"></a>Aggiunta di una colonna GridView di caselle di controllo (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) o [scaricare il file PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> In questa esercitazione viene illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo intuitivo per selezionare più righe di GridView.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato esaminato come aggiungere una colonna di pulsanti di opzione a GridView allo scopo di selezionare un record particolare. Una colonna di pulsanti di opzione è un'interfaccia utente adatta quando l'utente è limitato a scegliere al massimo un elemento dalla griglia. In alcuni casi, tuttavia, potrebbe essere necessario consentire all'utente di selezionare un numero arbitrario di elementi dalla griglia. I client di posta elettronica basati sul Web, ad esempio, in genere visualizzano l'elenco dei messaggi con una colonna di caselle di controllo. L'utente può selezionare un numero arbitrario di messaggi e quindi eseguire un'azione, ad esempio spostando i messaggi di posta elettronica in un'altra cartella o eliminando i messaggi.

In questa esercitazione verrà illustrato come aggiungere una colonna di caselle di controllo e come determinare le caselle di controllo selezionate durante il postback. In particolare, verrà compilato un esempio che simula in modo accurato l'interfaccia utente del client di posta elettronica basata sul Web. Nell'esempio seguente viene incluso un GridView di paging che elenca i prodotti nella tabella di database `Products` con una casella di controllo in ogni riga (vedere la figura 1). Quando si fa clic sul pulsante Elimina prodotti selezionati, i prodotti selezionati verranno eliminati.

[![ogni riga di prodotto include una casella di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figura 1**: ogni riga di prodotto include una casella[di controllo (fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Passaggio 1: aggiunta di un controllo GridView di paging che elenca le informazioni sul prodotto

Prima di preoccuparsi di aggiungere una colonna di caselle di controllo, è necessario concentrarsi innanzitutto sull'elenco dei prodotti in un GridView che supporta il paging. Per iniziare, aprire la pagina `CheckBoxField.aspx` nella cartella `EnhancedGridView` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostando la relativa `ID` su `Products`. Successivamente, scegliere di associare GridView a un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per l'utilizzo della classe `ProductsBLL`, chiamando il metodo `GetProducts()` per restituire i dati. Poiché il controllo GridView sarà di sola lettura, impostare gli elenchi a discesa nelle schede UPDATE, INSERT e DELETE su (nessuno).

[![creare un nuovo ObjectDataSource denominato ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))

[![configurare ObjectDataSource per il recupero dei dati tramite il metodo GetProducts ()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per recuperare i dati usando il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))

[![impostare gli elenchi a discesa nelle schede Aggiorna, Inserisci ed Elimina su (nessuno)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figura 4**: impostare gli elenchi a discesa nelle schede di aggiornamento, inserimento ed eliminazione su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))

Dopo aver completato la configurazione guidata origine dati, Visual Studio creerà automaticamente BoundColumns e un CheckBoxColumn per i campi dati correlati al prodotto. Come nell'esercitazione precedente, rimuovere tutti i BoundField, tranne i `ProductName`, `CategoryName`e `UnitPrice`, e modificare le proprietà del `HeaderText` in Product, Category e price. Configurare il BoundField `UnitPrice` in modo che il relativo valore venga formattato come valuta. Configurare inoltre GridView per supportare il paging selezionando la casella di controllo Abilita paging dallo smart tag.

Consente inoltre di aggiungere l'interfaccia utente per l'eliminazione dei prodotti selezionati. Aggiungere un controllo Web Button sotto GridView, impostando il `ID` su `DeleteSelectedProducts` e la relativa proprietà `Text` per eliminare i prodotti selezionati. Anziché eliminare effettivamente i prodotti dal database, in questo esempio verrà visualizzato solo un messaggio che informa i prodotti che sarebbero stati eliminati. Per risolvere questo problema, aggiungere un controllo Web etichetta sotto il pulsante. Impostare l'ID su `DeleteResults`, deselezionare la relativa proprietà `Text` e impostare le proprietà `Visible` e `EnableViewState` su `False`.

Dopo aver apportato queste modifiche, il markup dichiarativo GridView, ObjectDataSource, Button e Label dovrebbe essere simile al seguente:

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Esaminare la pagina in un browser (vedere la figura 5). A questo punto verranno visualizzati il nome, la categoria e il prezzo dei primi dieci prodotti.

[![vengono elencati il nome, la categoria e il prezzo dei primi dieci prodotti](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figura 5**: il nome, la categoria e il prezzo dei primi dieci prodotti sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Passaggio 2: aggiunta di una colonna di caselle di controllo

Poiché ASP.NET 2,0 include un oggetto CheckBoxField, è possibile che venga usato per aggiungere una colonna di caselle di controllo a un controllo GridView. Sfortunatamente, questo non è il caso, perché CheckBoxField è progettato per funzionare con un campo dati booleano. In altre termini, per usare CheckBoxField è necessario specificare il campo dati sottostante il cui valore viene consultato per determinare se la casella di controllo di cui è stato eseguito il rendering è selezionata. Non è possibile usare CheckBoxField per includere solo una colonna di caselle di controllo deselezionate.

Al contrario, è necessario aggiungere un TemplateField e aggiungere un controllo Web CheckBox al relativo `ItemTemplate`. Procedere con l'aggiunta di un TemplateField alla `Products` GridView e impostarlo come primo campo (a sinistra). Dallo smart tag di GridView, fare clic sul collegamento modifica modelli, quindi trascinare un controllo Web CheckBox dalla casella degli strumenti all'`ItemTemplate`. Impostare questa casella di controllo `ID` proprietà su `ProductSelector`.

[![aggiungere un controllo Web CheckBox denominato ProductSelector a TemplateField ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figura 6**: aggiungere un controllo Web CheckBox denominato `ProductSelector` al `ItemTemplate` TemplateField s ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))

Con il controllo Web TemplateField e CheckBox aggiunto, ogni riga include ora una casella di controllo. La figura 7 Mostra questa pagina, quando viene visualizzata tramite un browser, dopo l'aggiunta della casella di controllo TemplateField e.

[![ogni riga di prodotto ora include una casella di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figura 7**: ogni riga di prodotto ora include una casella[di controllo (fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Passaggio 3: determinazione delle caselle di controllo selezionate durante il postback

A questo punto è disponibile una colonna di caselle di controllo, ma non è possibile determinare quali caselle di controllo sono state controllate durante il postback. Quando si fa clic sul pulsante Elimina prodotti selezionati, tuttavia, è necessario conoscere le caselle di controllo controllate per eliminare tali prodotti.

La [proprietà`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) di GridView fornisce l'accesso alle righe di dati in GridView. È possibile scorrere queste righe, accedere a livello di codice alla casella di controllo e quindi consultare la relativa proprietà `Checked` per determinare se la casella di controllo è stata selezionata.

Creare un gestore eventi per l'evento `Click` del controllo Web di `DeleteSelectedProducts` Button e aggiungere il codice seguente:

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

La proprietà `Rows` restituisce una raccolta di istanze di `GridViewRow` che comprimano le righe di dati di GridView. Il ciclo `For Each` enumera questa raccolta. Per ogni oggetto `GridViewRow`, viene eseguito l'accesso a livello di codice alla casella di controllo Row con `row.FindControl("controlID")`. Se la casella di controllo è selezionata, la riga s corrispondente `ProductID` valore viene recuperato dalla raccolta di `DataKeys`. In questo esercizio viene semplicemente visualizzato un messaggio informativo nell'etichetta di `DeleteResults`, sebbene in un'applicazione funzionante si effettui una chiamata al metodo `ProductsBLL` Class s `DeleteProduct(productID)`.

Con l'aggiunta di questo gestore eventi, facendo clic sul pulsante Elimina prodotti selezionati viene ora visualizzato il `ProductID` dei prodotti selezionati.

[![quando si fa clic sul pulsante Elimina prodotti selezionati, i prodotti selezionati ProductID sono elencati](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figura 8**: quando si fa clic sul pulsante Elimina prodotti selezionati, i prodotti selezionati `ProductID` s sono elencati ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Passaggio 4: aggiunta di check all e deselezionare tutti i pulsanti

Se un utente desidera eliminare tutti i prodotti nella pagina corrente, deve selezionare ognuna delle dieci caselle di controllo. È possibile velocizzare questo processo aggiungendo un pulsante Seleziona tutto che, quando selezionato, seleziona tutte le caselle di controllo nella griglia. Un pulsante Deseleziona tutto sarebbe ugualmente utile.

Aggiungere due controlli Web Button alla pagina, inserendoli sopra il GridView. Impostare la prima `ID` s su `CheckAll` e la relativa proprietà `Text` per verificare tutto; impostare il secondo `ID` su `UncheckAll` e la relativa proprietà `Text` per deselezionare tutti.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Successivamente, creare un metodo nella classe code-behind denominata `ToggleCheckState(checkState)` che, quando viene richiamato, enumera la raccolta `Products` GridView s `Rows` e imposta ogni casella di controllo `Checked` proprietà sul valore del parametro *CheckState* passato.

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Successivamente, creare `Click` gestori eventi per i pulsanti `CheckAll` e `UncheckAll`. Nel gestore dell'evento `CheckAll` s è sufficiente chiamare `ToggleCheckState(True)`; in `UncheckAll`chiamare `ToggleCheckState(False)`.

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Con questo codice, facendo clic sul pulsante Controlla tutto, viene generato un postback e vengono controllate tutte le caselle di controllo in GridView. Analogamente, facendo clic su Deseleziona tutte le caselle di controllo Deseleziona tutto. La figura 9 Mostra la schermata dopo che il pulsante Controlla tutto è stato selezionato.

[![facendo clic sul pulsante Seleziona tutto selezionare tutte le caselle di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figura 9**: fare clic sul pulsante Seleziona tutto per selezionare tutte le caselle di controllo ([fare clic per visualizzare l'immagine con dimensioni complete](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))

> [!NOTE]
> Quando si visualizza una colonna di caselle di controllo, un approccio per la selezione o la deselezione di tutte le caselle di controllo avviene tramite una casella di controllo nella riga di intestazione. Inoltre, il controllo corrente All/Uncheck All implementation richiede un postback. Le caselle di controllo possono essere selezionate o deselezionate, tuttavia, interamente tramite script sul lato client, offrendo così un'esperienza utente ottimizzare. Per esplorare la casella di controllo usando una riga di intestazione per verificare tutti e deselezionare tutti i dettagli, oltre a una discussione sull'uso delle tecniche sul lato client, vedere Selezionare [tutte le caselle di controllo in un GridView usando uno script lato client e una casella di controllo Seleziona tutto](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).

## <a name="summary"></a>Riepilogo

Nei casi in cui è necessario consentire agli utenti di scegliere un numero arbitrario di righe da un controllo GridView prima di procedere, l'aggiunta di una colonna di caselle di controllo è un'opzione. Come illustrato in questa esercitazione, inclusa una colonna di caselle di controllo in GridView che comporta l'aggiunta di un TemplateField con un controllo Web CheckBox. Usando un controllo Web, anziché inserendo il markup direttamente nel modello, come nell'esercitazione precedente, ASP.NET memorizza automaticamente le caselle di controllo e non sono state controllate durante il postback. È anche possibile accedere a livello di codice alle caselle di controllo nel codice per determinare se una determinata casella di controllo è selezionata o modificare lo stato selezionato.

Questa esercitazione e l'ultima esaminata l'aggiunta di una colonna del selettore di riga al controllo GridView. Nell'esercitazione successiva si esaminerà come, con un po' di lavoro, è possibile aggiungere funzionalità di inserimento al controllo GridView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Successivo](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
