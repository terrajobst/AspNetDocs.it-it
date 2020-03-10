---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Uso dei modelli di FormView (C#) | Microsoft Docs
author: rick-anderson
description: Diversamente da DetailsView, FormView non è costituito da campi. Viene invece eseguito il rendering del FormView utilizzando i modelli. In questa esercitazione verrà esaminato l'utilizzo di F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 013c6878aad1a2277b0a334c096ff16ed84a95f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531206"
---
# <a name="using-the-formviews-templates-c"></a>Uso dei modelli di FormView (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) o [scaricare il file PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> Diversamente da DetailsView, FormView non è costituito da campi. Viene invece eseguito il rendering del FormView utilizzando i modelli. In questa esercitazione verrà esaminato l'utilizzo del controllo FormView per presentare una visualizzazione dei dati meno rigida.

## <a name="introduction"></a>Introduzione

Nelle ultime due esercitazioni è stato illustrato come personalizzare gli output dei controlli GridView e DetailsView usando TemplateFields. TemplateFields consente di personalizzare in modo estremamente personalizzato il contenuto di un campo specifico, ma alla fine entrambi i controlli GridView e DetailsView hanno un aspetto simile alla griglia. Per molti scenari, un layout simile a Grid è ideale, ma a volte è necessario un livello di visualizzazione più fluido e meno rigido. Quando si visualizza un singolo record, è possibile usare il controllo FormView per un layout fluido di questo tipo.

Diversamente da DetailsView, FormView non è costituito da campi. Non è possibile aggiungere un BoundField o un TemplateField a un oggetto FormView. Viene invece eseguito il rendering del FormView utilizzando i modelli. Si pensi a FormView come un controllo DetailsView che contiene un singolo TemplateField. Il FormView supporta i modelli seguenti:

- `ItemTemplate` utilizzato per eseguire il rendering del record specifico visualizzato in FormView
- `HeaderTemplate` utilizzata per specificare una riga di intestazione facoltativa
- `FooterTemplate` utilizzata per specificare una riga del piè di pagina facoltativa
- `EmptyDataTemplate` quando il `DataSource` di FormView non dispone di alcun record, il `EmptyDataTemplate` viene usato al posto del `ItemTemplate` per il rendering del markup del controllo
- `PagerTemplate` può essere usato per personalizzare l'interfaccia di paging per gli FormView con paging abilitato
- `EditItemTemplate` / `InsertItemTemplate` utilizzato per personalizzare l'interfaccia di modifica o l'interfaccia di inserimento per FormView che supportano tale funzionalità.

In questa esercitazione verrà esaminato l'utilizzo del controllo FormView per presentare una visualizzazione meno rigida dei prodotti. Anziché disporre di campi per nome, categoria, fornitore e così via, il `ItemTemplate` di FormView visualizzerà questi valori utilizzando una combinazione di un elemento intestazione e una `<table>` (vedere la figura 1).

[![il FormView si interrompe dal layout simile a Grid visualizzato in DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Figura 1**: FormView si interrompe dal layout simile a Grid visualizzato in DetailsView ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-formview-s-templates-cs/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-formview"></a>Passaggio 1: associazione dei dati al FormView

Aprire la pagina `FormView.aspx` e trascinare un oggetto FormView dalla casella degli strumenti nella finestra di progettazione. Quando si aggiunge per la prima volta il FormView, questo viene visualizzato come una casella grigia, indicando che è necessario un `ItemTemplate`.

[![non è possibile eseguire il rendering di FormView nella finestra di progettazione fino a quando non viene fornito un ItemTemplate](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Figura 2**: non è possibile eseguire il rendering di FormView nella finestra di progettazione fino a quando non viene fornito un `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-formview-s-templates-cs/_static/image6.png))

Il `ItemTemplate` può essere creato manualmente (tramite la sintassi dichiarativa) oppure può essere creato automaticamente associando FormView a un controllo origine dati tramite la finestra di progettazione. Questo `ItemTemplate` creato automaticamente contiene codice HTML che elenca il nome di ogni campo e un controllo etichetta la cui proprietà `Text` è associata al valore del campo. Questo approccio crea anche automaticamente una `InsertItemTemplate` e `EditItemTemplate`, entrambe popolate con i controlli di input per ognuno dei campi dati restituiti dal controllo origine dati.

Se si vuole creare automaticamente il modello, dallo smart tag di FormView aggiungere un nuovo controllo ObjectDataSource che richiama il metodo `GetProducts()` della classe `ProductsBLL`. Verrà creato un oggetto FormView con una `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate`. Dalla visualizzazione origine rimuovere il `InsertItemTemplate` e `EditItemTemplate` poiché non si è interessati alla creazione di un FormView che supporta ancora la modifica o l'inserimento. A questo punto, cancellare il markup all'interno del `ItemTemplate` in modo da avere uno Slate pulito da usare.

Se si preferisce creare manualmente il `ItemTemplate`, è possibile aggiungere e configurare ObjectDataSource trascinandoli dalla casella degli strumenti nella finestra di progettazione. Tuttavia, non impostare l'origine dati di FormView dalla finestra di progettazione. Al contrario, passare alla visualizzazione origine e impostare manualmente la proprietà `DataSourceID` di FormView sul valore `ID` di ObjectDataSource. Successivamente, aggiungere manualmente il `ItemTemplate`.

Indipendentemente dall'approccio adottato, a questo punto il markup dichiarativo di FormView dovrebbe essere simile al seguente:

[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Controllare la casella di controllo Abilita paging nello smart tag di FormView. verrà aggiunto l'attributo `AllowPaging="True"` alla sintassi dichiarativa di FormView. Inoltre, impostare la proprietà `EnableViewState` su false.

## <a name="step-2-defining-theitemtemplates-markup"></a>Passaggio 2: definizione del markup del`ItemTemplate`

Con l'oggetto FormView associato al controllo ObjectDataSource e configurato per supportare il paging, è possibile specificare il contenuto per la `ItemTemplate`. Per questa esercitazione, verrà visualizzato il nome del prodotto in un'intestazione `<h3>`. A questo punto, si userà un `<table>` HTML per visualizzare le proprietà del prodotto rimanenti in una tabella a quattro colonne in cui la prima e la terza colonna elencano i nomi delle proprietà e il secondo e il quarto elenco dei relativi valori.

Questo markup può essere immesso tramite l'interfaccia di modifica dei modelli di FormView nella finestra di progettazione o immesso manualmente tramite la sintassi dichiarativa. Quando si lavora con i modelli, in genere risulta più veloce lavorare direttamente con la sintassi dichiarativa, ma è possibile usare qualsiasi tecnica più comoda.

Il markup seguente mostra il markup dichiarativo FormView dopo che la struttura del `ItemTemplate`è stata completata:

[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Si noti che la sintassi di associazione dati-`<%# Eval("ProductName") %>`, ad esempio, può essere inserita direttamente nell'output del modello. Ovvero non è necessario assegnarlo alla proprietà `Text` di un controllo Label. Ad esempio, il valore `ProductName` viene visualizzato in un elemento `<h3>` usando `<h3><%# Eval("ProductName") %></h3>`, che per il prodotto Chai verrà renderizzato come `<h3>Chai</h3>`.

Le classi CSS `ProductPropertyLabel` e `ProductPropertyValue` vengono utilizzate per specificare lo stile dei nomi e dei valori delle proprietà del prodotto nell'`<table>`. Queste classi CSS sono definite in `Styles.css` e fanno in modo che i nomi delle proprietà siano in grassetto e allineati a destra e aggiungono una spaziatura interna a destra per i valori delle proprietà.

Poiché non sono disponibili CheckBoxFields con FormView, per visualizzare il valore `Discontinued` come casella di controllo, è necessario aggiungere il controllo CheckBox personalizzato. La proprietà `Enabled` è impostata su false, rendendola di sola lettura e la proprietà `Checked` della casella di controllo è associata al valore del campo `Discontinued` data.

Con la `ItemTemplate` completata, le informazioni sul prodotto vengono visualizzate in modo molto più fluido. Confrontare l'output DetailsView dell'ultima esercitazione (Figura 3) con l'output generato da FormView in questa esercitazione (Figura 4).

[![l'output del DetailsView rigido](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Figura 3**: l'output del DetailsView rigido ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-formview-s-templates-cs/_static/image9.png))

[![output di FormView fluido](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Figura 4**: output di FormView fluido ([fare clic per visualizzare l'immagine con dimensioni complete](using-the-formview-s-templates-cs/_static/image12.png))

## <a name="summary"></a>Riepilogo

Mentre i controlli GridView e DetailsView possono avere un output personalizzato con TemplateFields, entrambi presentano comunque i dati in un formato a griglia. Per i casi in cui è necessario visualizzare un singolo record utilizzando un layout meno rigido, FormView è la scelta ideale. Analogamente a DetailsView, FormView esegue il rendering di un singolo record dalla relativa `DataSource`, ma a differenza di DetailsView, è costituito solo da modelli e non supporta i campi.

Come illustrato in questa esercitazione, FormView consente un layout più flessibile quando viene visualizzato un singolo record. Nelle esercitazioni future verranno esaminati i controlli DataList e Repeater, che forniscono lo stesso livello di flessibilità del FormsView, ma sono in grado di visualizzare più record, ad esempio GridView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore del lead per questa esercitazione è stato pronto Gilmore. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-detailsview-control-cs.md)
> [Successivo](displaying-summary-information-in-the-gridview-s-footer-cs.md)
