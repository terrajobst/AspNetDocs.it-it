---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Uso di TemplateFields nel controllo GridView (VB) | Microsoft Docs
author: rick-anderson
description: Per garantire la flessibilità, GridView offre TemplateField, che esegue il rendering usando un modello. Un modello può includere una combinazione di HTML statico, controlli Web e...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c090dbf65d9acbcc0e343cda5e8da7fff2d35d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531479"
---
# <a name="using-templatefields-in-the-gridview-control-vb"></a>Uso di TemplateFields nel controllo GridView (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) o [scaricare il file PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Per garantire la flessibilità, GridView offre TemplateField, che esegue il rendering usando un modello. Un modello può includere una combinazione di HTML statico, controlli Web e sintassi di associazione dati. In questa esercitazione verrà esaminato come usare TemplateField per ottenere un livello di personalizzazione maggiore con il controllo GridView.

## <a name="introduction"></a>Introduzione

GridView è costituito da un set di campi che indicano quali proprietà del `DataSource` devono essere incluse nell'output di cui viene eseguito il rendering insieme alla modalità di visualizzazione dei dati. Il tipo di campo più semplice è il BoundField, che visualizza un valore di dati come testo. Altri tipi di campo visualizzano i dati usando elementi HTML alternativi. CheckBoxField, ad esempio, esegue il rendering come casella di controllo il cui stato selezionato dipende dal valore di un campo dati specificato. ImageField esegue il rendering di un'immagine la cui origine dell'immagine è basata su un campo dati specificato. I collegamenti ipertestuali e i pulsanti il cui stato dipende da un valore del campo dati sottostante possono essere sottoposti a rendering usando i tipi di campo HyperLinkField e ButtonField.

Mentre i tipi di campo CheckBoxField, ImageField, HyperLinkField e ButtonField consentono una visualizzazione alternativa dei dati, sono comunque piuttosto limitati rispetto alla formattazione. Un oggetto CheckBoxField può visualizzare solo una singola casella di controllo, mentre un ImageField può visualizzare solo una singola immagine. Che cosa accade se un campo specifico deve visualizzare testo, una casella di controllo *e* un'immagine, tutti basati su valori di campo dati diversi? O se volessimo visualizzare i dati usando un controllo Web diverso da CheckBox, image, HyperLink o Button? Inoltre, il BoundField limita la visualizzazione a un singolo campo di dati. Cosa accade se si vuole mostrare due o più valori di campo dati in una singola colonna GridView?

Per supportare questo livello di flessibilità, GridView offre TemplateField, che esegue il rendering usando un *modello*. Un modello può includere una combinazione di HTML statico, controlli Web e sintassi di associazione dati. Inoltre, TemplateField dispone di un'ampia gamma di modelli che possono essere utilizzati per personalizzare il rendering per diverse situazioni. Per impostazione predefinita, ad esempio, la `ItemTemplate` viene utilizzata per eseguire il rendering della cella per ogni riga, ma il modello di `EditItemTemplate` può essere utilizzato per personalizzare l'interfaccia durante la modifica dei dati.

In questa esercitazione verrà esaminato come usare TemplateField per ottenere un livello di personalizzazione maggiore con il controllo GridView. Nell' [esercitazione precedente](custom-formatting-based-upon-data-vb.md) è stato illustrato come personalizzare la formattazione basata sui dati sottostanti usando i gestori eventi `DataBound` e `RowDataBound`. Un altro modo per personalizzare la formattazione basata sui dati sottostanti consiste nel chiamare i metodi di formattazione dall'interno di un modello. In questa esercitazione verrà esaminata anche questa tecnica.

Per questa esercitazione verrà usato TemplateFields per personalizzare l'aspetto di un elenco di dipendenti. In particolare, verranno elencati tutti i dipendenti, ma verranno visualizzati il nome e il cognome del dipendente in una colonna, la data di assunzione in un controllo calendario e una colonna stato che indica il numero di giorni in cui sono stati utilizzati nell'azienda.

[per personalizzare la visualizzazione vengono usati ![tre TemplateFields](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Figura 1**: tre TemplateFields vengono usati per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Passaggio 1: associazione dei dati al controllo GridView

Negli scenari di creazione di report in cui è necessario usare TemplateFields per personalizzare l'aspetto, mi risulta più semplice iniziare creando un controllo GridView che contiene prima di tutto i BoundField e poi aggiungere nuovi TemplateFields o convertire i BoundField esistenti in TemplateFields in base alle esigenze. Per iniziare questa esercitazione, è quindi necessario aggiungere un controllo GridView alla pagina tramite la finestra di progettazione e associarlo a un ObjectDataSource che restituisce l'elenco di dipendenti. Con questi passaggi viene creato un controllo GridView con BoundField per ognuno dei campi Employee.

Aprire la pagina `GridViewTemplateField.aspx` e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Dallo smart tag di GridView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama il metodo `GetEmployees()` della classe `EmployeesBLL`.

[![aggiungere un nuovo controllo ObjectDataSource che richiama il metodo GetEmployees ()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource che richiama il metodo `GetEmployees()` ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image6.png))

Se si associa GridView in questo modo, verrà automaticamente aggiunto un BoundField per ogni proprietà Employee: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`e `Country`. Per questo report non è preferibile visualizzare le proprietà `EmployeeID`, `ReportsTo`o `Country`. Per rimuovere questi BoundField è possibile:

- Utilizzare la finestra di dialogo campi fare clic sul collegamento Modifica colonne dallo smart tag di GridView per visualizzare questa finestra di dialogo. Selezionare quindi i BoundField nell'elenco in basso a sinistra e fare clic sul pulsante rosso X per rimuovere il BoundField.
- Modificare manualmente la sintassi dichiarativa di GridView dalla visualizzazione origine, eliminare l'elemento `<asp:BoundField>` per il BoundField che si vuole rimuovere.

Dopo aver rimosso i BoundField `EmployeeID`, `ReportsTo`e `Country`, il markup di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Esaminare lo stato di avanzamento in un browser. A questo punto dovrebbe essere visualizzata una tabella con un record per ogni dipendente e quattro colonne: una per il cognome del dipendente, una per il nome, una per il titolo e una per la data di assunzione.

[![vengono visualizzati i campi LastName, FirstName, title e Hiret per ogni dipendente](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Figura 3**: vengono visualizzati i campi `LastName`, `FirstName`, `Title`e `HireDate` per ogni dipendente ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Passaggio 2: visualizzazione del nome e del cognome in una singola colonna

Attualmente, i nomi e i cognomi di ogni dipendente vengono visualizzati in una colonna separata. In alternativa, potrebbe essere utile combinarli in un'unica colonna. A tale scopo, è necessario usare un TemplateField. È possibile aggiungere un nuovo TemplateField, aggiungervi il markup e la sintassi di associazione dati necessari, quindi eliminare il `FirstName` e `LastName` i BoundField, oppure è possibile convertire il BoundField `FirstName` in un TemplateField, modificare TemplateField in modo da includere il valore di `LastName`, quindi rimuovere il `LastName` BoundField.

Entrambi si avvicinano alla rete lo stesso risultato, ma personalmente preferisco convertire i BoundField in TemplateFields, quando possibile, perché la conversione aggiunge automaticamente una `ItemTemplate` e `EditItemTemplate` con i controlli Web e la sintassi di associazione dati per simulare l'aspetto e la funzionalità del BoundField. Il vantaggio è che sarà necessario eseguire meno lavoro con la TemplateField, perché il processo di conversione eseguirà alcune operazioni per noi.

Per convertire un BoundField esistente in un TemplateField, fare clic sul collegamento Modifica colonne dallo smart tag di GridView e visualizzata la finestra di dialogo campi. Selezionare il BoundField da convertire dall'elenco nell'angolo in basso a sinistra e quindi fare clic sul collegamento "Converti questo campo in un TemplateField" nell'angolo in basso a destra.

[![convertire un BoundField in un TemplateField dalla finestra di dialogo campi](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Figura 4**: convertire un BoundField in un TemplateField dalla finestra di dialogo campi ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image12.png))

Andare avanti e convertire il `FirstName` BoundField in un TemplateField. Dopo questa modifica non esiste alcuna differenza percettiva nella finestra di progettazione. Questo perché la conversione del BoundField in un TemplateField crea un TemplateField che mantiene l'aspetto di BoundField. Sebbene non esistano differenze visive in questa fase della finestra di progettazione, questo processo di conversione ha sostituito la sintassi dichiarativa del BoundField `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`-con la sintassi TemplateField seguente:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Come si può notare, TemplateField è costituito da due modelli, un `ItemTemplate` con un'etichetta la cui proprietà `Text` è impostata sul valore del campo `FirstName` data e un `EditItemTemplate` con un controllo TextBox la cui proprietà `Text` è impostata anche sul campo `FirstName` data. La sintassi di associazione dati-`<%# Bind("fieldName") %>`-indica che il campo dati *`fieldName`* è associato alla proprietà del controllo Web specificata.

Per aggiungere il valore del campo dati `LastName` a questo TemplateField, è necessario aggiungere un altro controllo Web etichetta nel `ItemTemplate` e associare la relativa proprietà `Text` a `LastName`. Questa operazione può essere eseguita manualmente o tramite la finestra di progettazione. Per eseguire questa operazione manualmente, è sufficiente aggiungere la sintassi dichiarativa appropriata al `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Per aggiungerlo tramite la finestra di progettazione, fare clic sul collegamento modifica modelli dallo smart tag di GridView. Verrà visualizzata l'interfaccia di modifica dei modelli di GridView. Lo smart tag di questa interfaccia è un elenco dei modelli in GridView. Poiché a questo punto è disponibile un solo TemplateField, gli unici modelli elencati nell'elenco a discesa sono i modelli per il `FirstName` TemplateField insieme al `EmptyDataTemplate` e `PagerTemplate`. Il modello di `EmptyDataTemplate`, se specificato, viene utilizzato per eseguire il rendering dell'output di GridView se non sono presenti risultati nei dati associati a GridView; il `PagerTemplate`, se specificato, viene usato per eseguire il rendering dell'interfaccia di paging per un controllo GridView che supporta il paging.

[![i modelli di GridView possono essere modificati tramite la finestra di progettazione](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Figura 5**: i modelli di GridView possono essere modificati tramite la finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image15.png))

Per visualizzare anche il `LastName` nel `FirstName` TemplateField trascinare il controllo Label dalla casella degli strumenti nel `ItemTemplate` `FirstName` di TemplateField nell'interfaccia di modifica dei modelli di GridView.

[![aggiungere un controllo Web Label all'ItemTemplate di FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Figura 6**: aggiungere un controllo Web Label alla `FirstName` ItemTemplate di TemplateField ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image18.png))

A questo punto il controllo Web Label aggiunto a TemplateField ha la proprietà `Text` impostata su "label". È necessario modificare questa proprietà in modo che questa proprietà sia associata al valore del campo dati `LastName`. Per eseguire questa operazione, fare clic sullo smart tag del controllo Label e scegliere l'opzione Modifica DataBindings.

[![scegliere l'opzione Modifica DataBindings dallo smart tag dell'etichetta](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Figura 7**: scegliere l'opzione Modifica DataBindings dallo smart tag dell'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image21.png))

Verrà visualizzata la finestra di dialogo DataBindings. Da qui è possibile selezionare la proprietà per partecipare all'associazione dati dall'elenco a sinistra e scegliere il campo a cui associare i dati dall'elenco a discesa a destra. Scegliere la proprietà `Text` a sinistra e il campo `LastName` a destra e fare clic su OK.

[![associare la proprietà Text al campo dati LastName](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Figura 8**: associare la proprietà `Text` al campo dati `LastName` ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image24.png))

> [!NOTE]
> La finestra di dialogo DataBindings consente di indicare se eseguire l'associazione dati bidirezionale. Se si lascia deselezionata questa opzione, viene utilizzata la sintassi di associazione dati `<%# Eval("LastName")%>` al posto di `<%# Bind("LastName")%>`. Uno degli approcci è adatto a questa esercitazione. L'associazione dati bidirezionale diventa importante quando si inseriscono e si modificano i dati. Per visualizzare semplicemente i dati, tuttavia, entrambi gli approcci funzioneranno ugualmente correttamente. Nelle esercitazioni future verranno illustrate in dettaglio l'associazione dati bidirezionale.

Per visualizzare questa pagina, è possibile usare un browser. Come si può notare, GridView include ancora quattro colonne; Tuttavia, nella colonna `FirstName` sono ora *elencati i valori dei campi* dati `FirstName` e `LastName`.

[![i valori FirstName e LastName sono visualizzati in una singola colonna](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Figura 9**: i valori `FirstName` e `LastName` sono visualizzati in una singola colonna ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image27.png))

Per completare il primo passaggio, rimuovere il `LastName` BoundField e rinominare la proprietà `HeaderText` di `FirstName` TemplateField in "nome". Una volta apportate queste modifiche, il markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]

[![il nome e il cognome di ogni dipendente vengono visualizzati in una colonna](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Figura 10**: il nome e il cognome di ogni dipendente vengono visualizzati in una colonna ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Passaggio 3: uso del controllo Calendar per visualizzare il campo`HiredDate`

La visualizzazione di un valore di campo dati come testo in GridView è semplice come l'uso di un BoundField. Per determinati scenari, tuttavia, i dati vengono espressi meglio utilizzando un particolare controllo Web anziché solo il testo. Con TemplateFields è possibile personalizzare la visualizzazione dei dati. Ad esempio, invece di visualizzare la data di assunzione del dipendente come testo, è possibile visualizzare un calendario (usando [il controllo calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con la data di assunzione evidenziata.

A tale scopo, iniziare convertendo il `HiredDate` BoundField in un TemplateField. È sufficiente passare allo smart tag di GridView e fare clic sul collegamento Modifica colonne, visualizzando la finestra di dialogo campi. Selezionare il BoundField `HiredDate` e fare clic su "Converti questo campo in un TemplateField".

[![convertire il BoundField HiredDate in TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Figura 11**: convertire il BoundField `HiredDate` in un TemplateField ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image33.png))

Come illustrato nel passaggio 2, questo sostituirà il BoundField con una TemplateField che contiene un `ItemTemplate` e `EditItemTemplate` con un'etichetta e una casella di testo le cui proprietà `Text` sono associate al valore `HiredDate` usando la sintassi DataBinding `<%# Bind("HiredDate")%>`.

Per sostituire il testo con un controllo Calendar, modificare il modello rimuovendo l'etichetta e aggiungendo un controllo Calendar. Dalla finestra di progettazione selezionare modifica modelli dallo smart tag di GridView e scegliere il `ItemTemplate` `HireDate` TemplateField dall'elenco a discesa. Eliminare quindi il controllo Label e trascinare un controllo Calendar dalla casella degli strumenti all'interfaccia di modifica del modello.

[![aggiungere un controllo Calendar all'ItemTemplate del TemplateField di assunzione](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Figura 12**: aggiungere un controllo Calendar al `ItemTemplate` di `HireDate` TemplateField ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image36.png))

A questo punto ogni riga in GridView conterrà un controllo Calendar nel relativo `HiredDate` TemplateField. Tuttavia, il valore effettivo di `HiredDate` del dipendente non viene impostato in alcun punto del controllo Calendar, causando la visualizzazione predefinita di ogni controllo Calendar per visualizzare il mese e la data correnti. Per risolvere questo problema, è necessario assegnare `HiredDate` di ogni dipendente alle proprietà [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) del controllo Calendar.

Dallo smart tag del controllo calendario scegliere modifica DataBinding. Quindi, associare le proprietà `SelectedDate` e `VisibleDate` al campo dati `HiredDate`.

[![associare le proprietà SelectedDate e VisibleDate al campo dati HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Figura 13**: associare le proprietà `SelectedDate` e `VisibleDate` al campo dati `HiredDate` ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image39.png))

> [!NOTE]
> La data selezionata del controllo calendario non deve necessariamente essere visibile. Ad esempio, un calendario potrebbe avere il 1 ° agosto<sup>St</sup>, 1999 come data selezionata, ma mostrare il mese e l'anno correnti. La data selezionata e la data visibile sono specificate dalle proprietà `SelectedDate` e `VisibleDate` del controllo Calendar. Poiché si desidera selezionare la `HiredDate` del dipendente e assicurarsi che sia indicato che è necessario associare entrambe le proprietà al campo dati `HireDate`.

Quando si visualizza la pagina in un browser, il calendario Mostra ora il mese della data Assunta del dipendente e seleziona tale data specifica.

[![il HiredDate del dipendente viene visualizzato nel controllo calendario](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Figura 14**: il `HiredDate` del dipendente viene visualizzato nel controllo calendario ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image42.png))

> [!NOTE]
> Contrariamente a tutti gli esempi che abbiamo visto finora, per questa esercitazione *non* è stato impostato `EnableViewState` proprietà su `False` per questo GridView. Il motivo di questa decisione è dovuto al fatto che facendo clic sulle date del controllo calendario viene generato un postback, impostando la data selezionata del calendario sulla data appena selezionata. Se lo stato di visualizzazione di GridView è disabilitato, tuttavia, a ogni postback i dati di GridView vengono riassociati all'origine dati sottostante, che fa sì che la data selezionata del calendario venga *reimpostata* sul `HireDate`del dipendente, sovrascrivendo la data scelta dall'utente.

Per questa esercitazione si tratta di una discussione discutibile, perché l'utente non è in grado di aggiornare la `HireDate`del dipendente. È consigliabile configurare il controllo Calendar in modo che le date non siano selezionabili. Indipendentemente dall'esercitazione, in questa esercitazione viene illustrato che in alcune circostanze lo stato di visualizzazione deve essere abilitato per fornire determinate funzionalità.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Passaggio 4: visualizzazione del numero di giorni per cui il dipendente ha lavorato per la società

Fino a questo punto abbiamo visto due applicazioni di TemplateFields:

- Combinare due o più valori di campo dati in una sola colonna e
- Espressione di un valore del campo dati utilizzando un controllo Web anziché un testo

Un terzo uso di TemplateFields è la visualizzazione dei metadati sui dati sottostanti di GridView. Oltre a visualizzare le date di assunzione dei dipendenti, è possibile, ad esempio, che si desideri includere una colonna che Visualizza il numero di giorni totali in cui si è verificato il processo.

Un altro uso di TemplateFields si verifica negli scenari in cui i dati sottostanti devono essere visualizzati in modo diverso nel report della pagina Web rispetto al formato archiviato nel database. Si supponga che la tabella `Employees` disponga di un campo `Gender` in cui è stato archiviato il carattere `M` o `F` per indicare il sesso del dipendente. Quando si visualizzano queste informazioni in una pagina Web, è possibile che si desideri mostrare il genere come "maschio" o "femmina", anziché solo "M" o "F".

Entrambi gli scenari possono essere gestiti creando un metodo di *formattazione* nella classe code-behind della pagina ASP.NET (o in una libreria di classi separata, implementata come metodo di `Shared`) richiamato dal modello. Un metodo di formattazione di questo tipo viene richiamato dal modello utilizzando la stessa sintassi di associazione dati visualizzata in precedenza. Il metodo di formattazione può assumere un numero qualsiasi di parametri, ma deve restituire una stringa. Questa stringa restituita è il codice HTML inserito nel modello.

Per illustrare questo concetto, è possibile aumentare l'esercitazione per visualizzare una colonna in cui è elencato il numero totale di giorni per cui un dipendente si trova nel processo. Questo metodo di formattazione accetta un oggetto `Northwind.EmployeesRow` e restituisce il numero di giorni per cui il dipendente è stato utilizzato come stringa. Questo metodo può essere aggiunto alla classe code-behind della pagina ASP.NET, ma *deve* essere contrassegnato come `Protected` o `Public` per essere accessibile dal modello.

[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Poiché il campo `HiredDate` può contenere `NULL` valori del database, prima di procedere con il calcolo è necessario assicurarsi che il valore non sia `NULL`. Se il valore `HiredDate` è `NULL`, è sufficiente restituire la stringa "Unknown"; Se non è `NULL`, viene calcolata la differenza tra l'ora corrente e il valore di `HiredDate` e viene restituito il numero di giorni.

Per utilizzare questo metodo, è necessario richiamarlo da un TemplateField in GridView utilizzando la sintassi di associazione dati. Per iniziare, aggiungere un nuovo TemplateField a GridView facendo clic sul collegamento Modifica colonne nello smart tag di GridView e aggiungendo un nuovo TemplateField.

[![aggiungere un nuovo TemplateField a GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Figura 15**: aggiungere un nuovo TemplateField al GridView ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image45.png))

Impostare questa nuova proprietà `HeaderText` di TemplateField su "giorni nel processo" e la relativa proprietà `HorizontalAlign` della `ItemStyle`su `Center`. Per chiamare il metodo `DisplayDaysOnJob` dal modello, aggiungere una `ItemTemplate` e usare la sintassi di associazione dati seguente:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` restituisce un oggetto `DataRowView` corrispondente al record `DataSource` associato al `GridViewRow`. La relativa proprietà `Row` restituisce la `Northwind.EmployeesRow`fortemente tipizzata, che viene passata al metodo `DisplayDaysOnJob`. Questa sintassi di associazione dati può essere visualizzata direttamente nel `ItemTemplate` (come illustrato nella sintassi dichiarativa riportata di seguito) oppure può essere assegnata alla proprietà `Text` di un controllo Web Label.

> [!NOTE]
> In alternativa, anziché passare un'istanza di `EmployeesRow`, è possibile passare il valore di `HireDate` utilizzando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Tuttavia, il metodo `Eval` restituisce un `Object`, quindi è necessario modificare la firma del metodo di `DisplayDaysOnJob` in modo che accetti un parametro di input di tipo `Object`. Non è possibile eseguire il cast cieco della chiamata `Eval("HireDate")` a un `DateTime` perché la colonna `HireDate` nella tabella `Employees` può contenere valori di `NULL`. Pertanto, è necessario accettare un `Object` come parametro di input per il metodo `DisplayDaysOnJob`, verificare se dispone di un valore di `NULL` di database (che può essere ottenuto utilizzando `Convert.IsDBNull(objectToCheck)`), quindi procedere di conseguenza.

A causa di queste sottigliezze, ho deciso di passare l'intera istanza di `EmployeesRow`. Nell'esercitazione successiva verrà illustrato un esempio più appropriato per l'uso della sintassi `Eval("columnName")` per passare un parametro di input in un metodo di formattazione.

Di seguito viene illustrata la sintassi dichiarativa per GridView dopo l'aggiunta di TemplateField e il metodo `DisplayDaysOnJob` chiamato dall'`ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Nella figura 16 viene illustrata l'esercitazione completata, se visualizzata tramite un browser.

[![il numero di giorni di visualizzazione del dipendente sul processo](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Figura 16**: numero di giorni di visualizzazione del dipendente nel processo ([fare clic per visualizzare l'immagine con dimensioni complete](using-templatefields-in-the-gridview-control-vb/_static/image48.png))

## <a name="summary"></a>Riepilogo

TemplateField nel controllo GridView consente un livello superiore di flessibilità nella visualizzazione dei dati rispetto a quello disponibile con gli altri controlli campo. TemplateFields sono ideali per le situazioni in cui:

- È necessario visualizzare più campi dati in una colonna GridView
- I dati vengono espressi in modo ottimale usando un controllo Web anziché testo normale
- L'output dipende dai dati sottostanti, ad esempio la visualizzazione dei metadati o la riformattazione dei dati

Oltre a personalizzare la visualizzazione dei dati, TemplateFields vengono usati anche per personalizzare le interfacce utente usate per la modifica e l'inserimento dei dati, come si vedrà nelle esercitazioni future.

Le due esercitazioni successive continuano a esplorare i modelli, iniziando con un'occhiata all'uso di TemplateFields in un DetailsView. Successivamente, si passerà a FormView, che usa i modelli al posto dei campi per offrire maggiore flessibilità nel layout e nella struttura dei dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Dan Jagers. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](custom-formatting-based-upon-data-vb.md)
> [Successivo](using-templatefields-in-the-detailsview-control-vb.md)
