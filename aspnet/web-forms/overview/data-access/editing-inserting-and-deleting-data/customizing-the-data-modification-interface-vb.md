---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizzazione dell'interfaccia di modifica dei dati (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come personalizzare l'interfaccia di un GridView modificabile, sostituendo i controlli TextBox e CheckBox standard con alternat...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec7bdde6b2bffbbda066b0441bbd36b7072197
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592617"
---
# <a name="customizing-the-data-modification-interface-vb"></a>Personalizzazione dell'interfaccia di modifica dei dati (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) o [scaricare il file PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> In questa esercitazione verrà illustrato come personalizzare l'interfaccia di un GridView modificabile, sostituendo i controlli TextBox e CheckBox standard con i controlli Web di input alternativi.

## <a name="introduction"></a>Introduzione

I BoundField e i CheckBoxFields utilizzati dai controlli GridView e DetailsView semplificano il processo di modifica dei dati a causa della loro capacità di eseguire il rendering di interfacce di sola lettura, modificabili e inseribili. È possibile eseguire il rendering di queste interfacce senza dover aggiungere codice o markup dichiarativo. Tuttavia, le interfacce del BoundField e di CheckBoxField non hanno una personalizzazione spesso necessaria in scenari reali. Per personalizzare l'interfaccia modificabile o inseribile in GridView o DetailsView, è necessario usare invece un TemplateField.

Nell' [esercitazione precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) è stato illustrato come personalizzare le interfacce di modifica dei dati mediante l'aggiunta di controlli Web di convalida. In questa esercitazione verrà illustrato come personalizzare i controlli Web della raccolta dati effettivi, sostituendo i controlli casella di controllo e TextBox standard di BoundField e CheckBoxField con controlli Web di input alternativi. In particolare, verrà compilato un GridView modificabile che consente di aggiornare il nome, la categoria, il fornitore e lo stato di un prodotto. Quando si modifica una riga specifica, il rendering dei campi Category e Supplier viene eseguito come DropDownList, che contiene il set di categorie e fornitori disponibili tra cui scegliere. Inoltre, la casella di controllo predefinita di CheckBoxField verrà sostituita con un controllo RadioButtonList che offre due opzioni: "attivo" e "interrotto".

[![l'interfaccia di modifica di GridView include gli DropDownList e i radiopulsanti](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: l'interfaccia di modifica di GridView include gli DropDownList e i radiopulsanti ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Passaggio 1: creazione dell'overload`UpdateProduct`appropriato

In questa esercitazione verrà creato un controllo GridView modificabile che consente di modificare il nome, la categoria, il fornitore e lo stato non più disponibile di un prodotto. Per questo motivo, è necessario un overload di `UpdateProduct` che accetti cinque parametri di input di questi quattro valori di prodotto oltre al `ProductID`. Come negli overload precedenti, questo verrà:

1. Recuperare le informazioni sul prodotto dal database per il `ProductID`specificato,
2. Aggiornare i campi `ProductName`, `CategoryID`, `SupplierID`e `Discontinued` e
3. Inviare la richiesta di aggiornamento al DAL tramite il metodo di `Update()` del TableAdapter.

Per brevità, per questo particolare overload ho omesso il controllo delle regole di business che garantisce che un prodotto contrassegnato come obsoleto non sia l'unico prodotto offerto dal fornitore. È possibile aggiungerlo in se si preferisce o, idealmente, effettuare il refactoring della logica a un metodo separato.

Nel codice seguente viene illustrato il nuovo overload `UpdateProduct` nella classe `ProductsBLL`:

[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Passaggio 2: creazione del controllo GridView modificabile

Con l'overload di `UpdateProduct` aggiunto, è possibile creare il GridView modificabile. Aprire la pagina `CustomizedUI.aspx` nella cartella `EditInsertDelete` e aggiungere un controllo GridView alla finestra di progettazione. Successivamente, creare un nuovo ObjectDataSource dallo smart tag di GridView. Configurare ObjectDataSource per recuperare le informazioni sul prodotto tramite il metodo `GetProducts()` della classe `ProductBLL` e per aggiornare i dati del prodotto utilizzando l'overload di `UpdateProduct` appena creato. Nelle schede Inserisci ed Elimina selezionare (nessuno) dagli elenchi a discesa.

[![configurare ObjectDataSource per l'uso dell'overload UpdateProduct appena creato](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configurare ObjectDataSource per l'uso dell'overload `UpdateProduct` appena creato ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image6.png))

Come si è visto in tutte le esercitazioni sulla modifica dei dati, la sintassi dichiarativa per ObjectDataSource creata da Visual Studio assegna la proprietà `OldValuesParameterFormatString` `original_{0}`. Questo, ovviamente, non funzionerà con il livello della logica di business, perché i metodi non prevedono il passaggio del valore di `ProductID` originale. Pertanto, come è stato fatto nelle esercitazioni precedenti, rimuovere l'assegnazione di questa proprietà dalla sintassi dichiarativa oppure impostare il valore di questa proprietà su `{0}`.

Dopo questa modifica, il markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Si noti che la proprietà `OldValuesParameterFormatString` è stata rimossa e che è presente un `Parameter` nella raccolta di `UpdateParameters` per ognuno dei parametri di input previsti dall'overload `UpdateProduct`.

Mentre ObjectDataSource è configurato per aggiornare solo un subset di valori di prodotto, GridView mostra attualmente *tutti* i campi del prodotto. Modificare il controllo GridView in modo che:

- Include solo i BoundField `ProductName`, `SupplierName`, `CategoryName` e la `Discontinued` CheckBoxField
- I campi `CategoryName` e `SupplierName` da visualizzare prima (a sinistra di) `Discontinued` CheckBoxField
- La proprietà `HeaderText` `CategoryName` e `SupplierName` BoundField ' è impostata rispettivamente su "Category" e "Supplier"
- Il supporto per la modifica è abilitato (selezionare la casella di controllo Abilita modifica nello smart tag di GridView)

Una volta apportate queste modifiche, la finestra di progettazione sarà simile alla figura 3, con la sintassi dichiarativa di GridView illustrata di seguito.

[![rimuovere i campi non necessari da GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: rimuovere i campi non necessari da GridView ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

A questo punto il comportamento di sola lettura di GridView è completo. Quando si visualizzano i dati, viene eseguito il rendering di ogni prodotto come riga in GridView, mostrando il nome del prodotto, la categoria, il fornitore e lo stato interrotto.

[![l'interfaccia di sola lettura di GridView è stata completata](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: l'interfaccia di sola lettura di GridView è completa ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image12.png))

> [!NOTE]
> Come illustrato in [una panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](an-overview-of-inserting-updating-and-deleting-data-cs.md), è essenziale che lo stato di visualizzazione di GridView sia abilitato (comportamento predefinito). Se si imposta la proprietà `EnableViewState` GridView s su `false`, si rischia di avere utenti simultanei che eliminano o modificano inavvertitamente i record. Vedere [Avviso: problema di concorrenza con ASP.NET 2,0 GridView/DetailsView/formviews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per altre informazioni.

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Passaggio 3: uso di un oggetto DropDownList per le interfacce di modifica Category e Supplier

Tenere presente che l'oggetto `ProductsRow` contiene le proprietà `CategoryID`, `CategoryName`, `SupplierID`e `SupplierName`, che forniscono i valori ID di chiave esterna effettivi nella tabella `Products` database e i valori `Name` corrispondenti nelle tabelle `Categories` e `Suppliers`. Le `CategoryID` e `SupplierID` di `ProductRow`possono essere lette e scritte in, mentre le proprietà `CategoryName` e `SupplierName` sono contrassegnate come di sola lettura.

A causa dello stato di sola lettura delle proprietà `CategoryName` e `SupplierName`, i BoundField corrispondenti hanno la proprietà `ReadOnly` impostata su `True`, impedendo la modifica di questi valori quando viene modificata una riga. Sebbene sia possibile impostare la proprietà `ReadOnly` su `False`, eseguendo il rendering del `CategoryName` e `SupplierName` i BoundField come caselle di testo durante la modifica, un approccio di questo tipo genera un'eccezione quando l'utente tenta di aggiornare il prodotto perché non è presente alcun overload di `UpdateProduct` che accetta gli input `CategoryName` e `SupplierName`. In realtà, non si vuole creare un sovraccarico di questo tipo per due motivi:

- La tabella `Products` non dispone di campi `SupplierName` o `CategoryName`, ma `SupplierID` e `CategoryID`. Pertanto, si desidera che il metodo venga passato a questi valori ID specifici, non ai valori delle tabelle di ricerca.
- Richiedere all'utente di digitare il nome del fornitore o della categoria è inferiore all'ideale, perché richiede all'utente di conoscerne le categorie e i fornitori disponibili e le relative ortografie corrette.

I campi Supplier e Category devono visualizzare i nomi della categoria e dei fornitori in modalità di sola lettura (come ora) e un elenco a discesa delle opzioni applicabili durante la modifica. Utilizzando un elenco a discesa, l'utente finale può visualizzare rapidamente le categorie e i fornitori disponibili per la scelta tra e può effettuare la selezione in modo più semplice.

Per fornire questo comportamento, è necessario convertire i BoundField `SupplierName` e `CategoryName` in TemplateFields il cui `ItemTemplate` emette i valori `SupplierName` e `CategoryName` e il cui `EditItemTemplate` usa un controllo DropDownList per elencare le categorie e i fornitori disponibili.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Aggiunta della`Categories`e della`Suppliers`DropDownList

Per iniziare, convertire i BoundField di `SupplierName` e `CategoryName` in TemplateFields da: facendo clic sul collegamento Modifica colonne dallo smart tag di GridView; selezione del BoundField dall'elenco in basso a sinistra; e facendo clic sul collegamento "Converti questo campo in un TemplateField". Il processo di conversione creerà un TemplateField con una `ItemTemplate` e un `EditItemTemplate`, come illustrato nella sintassi dichiarativa di seguito:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Poiché il BoundField è stato contrassegnato come di sola lettura, sia la `ItemTemplate` che la `EditItemTemplate` contengono un controllo Web etichetta la cui proprietà `Text` è associata al campo dati applicabile (`CategoryName`nella sintassi precedente). È necessario modificare il `EditItemTemplate`, sostituendo il controllo Web Label con un controllo DropDownList.

Come illustrato nelle esercitazioni precedenti, il modello può essere modificato tramite la finestra di progettazione o direttamente dalla sintassi dichiarativa. Per modificarlo tramite la finestra di progettazione, fare clic sul collegamento modifica modelli dallo smart tag di GridView e scegliere di usare la `EditItemTemplate`del campo categoria. Rimuovere il controllo Web Label e sostituirlo con un controllo DropDownList, impostando la proprietà ID di DropDownList su `Categories`.

[![rimuovere il TextBox e aggiungere un oggetto DropDownList al EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: rimuovere il TextBox e aggiungere un oggetto DropDownList al `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image15.png))

È necessario quindi popolare il DropDownList con le categorie disponibili. Fare clic sul collegamento Scegli origine dati dallo smart tag di DropDownList e scegliere di creare un nuovo ObjectDataSource denominato `CategoriesDataSource`.

[![creare un nuovo controllo ObjectDataSource denominato CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image18.png))

Per fare in modo che ObjectDataSource restituisca tutte le categorie, associarlo al metodo `GetCategories()` della classe `CategoriesBLL`.

[![associare ObjectDataSource al metodo getcategorys () di CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: associare ObjectDataSource al metodo di `GetCategories()` del `CategoriesBLL`([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image21.png))

Infine, configurare le impostazioni di DropDownList in modo che il campo `CategoryName` venga visualizzato in ogni DropDownList `ListItem` con il campo `CategoryID` usato come valore.

[![visualizzare il campo CategoryName e il valore CategoryID usato come valore](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: visualizzare il campo `CategoryName` e il `CategoryID` usato come valore ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image24.png))

Dopo aver apportato queste modifiche, il markup dichiarativo per la `EditItemTemplate` nel `CategoryName` TemplateField includerà sia DropDownList che ObjectDataSource:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Il relativo stato di visualizzazione deve essere abilitato per il DropDownList nel `EditItemTemplate`. A breve verrà aggiunta la sintassi di associazione dati alla sintassi dichiarativa di DropDownList e i comandi di associazione dati come `Eval()` e `Bind()` possono essere visualizzati solo in controlli con stato di visualizzazione abilitato.

Ripetere questi passaggi per aggiungere un oggetto DropDownList denominato `Suppliers` al `EditItemTemplate`di `SupplierName` TemplateField. Questo comporterà l'aggiunta di un oggetto DropDownList al `EditItemTemplate` e la creazione di un altro ObjectDataSource. È tuttavia necessario configurare ObjectDataSource `Suppliers` ObjectDataSource per richiamare il metodo `GetSuppliers()` della classe `SuppliersBLL`. Inoltre, configurare il `Suppliers` DropDownList per visualizzare il campo `CompanyName` e utilizzare il campo `SupplierID` come valore per la `ListItem` s.

Dopo aver aggiunto gli DropDownList ai due `EditItemTemplate` s, caricare la pagina in un browser e fare clic sul pulsante modifica per il prodotto Seasoning di Cajun di chef Anton. Come illustrato nella figura 9, le colonne Category e Supplier del prodotto vengono visualizzate come elenchi a discesa contenenti le categorie e i fornitori disponibili tra cui scegliere. Si noti, tuttavia, che i *primi* elementi in entrambi gli elenchi a discesa sono selezionati per impostazione predefinita (bevande per la categoria e i liquidi esotici come fornitore), anche se la stagione cajun di chef Anton è un condimento fornito da New Orleans Cajun Lights.

[![il primo elemento negli elenchi a discesa è selezionato per impostazione predefinita](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: il primo elemento negli elenchi a discesa è selezionato per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image27.png))

Inoltre, se si fa clic su Aggiorna, si noterà che i valori `CategoryID` e `SupplierID` del prodotto sono impostati su `NULL`. Entrambi questi comportamenti indesiderati sono causati dal fatto che i controlli DropDownList nell'`EditItemTemplate` s non sono associati ad alcun campo dati dei dati del prodotto sottostante.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Associazione di DropDownList ai campi dati`CategoryID`e`SupplierID`

Per fare in modo che gli elenchi a discesa Category e Supplier del prodotto modificato siano impostati sui valori appropriati e che questi valori vengano restituiti al metodo `UpdateProduct` di BLL quando si fa clic su Aggiorna, è necessario associare le proprietà `SelectedValue` di DropDownList ai campi di `CategoryID` e `SupplierID` usando l'associazione dati bidirezionale. Per eseguire questa operazione con il `Categories` DropDownList, è possibile aggiungere `SelectedValue='<%# Bind("CategoryID") %>'` direttamente alla sintassi dichiarativa.

In alternativa, è possibile impostare i DataBindings di DropDownList modificando il modello tramite la finestra di progettazione e facendo clic sul collegamento Modifica DataBindings dallo smart tag di DropDownList. A questo punto, indicare che la proprietà `SelectedValue` deve essere associata al campo `CategoryID` mediante l'associazione dati bidirezionale (vedere la figura 10). Ripetere il processo dichiarativo o di progettazione per associare il campo dati `SupplierID` al `Suppliers` DropDownList.

[![associare CategoryID alla proprietà SelectedValue di DropDownList utilizzando l'associazione dati bidirezionale](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: associare il `CategoryID` alla proprietà `SelectedValue` di DropDownList usando l'associazione dati bidirezionale ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image30.png))

Una volta applicate le associazioni alle proprietà `SelectedValue` dei due DropDownList, le colonne Category e Supplier del prodotto modificato utilizzeranno per impostazione predefinita i valori del prodotto corrente. Quando si fa clic su Aggiorna, i valori `CategoryID` e `SupplierID` dell'elemento dell'elenco a discesa selezionato verranno passati al metodo `UpdateProduct`. La figura 11 Mostra l'esercitazione dopo l'aggiunta delle istruzioni DataBinding; Si noti che gli elementi dell'elenco a discesa selezionati per la stagione cajun di chef Anton sono correttamente condimento e New Orleans Cajun.

[![la categoria e i valori di fornitore correnti del prodotto modificato sono selezionati per impostazione predefinita](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: i valori di categoria e fornitore correnti del prodotto modificato sono selezionati per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image33.png))

## <a name="handlingnullvalues"></a>Gestione dei valori di`NULL`

È possibile `NULL`le colonne `CategoryID` e `SupplierID` della tabella `Products`, ma i controlli DropDownList del `EditItemTemplate` non includono un elemento elenco per rappresentare un valore di `NULL`. Questa operazione ha due conseguenze:

- L'utente non può utilizzare l'interfaccia per modificare la categoria o il fornitore di un prodotto da un valore non`NULL` a un `NULL`
- Se un prodotto ha un `NULL` `CategoryID` o `SupplierID`, se si fa clic sul pulsante modifica viene generata un'eccezione. Ciò è dovuto al fatto che il valore `NULL` restituito da `CategoryID` (o `SupplierID`) nell'istruzione `Bind()` non è mappato a un valore nell'oggetto DropDownList (il DropDownList genera un'eccezione quando la relativa proprietà `SelectedValue` è impostata su un valore *non* presente nella raccolta di elementi dell'elenco).

Per supportare `NULL` valori `CategoryID` e `SupplierID`, è necessario aggiungere un altro `ListItem` a ogni DropDownList per rappresentare il valore di `NULL`. Nell'esercitazione sull'applicazione di [filtri master/dettagli con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) è stato illustrato come aggiungere un `ListItem` aggiuntivo a una DropDownList con associazione a un oggetto, che ha richiesto l'impostazione della proprietà `AppendDataBoundItems` di dropdownlist su `True` e l'aggiunta manuale della `ListItem`aggiuntiva. Nell'esercitazione precedente, tuttavia, è stato aggiunto un `ListItem` con una `Value` di `-1`. La logica di associazione dati in ASP.NET, tuttavia, converte automaticamente una stringa vuota in un valore `NULL` e viceversa. Per questa esercitazione si vuole pertanto che la `Value` del `ListItem`sia una stringa vuota.

Per iniziare, impostare entrambi i controlli DropDownList ' `AppendDataBoundItems` proprietà su `True`. Aggiungere quindi il `NULL` `ListItem` aggiungendo il seguente elemento `<asp:ListItem>` a ogni DropDownList in modo che il markup dichiarativo sia simile al seguente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Si è scelto di usare "(nessuno)" come valore di testo per questo `ListItem`, ma è possibile modificarlo anche in una stringa vuota, se si vuole.

> [!NOTE]
> Come illustrato nell'esercitazione relativa al *filtro master/dettagli con un'esercitazione DropDownList* , è possibile aggiungere `ListItem` s a un oggetto DropDownList tramite la finestra di progettazione facendo clic sulla proprietà `Items` dell'oggetto dropdownlist nella finestra Proprietà (che visualizzerà l'editor della raccolta `ListItem`). Tuttavia, assicurarsi di aggiungere la `NULL` `ListItem` per questa esercitazione tramite la sintassi dichiarativa. Se si usa `ListItem` editor della raccolta, la sintassi dichiarativa generata ometterà completamente l'impostazione `Value` quando viene assegnata una stringa vuota, creando markup dichiarativo come: `<asp:ListItem>(None)</asp:ListItem>`. Sebbene ciò possa sembrare innocuo, il valore mancante fa in modo che il DropDownList usi il valore della proprietà `Text` al suo posto. Ciò significa che se si seleziona questa `NULL` `ListItem`, verrà tentata l'assegnazione del valore "(None)" al `CategoryID`, che genererà un'eccezione. Impostando in modo esplicito `Value=""`, viene assegnato un valore `NULL` a `CategoryID` quando viene selezionato il `ListItem` `NULL`.

Ripetere questi passaggi per la DropDownList Suppliers.

Con questa `ListItem`aggiuntiva, l'interfaccia di modifica ora può assegnare valori `NULL` ai campi di `CategoryID` e `SupplierID` di un prodotto, come illustrato nella figura 12.

[![scegliere (nessuno) per assegnare un valore NULL per la categoria o il fornitore di un prodotto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: scegliere (nessuno) per assegnare un valore `NULL` per la categoria o il fornitore di un prodotto ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image36.png))

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Passaggio 4: uso dei radiopulsanti per lo stato non più disponibile

Attualmente, il campo dati `Discontinued` dei prodotti viene espresso utilizzando un oggetto CheckBoxField, che esegue il rendering di una casella di controllo disabilitata per le righe di sola lettura e di una casella di controllo abilitata per la riga in fase di modifica. Sebbene questa interfaccia utente sia spesso adatta, è possibile personalizzarla se necessario usando un TemplateField. Per questa esercitazione, è possibile modificare l'oggetto CheckBoxField in un TemplateField che usa un controllo RadioButtonList con due opzioni "Active" e "Discontinued" da cui l'utente può specificare il valore `Discontinued` del prodotto.

Per iniziare, convertire il `Discontinued` CheckBoxField in un TemplateField, in modo da creare un TemplateField con una `ItemTemplate` e `EditItemTemplate`. Entrambi i modelli includono una casella di controllo con la relativa proprietà `Checked` associata al campo `Discontinued` dati, l'unica differenza tra i due modelli è che la proprietà `Enabled` della casella di controllo `ItemTemplate`è impostata su `False`.

Sostituire la casella di controllo nell'`ItemTemplate` e `EditItemTemplate` con un controllo RadioButtonList, impostando entrambe le proprietà di RadioButtonList ' `ID` su `DiscontinuedChoice`. Successivamente, indicare che RadioButtonList deve contenere due pulsanti di opzione, uno con etichetta "Active" con un valore "false" e l'altro con un valore "true". A tale scopo, è possibile immettere gli elementi `<asp:ListItem>` direttamente nella sintassi dichiarativa oppure utilizzare l'editor della raccolta `ListItem` dalla finestra di progettazione. La figura 13 Mostra l'editor della raccolta `ListItem` dopo aver specificato le due opzioni del pulsante di opzione.

[![aggiungere opzioni attive e Discontinued a RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: aggiungere opzioni attive e discontinue a RadioButtonList ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image39.png))

Poiché l'oggetto RadioButtonList nell'`ItemTemplate` non deve essere modificabile, impostare la relativa proprietà `Enabled` su `False`, lasciando la proprietà `Enabled` su `True` (impostazione predefinita) per l'oggetto RadioButtonList nell'`EditItemTemplate`. In questo modo i pulsanti di opzione nella riga non modificata sono di sola lettura, ma consente all'utente di modificare i valori di RadioButton per la riga modificata.

È comunque necessario assegnare i controlli RadioButtonList ' `SelectedValue` proprietà in modo che venga selezionato il pulsante di opzione appropriato in base al campo dati `Discontinued` del prodotto. Come per gli DropDownList esaminati in precedenza in questa esercitazione, questa sintassi di associazione dati può essere aggiunta direttamente al markup dichiarativo o tramite il collegamento Modifica DataBindings negli smart tag RadioButtonList.

Dopo l'aggiunta dei due RadioButtonList e la relativa configurazione, il markup dichiarativo di `Discontinued` TemplateField dovrebbe essere simile al seguente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Con queste modifiche, la colonna `Discontinued` è stata trasformata da un elenco di caselle di controllo a un elenco di coppie di pulsanti di opzione (vedere la figura 14). Quando si modifica un prodotto, viene selezionato il pulsante di opzione appropriato e lo stato del prodotto non più disponibile può essere aggiornato selezionando il pulsante di opzione altro e facendo clic su Aggiorna.

[![le caselle di controllo non più utilizzate sono state sostituite da coppie di pulsanti di opzione](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: le caselle di controllo obsolete sono state sostituite da coppie di pulsanti di opzione ([fare clic per visualizzare l'immagine con dimensioni complete](customizing-the-data-modification-interface-vb/_static/image42.png))

> [!NOTE]
> Poiché la colonna `Discontinued` nel database di `Products` non può contenere valori `NULL`, non è necessario preoccuparsi dell'acquisizione di informazioni `NULL` nell'interfaccia. Se, tuttavia, `Discontinued` colonna può contenere `NULL` valori, è necessario aggiungere un terzo pulsante di opzione all'elenco con la relativa `Value` impostata su una stringa vuota (`Value=""`), proprio come con le DropDownList della categoria e del fornitore.

## <a name="summary"></a>Riepilogo

Mentre i BoundField e CheckBoxField eseguono automaticamente il rendering delle interfacce di sola lettura, di modifica e di inserimento, non hanno la possibilità di personalizzazione. Spesso, tuttavia, è necessario personalizzare l'interfaccia di modifica o inserimento, ad esempio aggiungendo controlli di convalida, come illustrato nell'esercitazione precedente, oppure personalizzando l'interfaccia utente di raccolta dati (come illustrato in questa esercitazione). La personalizzazione dell'interfaccia con un TemplateField può essere riassunta nei passaggi seguenti:

1. Aggiungere un TemplateField o convertire un BoundField o un oggetto CheckBoxField esistente in un TemplateField
2. Aumentare l'interfaccia in base alle esigenze
3. Associare i campi dati appropriati ai controlli Web appena aggiunti usando l'associazione dati bidirezionale

Oltre a usare i controlli Web ASP.NET incorporati, è anche possibile personalizzare i modelli di un TemplateField con controlli server e controlli utente personalizzati e compilati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Successivo](implementing-optimistic-concurrency-vb.md)
